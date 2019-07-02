
**Las partes necesarias**  
1. Tendremos un modulo en el que iremos añadieno el codigo.
2. Tendremos dentro de la carpeta src/Form tendremos el formulario y lo que sucederá si se modifica alguno de estos filtros.
3. Tendremos un Contoller dentro de src que sera el encargado de crear el form BikoCustomModuleContrroller.php. En este caso desde el Routing se engancha con la función getAgenda().






**BikoCustomModuleContrroller.php en la ruta biko_custom_module/src/Controller/BikoCustomModuleContrroller.php**  
```php
function getAgenda(){
    $fecha = date('d/m/Y');
    $fecha = \Drupal::request()->query->get('date');
    $ciclo = \Drupal::request()->query->get('ciclo');
    $lugar = \Drupal::request()->query->get('lugar');
    $ajax_form_request = \Drupal::request()->query->has(FormBuilderInterface::AJAX_FORM_REQUEST);
    if (! $ajax_form_request) {
      $conectionService = \Drupal::service('conectionservice.service');
      $resource = 'eventos';
      $schema = '';
      $apiResponse = $conectionService->getEventos($resource, $schema);
      //$apiResponse = $this->getJsonAgendaEventos();
    }
    $form = \Drupal::formBuilder()->getForm('\Drupal\esmrs_agenda_eventos\Form\SettingsForm');

    return [
      '#theme' => 'esmrs_agenda',
      '#content' => $apiResponse,
      '#form' => $form,
      '#attached' => [
        'library' => [
          'esmrs_agenda_eventos/esmrs_agenda_eventos.prueba'
        ],
      ],
      '#cache' => ['contexts' => ['url.path', 'url.query_args']],

    ];
  }
```

