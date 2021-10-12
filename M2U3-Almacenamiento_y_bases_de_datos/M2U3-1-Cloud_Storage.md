# Cloud Storage
Unidad M2U3 - Ejercicio 1

## ¿Qué vamos a hacer?
1. Crear un bucket en Cloud Storage.
1. Gestionar objetos desde la consola web.
1. Gestionar objetos y sincronizar directorios desde la línea de comandos.
1. Gestionar las listas de control de acceso a Cloud Storage.
1. Establecer reglas de ciclo de vida.
1. Habilitar el versionado de objetos

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

Para responder a todas las preguntas del ejercicio de forma agrupada, puedes crear antes de empezar un archivo a entregar llamado "M2U3-1-preguntas.txt". Recuerda identificar y ordenar cada pregunta seguida de su respuesta.

Encontrarás las preguntas entre el texto en cursiva: *PREGUNTA: ¿Cómo se llama el servicio de instancias de VMs de Google Cloud?*

### Tarea 1: Gestionar buckets y objetos
En esta primera tarea vamos a crear un bucket y gestionar objetos en el mismo.

#### Crear un bucket desde la consola
Vamos a crear un bucket desde la consola:

1. En la consola, navega a **Storage > Navegador** y pulsa en **Crear bucket**.
1. Explora todas las opciones y crea un bucket con las siguientes características:
    - Nombre: escoge un nombre globalmente único, como p. ej. la ID de tu proyecto más un sufijo como `-bucket`. Nos referiremos al mismo como `NOMBRE_BUCKET`.
    - Ubicación: regional en `europe-west1`.
    - Clase de almacenamietno predeterminado: standard.
    - Deshabilita la prevención de acceso público al bucket.
    - Control de acceso: preciso.

De esta forma podemos crear buckets de GCS desde la consola web.

#### Gestionar objetos desde la consola
Para subir un archivo al bucket usando la consola:
1. En la consola, en **Storage > Navegador** pulsa sobre tu bucket.
1. Pulsa sobre **Subir archivos** y selecciona cualquier archivo local.
1. Una vez subido, puedes pulsar sobre el nombre del objeto y ver su información. Si es una imagen, se mostrará debajo de sus características.

Para descargar el objeto puedes hacerlo:
1. En el listado de objetos, con el botón de flecha hacia abajo a la derecha de la línea del objeto, o seleccionándolo y pulsando sobre **Descargar**.
1. Dentro de la página de detalles del objeto, pulsando sobre **Descargar**.

Para finalmente eliminar el objeto, puedes hacerlo:
1. En el listado de objetos, seleccionándolo y pulsando **Borrar**.
1. Dentro de la página de detalles del objeto, pulsando sobre **Borrar**.

De esta forma podemos subir archivos y carpetas a Cloud Storage, administrarlos y eliminarlos.

#### Gestionar objetos con gsutil
Ahora vamos a gestionar objetos desde la línea de comandos, usando la herramienta `gsutil`:

1. Crea un objeto en local: `echo "hola mundo!" > sample_file.txt`.
1. Sube el archivo al bucket: `gsutil cp sample_file.txt gs://NOMBRE_BUCKET/sample_file.txt`.
1. Lista los buckets creados: `gsutil ls`.
1. Lista los objetos dentro del bucket: `gsutil ls gs://NOMBRE_BUCKET/`.
1. Descarga el objeto con otro nombre: `gsutil cp gs://NOMBRE_BUCKET/sample_file.txt downloaded_file.txt`.
1. Elimina el objeto del bucket: `gsutil rm gs://NOMBRE_BUCKET/sample_file.txt`.

De esta forma podemos gestionar los buckets y objetos desde la línea de comandos.

#### Sincronizar directorios
Por último vamos a utilizar la línea de comandos para sincronizar archivos y subdirectorios entre local y Cloud Storage:
1. Crea una estructura de archivos local:
    1. Renombra el archivo anteriormente creado: `mv downloaded_file.txt sample_file1.txt`.
    1. Crea un subdirectorio: `mkdir sample_dir`.
    1. Crea 2 archivos dentro del subdirectorio: `touch sample_dir/sample_file2.txt` y `touch sample_dir/sample_file3.txt`.
1. Sincroniza los archivos con `gsutil -m rsync . gs://NOMBRE_BUCKET`.
1. Comprueba los archivos en Cloud Storage:
    1. Con `gsutil`: `gsutil ls gs://NOMBRE_BUCKET`.
    1. En la consola, en **Storage > Navegador**, pulsando sobre el nombre del bucket.
