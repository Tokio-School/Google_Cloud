# Control de acceso en Cloud Storage
Unidad M2U5 - Ejercicio 3

## ¿Qué vamos a hacer?
1. Controlar el acceso con las ACLs a nivel de bucket y objeto.
1. Permitir acceso temporal con las URL firmadas.

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

Para responder a todas las preguntas del ejercicio de forma agrupada, puedes crear antes de empezar un archivo a entregar llamado "M2U0-0-preguntas.txt". Recuerda identificar y ordenar cada pregunta seguida de su respuesta.

Encontrarás las preguntas entre el texto en cursiva: *PREGUNTA: ¿Cómo se llama el servicio de instancias de VMs de Google Cloud?*

### Tarea 1: Controlar acceso con las ACLs

#### Crear cuenta de servicio y bucket de Cloud Storage
Primero vamos a preparar el entorno necesario. Para ello, sigue los siguientes pasos generales cuyas instrucciones hemos seguido en ejercicios anteriores:
- Crea una cuenta de servicio ([M2U5-1-Cuentas_de_servicio](https://github.com/Tokio-School/Google_Cloud/blob/main/M2U5-Seguridad_y_control_de_acceso/M2U5-1-Cuentas_de_servicio.md)).
- Crea un bucket en GCS ([M2U3-1-Cloud_Storage](https://github.com/Tokio-School/Google_Cloud/blob/main/M2U3-Almacenamiento_y_bases_de_datos/M2U3-1-Cloud_Storage.md)) con permisos de acceso granulares.
- Sube un archivo de texto al bucket con un texto de ejemplo (p. ej. "hola mundo!").

#### A nivel de bucket
Vamos a asignar acceso a la cuenta de servicio a Cloud Storage por ACL a nivel del bucket, a todos los objetos por igual. En el ejercicio de [M2U3-1-Cloud_Storage](https://github.com/Tokio-School/Google_Cloud/blob/main/M2U3-Almacenamiento_y_bases_de_datos/M2U3-1-Cloud_Storage.md) ya vimos cómo hacerlo desde la consola web, por lo que ahora veremos cómo hacerlo desde Cloud SDK.

1. Primero comprueba cómo la cuenta de servicio no tiene acceso a ninguna operación a nivel del bucket:
    1. Lista el contenido del bucket impersonando la cuenta de servicio: `gsutil -i "[EMAIL_CUENTA_DE_SERVICIO]" ls gs://[NOMBRE_BUCKET]`
    1. La flag `-i` es un alias de `--impersonate-service-account`.
    1. Comprueba cómo la cuenta de servicio no tiene acceso. Si lo tuviera, comprueba que no tiene ningún rol asignado y que estás impersonándola correctamente.
    1. Comprueba como tu usuario sí tiene acceso a la misma operación: `gsutil ls gs://[NOMBRE_BUCKET]`.
1. Tras ello, añade tu usuario a la lista de control de acceso ("ACL" en inglés) asignada al bucket:
    1. Descarga la ACL en formato JSON y revísala: `gsutil acl get gs://[NOMBRE_BUCKET] >> acl_bucket.json` y `cat acl_bucket.json`.
    1. Añade tu email con el permiso de lectura o `READER` a las ACL del bucket: `gsutil acl ch -u [EMAIL_USUARIO]:READ gs://[NOMBRE_BUCKET]`.
    1. Descarga de nuevo y comprueba que tu usuario aparece ahora en la ACL: `gsutil acl get gs://[NOMBRE_BUCKET] >> acl_bucket.json` y `cat acl_bucket.json`.
1. Por último, comprueba de nuevo que la cuenta de servicio ahora puede listar el contenido del bucket, impersonándola con Cloud SDK.

#### A nivel de objeto
Del mismo modo, repite los pasos anteriores para actualizar las ACL del objeto:
1. Sustituye la URL `gs://[NOMBRE_BUCKET]` por `gs://[NOMBRE_BUCKET]/[NOMBRE_OBJETO]`.
1. Sustituye el nombre del archivo JSON `acl_bucket.json` por `acl_object.json`.

Por último, de forma autónoma utiliza lo aprendido para modificar tu acceso al bucket (no al objeto) a un ACL de `OWNER` modificando el archivo de ACL en JSON y subiéndolo actualizado, siguiendo esta guía: [Cambiando las ACLs](https://cloud.google.com/storage/docs/access-control/create-manage-lists?hl=en#changing-acls).

*ENTREGABLES:*
1. M2U5-1-tarea_1-archivo_1-acl_bucket.json: ACL final del bucket.
1. M2U5-1-tarea_1-archivo_2-acl_objeto.json: ACL final del objeto.

### Tarea 2: URL firmadas
En esta tarea vamos a utilizar las URL firmadas para permitir que una persona externa a la empresa, sin disponer de cuenta de Google, Google Workspace o Cloud Identity, pueda acceder a objetos o buckets temporalmente en GCS.

Para crear una URL firmada:
1. Descarga una credencial de la cuenta de servicio creada y autorizada en la tarea anterior: `gcloud iam service-accounts keys create credentials.json --iam-account=[EMAIL_CUENTA_SERVICIO]@[ID_PROYECTO].iam.gserviceaccount.com` (se descargará como `credentials.json` en tu directorio actual)
1. Crea una URL firmada con una duración de 60 minutos utilizando la credencial (autenticación y autorización) de la cuenta de servicio: `gsutil signurl -d 60m [PATH_ARCHIVO_CREDENCIALES.json] gs://[NOMBRE_BUCKET]/[NOMBRE_OBJETO]`.
1. La respuesta al comando contiene varios elementos, como puedes comprobar en [Crear una URL firmada para descargar un objeto](https://cloud.google.com/storage/docs/access-control/signing-urls-with-helpers#download-object). Copia la URL firmada de la respuesta, que será la última URL indicada.
1. Abre una nueva ventana de incógnito en el navegador para asegurarte no estar autenticado con tu cuenta de Google, y accede a la URL para poder comprobar el acceso al objeto de GCS.

*ENTREGABLES: M2U5-1-tarea_2-archivo_1-captura_1.jpeg: Captura de pantalla del navegador mostrando la URL y el contenido del objeto, o blanco si se ha descargado a local.*

## Resumen de entregas
1. M2U5-3-preguntas.txt: Respuestas a todas las preguntas planteadas en el ejercicio.
1. M2U5-1-tarea_1-archivo_1-acl_bucket.json: ACL final del bucket.
1. M2U5-1-tarea_1-archivo_2-acl_objeto.json: ACL final del objeto.
1. M2U5-1-tarea_2-archivo_1-captura_1.jpeg: Captura de pantalla del navegador mostrando la URL y el contenido del objeto, o blanco si se ha descargado a local.

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. Eliminar el bucket de Cloud Storage creado.
1. Eliminar la cuenta de servicio creada.
1. Eliminar los archivos en el entorno local.
