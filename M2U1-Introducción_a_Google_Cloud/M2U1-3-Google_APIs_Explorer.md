# Google APIs Explorer
Unidad M2U1

## ¿Qué vamos a hacer?
1. Explorar las APIs de Google Cloud.
1. Utilizar las APIs de Google Cloud para crear y administrar recursos.

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

*NOTA: No hay preguntas para este ejercicio.*

### Tarea 1: APIs de Google Cloud
En este ejercicio, vamos a trabajar con las APIs de Google Cloud usando el portal de desarrolladores de la API.

La práctica recomendada es, para desarrollar una aplicación o script que realice llamadas a la API, comprobar las mismas previamente en este portal de Google APIs Explorer.

#### Explorador de APIs
Explora el portal de Google APIs Explorer hasta que te encuentres cómodo para utilizarlo en un proyecto real:

1. Accede al portal de [Google APIs Explorer](https://developers.google.com/apis-explorer) y familiarízate con el mismo.
    1. El portal es un buscador de las APIs de Google (incluyendo las de Google Cloud) que enlaza con la referencia de la documentación de las mismas.
1. Explora algunas APIs de Google Cloud en la referencia, para comprender cómo se estructuran.
1. En la referencia, explora algunos métodos de la API. Al hacerlo, se abrirá el menú desplegable lateral de **Try this API** o "Prueba esta API" (a la derecha).
1. Explora los parámetros de la petición, y relaciónalos con la referencia de la documentación.
1. No te preocupes por la autenticación, ya que el portal recuperará tu login de usuario con tu cuenta de Google para autenticar la petición, si están activadas las opciones de **Google OAuth 2.0** y **API key**.

Una vez explorado el portal, úsalo para explorar las siguientes APIs y completar las siguientes instrucciones de forma autónoma.

*NOTAS:*
1. *En el menú desplegable de **Try this API**, en un botón cuadrado, o en el enlace de **Try it now** de la referencia, podrás maximizar las opciones para probar la API.*
1. *Puedes ejecutar las peticiones a la API desde el portal de **Try this API** o con `curl` desde local o Cloud Shell, si creas una API key y token de acceso, siguiendo las instrucciones del portal.*
1. *Revisa con atención la referencia y los parámetros necesarios, puesto que el objetivo del ejercicio es familiarizarte con el uso de las APIs.*
1. *En las APIs, como parámetro **project**, usa la ID de tu proyecto.*

#### Crear bucket de Cloud Storage
Utiliza la API de Cloud Storage (JSON) para:

*NOTA: Revisa los entregables tras las instrucciones para poder cumplirlos paso a paso.*

1. Listar los buckets de GCS disponibles.
1. Crear un bucket de GCS.
    1. Como nombre de bucket globalmente único puedes utilizar `ID_PROYECTO-M2U1-3`.
1. Listar de nuevo los buckets, mostrando el recién creado.
1. Comprueba el bucket creado en la consola de Google Cloud, en **Storage > Navegador**.
1. Obtener los detalles del bucket creado, con la API.
1. Eliminar el bucket creado.

*ENTREGABLES:*
1. M2U1-3-tarea_1-archivo_1-captura_1.jpg: Captura de pantalla con la petición y resultado de crear el bucket de GCS.
1. M2U1-3-tarea_1-archivo_2-captura_2.jpg: Captura de pantalla con la petición y resultado de obtener los detalles del bucket de GCS.
1. M2U1-3-tarea_1-archivo_3-captura_3.jpg: Captura de pantalla con la petición y resultado de eliminar el bucket de GCS.

#### Crear instancia de VM
Del mismo modo, utiliza la API v1 de Compute Engine para:

*NOTA: Revisa los entregables tras las instrucciones para poder cumplirlos paso a paso.*

1. Listar las instancias de VM disponibles.
1. Crear una nueva instancia de VM (en cualquier zona, de cualquier tipo, con cualquier SO, etc.).
1. Listar las instancias de VM disponibles de nuevo.
1. Comprueba la instancia creada en la consola de Google Cloud, en **Compute Engine > Instancias de VM**.
1. Obtener los detalles de la instancia creada, con la API.
1. Elimina la instancia creada.

*ENTREGABLES:*
1. M2U1-3-tarea_1-archivo_4-captura_4.jpg: Captura de pantalla con la petición y resultado de crear la instancia de GCE.
1. M2U1-3-tarea_1-archivo_5-captura_5.jpg: Captura de pantalla con la petición y resultado de obtener los detalles de la instancia de GCE.
1. M2U1-3-tarea_1-archivo_6-captura_6.jpg: Captura de pantalla con la petición y resultado de eliminar la instancia de GCE.

## Resumen de entregas
1. M2U1-3-tarea_1-archivo_1-captura_1.jpg: Captura de pantalla con la petición y resultado de crear el bucket de GCS.
1. M2U1-3-tarea_1-archivo_2-captura_2.jpg: Captura de pantalla con la petición y resultado de obtener los detalles del bucket de GCS.
1. M2U1-3-tarea_1-archivo_3-captura_3.jpg: Captura de pantalla con la petición y resultado de eliminar el bucket de GCS.
1. M2U1-3-tarea_1-archivo_4-captura_4.jpg: Captura de pantalla con la petición y resultado de crear la instancia de GCE.
1. M2U1-3-tarea_1-archivo_5-captura_5.jpg: Captura de pantalla con la petición y resultado de obtener los detalles de la instancia de GCE.
1. M2U1-3-tarea_1-archivo_6-captura_6.jpg: Captura de pantalla con la petición y resultado de eliminar la instancia de GCE.

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. Comprueba en la consola web que el bucket de Cloud Storage y la instancia de VM de Compute Engine se han eliminado correctamente.
