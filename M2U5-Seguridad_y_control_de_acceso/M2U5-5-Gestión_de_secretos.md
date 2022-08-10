# Gestión de secretos
Unidad M2U5 - Ejercicio 5

## ¿Qué vamos a hacer?
1. Crear y gestionar secretos.
1. Administrar el acceso a secretos.
1. Utilizar secretos en aplicaciones.

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

Para responder a todas las preguntas del ejercicio de forma agrupada, puedes crear antes de empezar un archivo a entregar llamado "M2U0-0-preguntas.txt". Recuerda identificar y ordenar cada pregunta seguida de su respuesta.

Encontrarás las preguntas entre el texto en cursiva: *PREGUNTA: ¿Cómo se llama el servicio de instancias de VMs de Google Cloud?*

### Tarea 1: Administrar secretos y sus accesos
Como primer paso para utilizar el servicio, habilita la API "Secret Manager API" en **API y servicios > Biblioteca**.

La información se organiza en *secretos* y sus *versiones*, donde un *secreto* puede contar con varias *versiones*, incluyendo la última versión que siempre podemos referenciar también como `latest`.

Un *secreto* representa, p. ej., una contraseña, credenciales o certificado, y sus *versiones* las diferentes versiones que ha tenido el contenido del mismo durante el tiempo.

#### Crear secretos
Primero crea un secreto sigiuiendo las siguientes instrucciones actualizadas con la consola web o CLI, cómo prefieras: [Creando un secreto](https://cloud.google.com/secret-manager/docs/creating-and-accessing-secrets?hl=en#create)
- ID de secreto: `secret-password`
- Política de replicación: automática
- No añadas una versión o contenido del secreto aún

Ahora lista y comprueba el secreto creado con el otro método (consola web o CLI): [Listando secretos](https://cloud.google.com/secret-manager/docs/managing-secrets?hl=en#listing_secrets) y [Obteniendo los detalles de un secreto](https://cloud.google.com/secret-manager/docs/managing-secrets?hl=en#getting_details_about_a_secret).

Por último, añade una versión al secreto: [Añadiendo una versión del secreto](https://cloud.google.com/secret-manager/docs/creating-and-accessing-secrets?hl=en#add-secret-version)
1. Crea un archivo local con un texto de ejemplo
1. Añade una versión al secreto con tu CLI y el path al archivo local con el secreto

*ENTREGABLES: M2U5-5-tarea_1-archivo_1-captura_1.jpg: Captura de pantalla mostrando en la consola web el secreto.*

#### Acceder a secretos
Como por defecto contamos con el rol de propietarios del proyecto que hemos creado, tenemos acceso completo a las operaciones sobre secretos y sus versiones.

1. Lista las versiones disponibles del secreto: [Listando las versiones de un secreto](https://cloud.google.com/secret-manager/docs/managing-secret-versions?hl=en#list)
1. Accede a la versión del secreto que has creado: [Accediendo a una versión del secreto](https://cloud.google.com/secret-manager/docs/creating-and-accessing-secrets?hl=en#access)

#### Actualizar y deshabilitar versiones de secretos
Vamos a comprobar cómo administrar secretos, habilitándo, deshabilitando y destruyendo sus versiones. Para ello sigue estas instrucciones en la consola web o CLI: [Administrando versiones de secretos](https://cloud.google.com/secret-manager/docs/managing-secret-versions)
1. Primero añade una nueva versión al secreto con el comando antes utilizado: `gcloud secrets version add [ID_SECRETO] --data-file="[PATH_ARCHIVO_SECRETO]"` (incluyendo comillas pero no corchetes)
1. Ahora lista las versiones del secreto y comprueba cómo puedes también acceder a la última versión con la ID alias de `latest`.
1. Deshabilita la última versión del secreto.
    1. *PREGUNTA: ¿A qué versión apunta ahora `latest`?*
1. Vuelve a habilitar la versión deshabilitada y comprueba que puedes volver a acceder a ella.

*ENTREGABLES: M2U5-5-tarea_1-archivo_2-captura_2.jpg: Captura de pantalla mostrando en la consola web las versiones del secreto.*

### Tarea 2: Utilizar secretos en aplicaciones
Dependiendo de tu aplicación y la plataforma sobre la que está desplegada, puedes acceder a los secretos utilizando la API o las Cloud Client Libraries, o también montándolos como volúmenes o variables de entorno.

Sigue el siguiente tutorial completo paso-a-paso para desplegar una app web de ejemplo en Cloud Run que utiliza Secret Manager para guardar las credenciales de conexión a la BBDD, disponible en veriones para Node.js, Python o Java: [Instructivo de autenticación de usuarios finales para Cloud Run](https://cloud.google.com/run/docs/tutorials/identity-platform).

*ENTREGABLES: M2U5-5-tarea_2-archivo_1-captura_1.jpg: Captura de pantalla mostrando la app web desplegada.*

## Resumen de entregas
1. M2U0-0-preguntas.txt: Respuestas a todas las preguntas planteadas en el ejercicio.
1. M2U5-5-tarea_1-archivo_1-captura_1.jpg: Captura de pantalla mostrando en la consola web el secreto.
1. M2U5-5-tarea_1-archivo_2-captura_2.jpg: Captura de pantalla mostrando en la consola web las versiones del secreto.
1. M2U5-5-tarea_2-archivo_1-captura_1.jpg: Captura de pantalla mostrando la app web desplegada.

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. Eliminar el secreto creado.
1. Eliminar los archivos del entorno local.
1. Elimina la app de Cloud Run desplegada: [Eliminando los recursos del tutorial](https://cloud.google.com/run/docs/tutorials/identity-platform#delete-resources).