1. Ahora elimina uno de los archivos en Cloud Storage.
1. Sincroniza los cambios de Cloud Storage a local: `gsutil rsync gs://NOMBRE_BUCKET .`.
1. Comprueba en local los cambios sincronizados.

De esta forma podemos sincronizar subdirectorios y archivos entre local y Cloud Storage.

*ENTREGAS:* M2U3-1-tarea_1-archivo_1-captura_1.jpg: Captura de pantalla del listado de objetos del bucket en GCS.

### Tarea 2: Gestionar las ACL a nivel granular
En esta tarea vamos a gestionar las listas de control de acceso (o "ACL" en inglés) de los buckets y objetos de GCS.

Para controlar los accesos tenemos 2 opciones a nivel de bucket:
- Uniforme: las ACL se aplicarán uniformemente a todos los objetos del bucket.
- Preciso: las ACL se podrán aplicar de forma individual en cada objeto del bucket.

#### A nivel de bucket
Vamos a editar los permisos a nivel de bucket:

1. En la consola, navega a **Storage > Navegador** y pulsa sobre tu bucket.
1. Pulsa sobre la pestaña de **Permisos**.
1. Explora los permisos concedidos. P. ej., las identidades con los roles asignados de "proyecto/propietario" y "proyecto/editor" tendrán acceso de lectura y escritura, y con "proyecto/visor" acceso de lectura.
1. Para modificar las ACL, puedes pulsar **Agregar**, pulsar el botón de editar (icono de un lápiz) o seleccionar una identidad y eliminarla.

Prueba a añadir una cuenta de Google diferente, si dispones de ella, para comprobar el acceso.

De esta forma podemos controlar los permisos de acceso a nivel de bucket en GCS.

#### A nivel de objeto
Ahora vamos a repetir la operación a nivel de un objeto individual:

1. Dentro de la vista de **Objetos** del bucket, pulsa sobre alguno de los archivos subidos.
1. En la página de detalles del objeto, pulsa sobre **Modificar permisos**.

Al igual que antes, prueba a añadir una cuenta de Google diferente, si dispones de ella, para comprobar el acceso.

De esta forma podemos controlar los permisos de acceso a nivel de objeto individual en GCS.

#### Hacer objeto público
Ahora vamos a hacer público un objeto, permitiendo que cualquier persona pueda verlo. Ésto es útil si queremos servir objetos desde GCS como archivos estáticos de una web (CSS, imágenes, JS, etc.) o alojar archivos descargables de cualquier tipo.

1. En la página de detalles del objeto, pulsa sobre **Modificar permisos**.
1. Agrega una entrada y selecciona **Public** y **allUsers**.
1. Podemos hacer un objeto público para cualquier usuario sin autenticarse (como un archivo estático de una web) con **allUsers** o sólo para peticiones autenticadas, que nos permitirá loguear quién ha accedido al archivo, con **allAuthenticatedUsers**.
1. Recuerda seleccionar **Acceso** sólo de lectura con **Reader**.

De esta forma se generará un nuevo endpoint de la API para acceder al archivo. Comprueba ambos endpoints:
1. URL pública.
1. URL autenticada.

*PREGUNTA: ¿Qué diferencias hay entre ambas URL? ¿Y tras acceder al enlace?*

Prueba a abrir ambos enlaces en una nueva ventana de incógnito:
*PREGUNTA: ¿Qué sucede al acceder a cada uno de ellas?*

Ahora edita los permisos de nuevo y elimina la entrada de **allUsers**.
*PREGUNTA: ¿Qué sucede con las URLs? ¿Qué sucede al acceder a cada uno de ellas?*

### Tarea 3: Reglas de ciclo de vida y versionado de objetos
En esta tarea vamos a trabajar con las reglas de ciclo de vida automáticas y el versionado de los objetos en GCS:

#### Reglas de ciclo de vida
Vamos a establecer una regla de ciclo de vida para el bucket de ejemplo:
- El objeto se crea en la clase de almacenamiento estándar por defecto.
- 30 días después, se migra a nearline.
- 60 días después, se migra a coldline.
- 90 días después, se migra a archive.
- A los 2 años de su creación, se elimina automáticamente.

1. En la consola, en la página de detalles del bucket, activa la pestaña **Ciclo de vida**.
1. Pulsa en **Agregar una regla** y explora las diferentes opciones.
1. Crea 4 reglas para aplicar dichos cambios, teniendo en cuenta las siguientes condiciones para aplicar las reglas:
    - Sólo a los archivos de la clase de almacenamiento correspondiente.
    - Según el tiempo que ha pasado desde la creación del archivo.

