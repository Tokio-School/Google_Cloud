# Cloud Tasks
Unidad M2U6 - Ejercicio 3

## ¿Qué vamos a hacer?
1. Crear colas de tareas.
1. Enviar tareas diferidas.
1. Configurar workers para procesar las tareas.

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

Para responder a todas las preguntas del ejercicio de forma agrupada, puedes crear antes de empezar un archivo a entregar llamado "M2U6-3-preguntas.txt". Recuerda identificar y ordenar cada pregunta seguida de su respuesta.

Encontrarás las preguntas entre el texto en cursiva: *PREGUNTA: ¿Cómo se llama el servicio de instancias de VMs de Google Cloud?*

### Tarea 1: Crear colas de tareas
Vamos a crear una cola de tareas de Cloud Tasks junto a una aplicación worker en App Engine como destino de procesamiento de las mismas en un servicio serverless.

Antes de comenzar, habilita la API de Cloud Tasks navegando a **APIs y administración > Biblioteca**.

#### Crear cola de tareas
Para crear la cola de tareas podemos hacerlo desde la consola o la CLI:
1. Crea una cola de tareas de Cloud Tasks: `gcloud tasks queues create [ID_COLA] --location [LOCALIZACION]`
    1. Utiliza una `[ID_COLA]` que escojas y `europe-west1` como `[LOCALIZACION]`
1. Tras unos momentos, comprueba la creación de la cola de tareas: `gcloud tasks queues describe [ID_COLA]`

Del mismo modo, puedes comprobar la creación de la cola de tareas en la consola navegando a **Cloud Tasks**.

#### Desplegar aplicación worker
Para procesar las tareas, vamos a utilizar una aplicación de App Engine como destino, que recibirá las tareas y las procesará, registrándolas en sus logs.

Para ello, sigue los siguientes pasos:
1. Crea una app en App Engine si no lo has hecho previamente, en el ejercicio de App Engine.
1. En Cloud Shell o tu entorno local, clona el repositorio y navega a [GoogleCloudPlatform/python-docs-samples/appengine/flexible/tasks/](https://github.com/GoogleCloudPlatform/python-docs-samples/tree/main/appengine/flexible/tasks)
1. Inspecciona el código y configuración de la aplicación en los archivos `main.py` y `app.yaml`
1. Despliega la app desde el directorio `tasks`: `gcloud app deploy --location europe-west1`
1. Comprueba la app copiando la URL de su endpoint en tu navegador

De esta forma podemos desplegar una aplicación serverless que será el destino de nuestras tareas.

#### Utilizar script que envía tareas
Ahora vamos a crear las tareas de forma programática, con una librería de cliente de Cloud Client Libraries, la API o gcloud CLI:
1. En Cloud Shell o tu entorno local, navega al directorio [GoogleCloudPlatform/python-docs-samples/appengine/flexible/tasks/](https://github.com/GoogleCloudPlatform/python-docs-samples/tree/main/appengine/flexible/tasks)
1. Inspecciona el script `create_app_engine_queue_task.py`
1. Crea una tarea con el comando: `python3 create_app_engine_queue_task.py [ID_PROYECTO] [ID_COLA] europe-west1 hola-mundo!`

Comprueba la ejecución revisando los logs de tu aplicación en App Engine.

*ENTREGABLES: M2U6-3-tarea_1-archivo_1-captura_1.jpg: Captura de pantalla mostrando los logs de la tarea procesada en App Engine.*

## Resumen de entregas
1. M2U6-3-tarea_1-archivo_1-captura_1.jpg: Captura de pantalla mostrando los logs de la tarea procesada en App Engine.

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. Eliminar la cola de tareas en Cloud Tasks.
1. La aplicación de App Engine no puede ser eliminada.
1. Opcionalmente, eliminar el repositorio clonado localmente.
