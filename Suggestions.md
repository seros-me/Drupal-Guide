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