**ArchivoForm.php en la ruta biko_custom_module/src/Form/ArchivoForm.php**  
```php
<?php

namespace Drupal\esmrs_agenda_eventos\Form;

use Drupal\Core\Form\ConfigFormBase;
use Drupal\Core\Form\FormBuilderInterface;
use Drupal\Core\Form\FormStateInterface;
use Drupal\Core\Ajax\AjaxResponse;
use Drupal\Core\Ajax\HtmlCommand;
use Drupal\Core\Datetime\DrupalDateTime;
use Drupal\esmrs_api;


/**
 * Configure fetri_atletas settings for this site.
 */
class SettingsForm extends ConfigFormBase {

  /**
   * {@inheritdoc}
   */
  public function getFormId() {
    return 'esmrs_agenda_eventos_settings';
  }
  public function setMessage(array $form, FormStateInterface $form_state) {
    // variables

    $fecha = $form_state->getValue('date');
    $ciclo = $form_state->getValue('ciclo');
    $lugar = $form_state->getValue('lugar');

    $url =

    //
    $response = new AjaxResponse();
    $response->addCommand(
      new HtmlCommand(
        '.result_message',
        '<div class="my_top_message">The result is ' . t('The results is ') . ($form_state->getValue('number_1') + $form_state->getValue('number_2')) . '</div>')
    );
    return $response;

  }
  /**
   * {@inheritdoc}
   */
  protected function getEditableConfigNames() {
    return ['esmrs_agenda_eventos.settings'];
  }

  /**
   * {@inheritdoc}
   */
  public function buildForm(array $form, FormStateInterface $form_state) {
    //Podriamos hacer la conexión a la API desde aquí para consumir los recursos necesarios Ciclo y Lugares.
    //$conectionService = \Drupal::service('conectionservice.service');
    //$ciclos = $conectionService->getRecurso('ciclos');
    //$lugares = $conectionService->getRecurso('lugares');

    $ajax_form_request = \Drupal::request()->query->has(FormBuilderInterface::AJAX_FORM_REQUEST);
    if (! $ajax_form_request){
      $conectionService = \Drupal::service('conectionservice.service');
      $schema = '';
      $ciclos = $conectionService->getRecurso('ciclos',$schema);
      $lugares = $conectionService->getRecurso('lugares',$schema);

      // $options_ciclo = $optons_lugar = Array();
      $options_ciclo = ['all' => 'CICLO'];
      $options_lugar = ['all' => 'LUGAR'];
      foreach ($ciclos->body as $ciclo){
        $options_ciclo[$ciclo->IdCiclo] = $ciclo->LabelCiclo;
      }
      foreach ($lugares->body as $lugar){
        $options_lugar[$lugar->IdLugar] = $lugar->LabelLugar;
      }
    }

    $form['date'] = array(
      '#type' => 'date',
      '#required' => TRUE,
      '#default_value' => date('Y-m-d'),
      '#format' => 'd/m/Y',
      '#ajax' => array(
        'callback' => '::alterDate',
        'effect' => 'fade',
        'event' => 'change',
        'progress' => array(
          'type' => 'throbber',
          'message' => NULL,
        ),
      ),
    );

    $form['ciclo'] = [
      '#type' => 'select',
      //array asociativo $ciclo con los datos del recurso
      '#options' => $options_ciclo,
      '#size' => NULL,
      '#default_value' => 'all',
      '#ajax' => array(
        'callback' => '::alterCiclo',
        'effect' => 'fade',
        'event' => 'change',
        'progress' => array(
          'type' => 'throbber',
          'message' => NULL,
        ),
      ),

    ];

    $form['lugar'] = [
      '#type' => 'select',
      '#options' => $options_lugar,
      '#size' => NULL,
      '#default_value' => 'all',
      '#ajax' => array(
        'callback' => '::alterLugar',
        'effect' => 'fade',
        'event' => 'change',
        'progress' => array(
          'type' => 'throbber',
          'message' => NULL,
        ),
      ),
    ];

    return $form;
  }

  /**
   * {@inheritdoc}
   */
  public function validateForm(array &$form, FormStateInterface $form_state) {
    if ($form_state->getValue('example') != 'example') {
      $form_state->setErrorByName('example', $this->t('The value is not correct.'));
    }
    parent::validateForm($form, $form_state);
  }

  /**
   * {@inheritdoc}
   */
  public function submitForm(array &$form, FormStateInterface $form_state) {
    $this->config('esmrs_agenda_eventos.settings')
      ->set('example', $form_state->getValue('example'))
      ->save();
    parent::submitForm($form, $form_state);
  }


  //  Pruebas de Sergio, funciones que actuan en el caso de modificacion del formulario
  public function alterDate(array $form, FormStateInterface $form_state) {
    $ajax_response = new AjaxResponse();

      $text = 'Cambio en fecha';

    $ajax_response->addCommand(new HtmlCommand('#micontenedor', $text));
    return $ajax_response;
  }
  public function alterCiclo(array $form, FormStateInterface $form_state) {
    $ajax_response = new AjaxResponse();
    $text = 'Cambio en ciclo';

    $conectionService = \Drupal::service('conectionservice.service');
    $resource ='eventos';
    $schema = $this->schemaUrl($form_state);
    $apiResponse = $conectionService->getEventos($resource,$schema);

    $ajax_response->addCommand(new HtmlCommand('#micontenedor', $text));
    return $ajax_response;
  }
  public function alterLugar(array $form, FormStateInterface $form_state) {
    $ajax_response = new AjaxResponse();

    $text = 'Cambio en lugar';
    // LLamo a la API para conseguir los nuevos resultados tras realizar el Filtrado.
    $conectionService = \Drupal::service('conectionservice.service');
    $resource ='eventos';
    $schema = $this->schemaUrl($form_state);
    //$apiResponse = $conectionService->getApi($resource,$schema);

    $ajax_response->addCommand(new HtmlCommand('#micontenedor', $text));
    return $ajax_response;
  }
  
  public function schemaUrl($form_state) {
    $schemaUrl = 'fecha=' . $form_state->getValue('date') . '&lugar=' . $form_state->getValue('lugar') . '&ciclo=' . $form_state->getValue('ciclo');

    return $schemaUrl;
  }


}

```



