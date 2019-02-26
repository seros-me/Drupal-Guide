****GUIA DRUPAL****


**Migas**

 /*
   *   Opción A  Migas desde la URL
   */
  /*
  //$host = \Drupal::request()->getHost();
  $nohost = \Drupal::request()->getRequestUri();
  $string = explode("/", $nohost);
  $length = count($string);
  $string[$length - 1] = str_replace("-"," ", $string[$length - 1] );
  $string[$length - 2] = str_replace("-"," ", $string[$length - 2] );
  $vars['miga_final'] = ucfirst($string[$length - 1]) ;
  $vars['miga_penultima'] = ucfirst($string[$length - 2]);
  */

  //Sacamos las migas desde el arbol del Menú (para solucionar problemas de ortografía) comparamos con la URL.
  //Falta controlar que dos secciones calendario no sepa quien es el padre

  /*
   *   Opción B
   */
  /*
  $tree = \Drupal::menuTree()->load('main', new \Drupal\Core\Menu\MenuTreeParameters());
  $x = "";
  foreach ($tree as $item) {
    $subtree = $item->subtree;
    if ($subtree != NULL) {
      foreach ($subtree as $subitem) {
        $aux = $subitem->link->getPluginDefinition()['title'];
        //Quito las tildes
        $str_sin_tildes = eliminar_tildes($aux);
        if (strcasecmp($str_sin_tildes, $vars['miga_final']) === 0) {
       // if (strcasecmp($subitem->link->getPluginDefinition()['title'], $vars['miga_final']) === 0) {
          $vars['miga_final'] = $subitem->link->getPluginDefinition()['title'];
          $vars['miga_penultima'] = $item->link->getPluginDefinition()['title'];
          $x = '' ;
        }
      }
    }
  }
  */

  /*
   * Opción C  Con Active Trail getCurrentRouteMenuTreeParameters
   */

  //$menuids = \Drupal::service('menu.active_trail')->getActiveTrailIds('main');
  $menulink = \Drupal::service('menu.active_trail')->getActiveLink();
  $x = '';
  /*if($menulink == null){
    $menulink = \Drupal::service('menu.active_trail')->getActiveLink('footer');
  }*/
  if($menulink != ""){
    if($menulink->getParent()) {
      $parent_link = Drupal::service('plugin.manager.menu.link')->createInstance($menulink->getParent());
      $vars['miga_penultima'] =  $parent_link->getPluginDefinition()['title'];
    }
  }

  if ($menulink != "" ){
    if (!empty($menulink->getPluginDefinition()['title'])){
      $vars['miga_final'] = $menulink->getPluginDefinition()['title'];
    }
  }
