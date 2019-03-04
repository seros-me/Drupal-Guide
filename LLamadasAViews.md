

llamadas a Views desde diferentes lugares:



```php
// Get the view machine id.
$view = \Drupal\views\Views::getView('my_view_machine_id');
// Set the display machine id.
$view->setDisplay('block_1');
// Get the title.
$title = $view->getTitle();
// Render.
$render = $view->render();

$the_title_render_array = [
  '#markup' => t('@title', ['@title'=> $title]),
  '#allowed_tags' => ['h2'],
];

$variables['content']['view_title'] = $the_title_render_array;
// Or $variables['content']['view_title']['#prefix'] = $title;.
$variables['content']['view_output'] = $render;
```

```php
$view = \Drupal\views\Views::getView('documentacion_year');
  $view->setDisplay('page_1');
  //$view->setArguments(array('arg1'));
  $view->setItemsPerPage(0);
  //$view->setOffset(0);
  //$view->usePager();
  $view->execute();
  //$view->serialize();
  $result = $view->result;

  foreach ($result as $key => $value) {
    $entity = $value->_entity;
    $title = $entity->get('title')->getValue()[0]['value'];
    $custom_field	= $entity->get('field_custom')->getValue();
  }
  
```
```php
``````php
``````php
``````php
``````php
``````php
``````php
``````php
``````php
``````php
``````php
```
