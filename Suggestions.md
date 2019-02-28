**Sugestions para poder optar por un twig u otro

´´´´php
function fetri_migas_theme_suggestions_page_alter(array &$suggestions,array &$variables) {

  $x= "";
  if ( $variables['elements']['#node']->getType() == 'noticia' && ! \Drupal::service('path.matcher')->isFrontPage()) {
    $suggestions[] = 'page__node__noticia';
  }

  return $suggestions;
}
´´´´
