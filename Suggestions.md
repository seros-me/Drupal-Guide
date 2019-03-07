
**Suggestion para página principal**
```php
/**
 * Alter theme in node  suggestion node__page__home
 */

function fetri_home_theme_suggestions_node_alter(array &$suggestions,array &$variables) {

  if ( $variables['elements']['#node']->getType() == 'page' && \Drupal::service('path.matcher')->isFrontPage()) {
    $suggestions[] = 'node__page__home';
  }

  return $suggestions;
}

/**
 * Implements hook_preprocess().
 * Get variables to node--page--home.html.twig.
 */
function fetri_home_preprocess_node__page__home(&$variables) {

  //var_dump('enttro  fetri_home_preprocess_node__page__home'); die();

  $node = $variables['elements']['#node'];
  if ($node->getType() == 'page' && \Drupal::service('path.matcher')->isFrontPage()){
    $view = Views::getView('slider_home');
    if (is_object($view))
    {
      $exclude = [];
      //$view->setArguments($args);
      $view->setDisplay('block_1');
      $view->preExecute();
      $view->execute();
      foreach ($view->result as $result) {
        $type = Node::load($result->nid)->getType();
        if ($type == 'noticia'){
          $exclude[] = $result->nid;
        }
      }
      $variables['slider'] = $view->buildRenderable();
    }

    if (is_array($exclude))
    {
      $arguments = implode (", ", $exclude);
      $noticias = Views::getView('noticias');
      $noticias->setArguments([$arguments]);
      $noticias->setDisplay('block_1');
      $noticias->preExecute();
      $noticias->execute();
      $variables['noticias'] = $noticias->buildRenderable();
    }
  }

}
```


**Sugestions para poder optar por un twig u otro**

https://befused.com/drupal/page-template-content-type

```php

/**
 * Implements hook_theme_suggestions_page_alter().
 */
function example_theme_suggestions_page_alter(array &$suggestions, array $variables) {
  if ($node = \Drupal::routeMatch()->getParameter('node')) {
    $suggestions[] = 'page__' . $node->bundle(); 
    //->bundle()   NOs da el tipo de nodo que es, Noticia, Documento, ...
  }
}





function fetri_migas_theme_suggestions_page_alter(array &$suggestions,array &$variables) {

  $x= "";
  if ( $variables['elements']['#node']->getType() == 'noticia' && ! \Drupal::service('path.matcher')->isFrontPage()) {
    $suggestions[] = 'page__node__noticia';
  }

  return $suggestions;
}
```