También podemos comprobar y establecer las reglas con `gsutil`:
1. Descarga la política de ciclo de vida en formato JSON: `gsutil lifecycle get gs://NOMBRE_BUCKET`.
1. Compruéba el contenido del archivo: `cat nombre_archivo.json`.

*ENTREGABLES:* M2U3-1-tarea_3-archivo_1-captura_1.jpg: Captura de pantalla del listado de reglas de ciclo de vida del bucket en GCS.

#### Habilitar versionado de objetos
Ahora vamos a trabajar con objetos versionados en GCS.

Primero vamos a habilitar el versionado de objetos, que no se puede realizar desde la consola:
1. Habilita el versionado: `gsutil versioning set on gs://NOMBRE_BUCKET`.
1. Comprueba que está activado correctamente: `gsutil versioning get gs://NOMBRE_BUCKET`.

Ahora vamos a subir y administrar versiones de objetos en el bucket:
1. Crea un nuevo archivo en local y súbelo a Cloud Storage: `echo "hola mundo - v1" > sample_versioned_file.txt` y `gsutil cp sample_versioned_file.txt gs://NOMBRE_BUCKET/`.
1. Comprueba la operación:
    1. Lista los objetos subidos: `gsutil ls gs://NOMBRE_BUCKET`.
    1. Lista todas las versiones del objeto: `gsutil ls -a gs://NOMBRE_BUCKET` (a continuación del `#` se muestra el número de generación de cada versión).
    1. Comprueba el contenido del objeto en vivo actual: `gsutil ls gs://NOMBRE_BUCKET/sample_versioned_file.txt`.
1. Ahora repite las operaciones para subir un nuevo objeto, cambiando `v1` por `v2`, y comprueba los objetos subidos, versiones y el objeto en vivo actual.
1. Anota el número de generación de la versión `v1` y accede al mismo: `gsutil cat gs://NOMBRE_BUCKET/sample_versioned_file.txt#NUMERO_GENERACION`.
1. Ahora sube una nueva versión `v3` y de nuevo comprueba los objetos subidos, versiones y el objeto en vivo actual.
1. Accede a las versiones `v1` y `v2`.
1. Elimina la versión `v1`: `gsutil rm gs://NOMBRE_BUCKET/sample_versioned_file.txt#NUMERO_GENERACION`.
1. Comprueba los objetos subidos, versiones y el objeto en vivo actual.
1. Restaura la versión `v2` como objeto actual: `gsutil cp gs://NOMBRE_BUCKET/sample_versioned_file.txt#NUMERO_GENERACION gs://NOMBRE_BUCKET/sample_versioned_file.txt`.
1. Comprueba de nuevo los objetos subidos, versiones y el objeto en vivo actual.

Ahora habilita una regla de ciclo de vida adicional para sólo mantener las últimas 3 versiones:
- Acción: eliminar el objeto.
- Condición: cantidad de versiones más recientes, 3.

Por último vamos a comprobar que sólo se mantienen esas 3 últimas versiones:
1. Vuelve a la terminal y asegúrate de cuántas versiones actuales hay de tu objeto: `gsutil ls -a gs://NOMBRE_BUCKET`.
1. Sube un par de archivos con `v4` y `v5` respectivamente.
1. Comprueba el archivo en vivo actual y cuántas versiones se mantienen del objeto.

De esta forma podemos mantener y administrar múltiples versiones de un objeto y utilizar una regla de ciclo de vida para eliminar versiones antiguas.

*ENTREGAS:* M2U3-1-tarea_3-archivo_2-captura_2.jpg: Captura de pantalla del listado de versiones del objeto.

## Resumen de entregas
1. M2U3-1-preguntas.txt: Respuestas a todas las preguntas planteadas en el ejercicio.
1. M2U3-1-tarea_1-archivo_1-captura_1.jpg: Captura de pantalla del listado de objetos del bucket en GCS.
1. M2U3-1-tarea_3-archivo_1-captura_1.jpg: Captura de pantalla del listado de reglas de ciclo de vida del bucket en GCS.
1. M2U3-1-tarea_3-archivo_2-captura_2.jpg: Captura de pantalla del listado de versiones del objeto.

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. Borrar el bucket de GCS.
1. (Opcional) Borrar los objetos locales.
