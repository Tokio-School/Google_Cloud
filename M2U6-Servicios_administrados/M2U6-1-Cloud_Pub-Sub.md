# Cloud Pub/Sub
Unidad M2U6 - Ejercicio 1

## ¿Qué vamos a hacer?
1. Crear una aplicación que envía y recibe mensajes.
1. Crear un esquema para un tema.
1. Utilizar suscripciones de tipo pull y push.
1. Desplegar arquitecturas de 1 a 1 y 1 a varios.

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

Para responder a todas las preguntas del ejercicio de forma agrupada, puedes crear antes de empezar un archivo a entregar llamado "M2U6-1-preguntas.txt". Recuerda identificar y ordenar cada pregunta seguida de su respuesta.

Encontrarás las preguntas entre el texto en cursiva: *PREGUNTA: ¿Cómo se llama el servicio de instancias de VMs de Google Cloud?*

### Tarea 1: Desplegar esquema "publisher"/"suscriber"
En esta primera tarea vamos a comenzar por crear un tema de Pub/Sub y varias suscripciones simples para comprobar los conceptos fundamentales del servicio.

Como es habitual, comienza por habilitar la API:
1. En la consola web, navega a **APIs y servicios > Biblioteca**
1. Busca la API de Cloud Pub/Sub y habilítala

#### Crear un tema
Vamos a comenzar por crear una pareja de tema para enviar mensajes de Pub/Sub.

Para ello, sigue los siguientes pasos:
1. En la consola web, navega a **Pub/Sub > Temas**
1. Pulsa el botón **Crear tema**
1. Asígnale una ID de tema y descripción que consideres
1. Deshabilita la opción **Añadir una suscripción por defecto**
1. Deja el resto de opciones con sus valores por defecto
1. Pulsa **Crear tema**

De esta forma contamos ya con un tema que podemos usar para enviar mensajes

#### Crear una suscripción de tipo pull
Vamos a continuar creando una suscripción de tipo pull simple conectada al tema creado.

Para ello, sigue los siguientes pasos:
1. En la consola web, navega a **Pub/Sub > Suscripciones**
1. Pulsa el botón **Crear suscripción**
1. Asígnale una ID de suscripción y una descripción que consideres
1. Como tema, selecciona el tema creado en el paso anterior
1. Como tipo de envío, selecciona "extraer" o "pull"
1. Deja el resto de opciones con sus valores por defecto
1. Pulsa **Crear suscripción**

Así hemos creado una suscripción de tipo pull para el tema de Pub/Sub.

#### Comprobar mensajes con la consola y Cloud SDK
Para comprobar la conexión entre tema y suscripción, vamos a publicar y verificar algunos mensajes:
1. En la consola web, navega a **Pub/Sub > Temas** y pulsa en el tema creado
1. En la pestaña **Mensajes > Detalles del tema**, pulsa **Publicar mensaje**
1. Añade un cuerpo de mensaje con un texto y algunos atributos, p. ej. `hola mundo!` y `foo=bar`

Ahora vamos a comprobar que la suscripción recibe dicho mensaje:
1. En la consola web, navega a **Pub/Sub > Suscripciones**
1. Pulsa en la suscripción creada
1. En la pestaña **Mensajes**, pulsa el botón **Pull** y comprueba en mensaje enviado

De esta forma podemos verificar la configuración de temas y suscripciones desde la consola o línea de comandos.

#### Deplegar emisor y receptor para una suscripción de tipo push
Por último, vamos a desplegar 2 aplicaciones, emisor y receptor, que se comunicarán a través de mensajes con una suscripción de tipo "push".

Para ello, sigue los siguientes pasos:
1. Crea una instancia de VM de núcleo compartido de Debian 10 con la cuenta de servicio por defecto de Compute Engine, para que disponga de permisos para llamar a la API de Cloud Pub/Sub y conéctate por SSH
1. Instala la librería de Cloud Client Library para Python: `pip install --upgrade google-cloud-pubsub`
1. Crea un script de Python llamado `publisher.py` que utilizaremos para publicar mensajes al tema: [publisher.py](https://github.com/googleapis/python-pubsub/blob/HEAD/samples/snippets/publisher.py)
1. En `publisher.py`, sustituye los valores de `project_id` por tu ID de proyecto y `topic_id` por la ID de tu tema de Pub/Sub en la función `publish_messages_with_error_handler`
1. Mantén la ventana de SSH abierta, ya que más tarde ejecutarás el script `publisher.py` para enviar múltiples mensajes
1. En la consola web, navega a **Cloud Functions** y despliega una función con activador HTTP de Python 3 con este código: [main.py](https://github.com/GoogleCloudPlatform/python-docs-samples/blob/main/functions/pubsub/main.py) y [requirements.txt](https://github.com/GoogleCloudPlatform/python-docs-samples/blob/main/functions/pubsub/requirements.txt)
1. Anota la URL del endpoint de la función de Cloud Functions
1. Ahora navega a **Pub/Sub > Suscripciones** y crea una suscripción de tipo "enviar" o "push" para el tema creado, con la URL de la función como extremo

Por último, comprueba tu despliegue:
1. En la ventana SSH de la instancia de VM, ejecuta el script para enviar múltiples mensajes al tema: `python publisher.py [ID_PROYECTO] publish-with-error-handler [ID_TEMA]`
1. En la consola web, navega a **Cloud Functions**, pulsa el nombre de tu función y revisa los logs de la misma
1. La publicación de mensajes puede tardar unos segundos para cada uno, un par de minutos en total. Si no aparecen los logs, revísalo de nuevo pasados unos segundos.

De esta forma hemos desplegado una aplicación de emisor y otra de receptor, enviando mensajes de Pub/Sub a través de una suscripción de tipo push desde una instancia de VM a un destino serverless en una función de Cloud Functions.

Como bonus, estos mensajes se han enviado al tema creado, que cuenta con 2 suscripciones, una de tipo pull y otra push, por lo que hemos desplegado una arquitectura de mensajería de 1 a muchos o "fan-out".

*ENTREGABLES: M2U6-1-tarea_1-archivo_1-captura_1.jpg: Captura de pantalla mostrando los logs de invocación de la función de Cloud Functions.*

## Resumen de entregas
1. M2U6-1-tarea_1-archivo_1-captura_1.jpg: Captura de pantalla mostrando los logs de invocación de la función de Cloud Functions.

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. Elimina las suscripciones y tema de Pub/Sub
1. Elimina la instancia de VM creada
1. Elimina la función de Cloud Functions creada
