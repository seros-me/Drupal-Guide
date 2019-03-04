#Como conseguir la extensión de un tipo de archivo#



*En el Preprocess que corresponda

´´´ [php]
 //Para pintar el icono, sacamos la extensión.
  $file = \Drupal\file\Entity\File::load($media_field['target_id']);
  //$imageSRC = file_create_url($file->getFileUri());
  $extension = $file->getMimeType();
  $vars['extension'] = mimeType($extension);

´´´
* la función a la que llamo
´´´ php
function mimeType($extension){
  $ext_limpia = 'alt';
  switch ($extension)
  {
    //Pdf
    case 'application/pdf':
      $ext_limpia = 'pdf';
      break;
    //Word doc, docx, odt
    case 'application/vnd.openxmlformats-officedocument.wordprocessingml.document':
      $ext_limpia = 'word';
      break;
    case 'application/vnd.oasis.opendocument.text':
      $ext_limpia = 'word';
      break;
    case 'application/msword':
      $ext_limpia = 'word';
      break;
    //Excel xlsx, xls, ods
    case 'application/vnd.oasis.opendocument.spreadsheet':
      $ext_limpia = 'excel';
      break;
    case 'application/vnd.openxmlformats-officedocument.spreadsheetml.sheet':
      $ext_limpia = 'excel';
      break;
    case 'application/vnd.ms-excel':
      $ext_limpia = 'excel';
      break;
    //PowerPoint pps, ppsx, odp
    case 'application/vnd.oasis.opendocument.presentation':
      $ext_limpia = 'powerpoint';
      break;
    case 'application/vnd.openxmlformats-officedocument.presentationml.presentation':
      $ext_limpia = 'powerpoint';
      break;
    case 'application/vnd.ms-powerpoint':
      $ext_limpia = 'powerpoint';
      break;
  }
  return $ext_limpia;
}

´´´
