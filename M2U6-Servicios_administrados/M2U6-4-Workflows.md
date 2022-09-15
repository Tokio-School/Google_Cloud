# Workflows
Unidad M2U6 - Ejercicio 4

## ¿Qué vamos a hacer?
1. Desplegar endpoints serverless y externos.
1. Ejecutar un workflow sencillo.

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

Para responder a todas las preguntas del ejercicio de forma agrupada, puedes crear antes de empezar un archivo a entregar llamado "M2U6-4-preguntas.txt". Recuerda identificar y ordenar cada pregunta seguida de su respuesta.

Encontrarás las preguntas entre el texto en cursiva: *PREGUNTA: ¿Cómo se llama el servicio de instancias de VMs de Google Cloud?*

### Tarea 1: Desplegar un workflow serverless
Vamos a desplegar un workflow que utilice varios endpoints serverless de servicios de Google Cloud y un endpoint externo.

Este workflow o flujo de trabajo contará con varios pasos:
1. Una función de Cloud Functions generará un nº aleatorio
1. La siguiente función de Cloud Functions lo multiplicará
1. Una API externa devolverá el logaritmo del resultado
1. Un servicio de Cloud Run lo redondeará
1. Workflow se encargará de ejectuar el flujo de trabajo completo, de principio a fin, llamando a todos los servicios, internos y externos.

Esquema del workflow:
![Esquema del workflow](https://cloud.google.com/static/workflows/images/workflows-run-visualization.svg)

#### Habilitar APIs, configuración y autenticación
Antes de comenzar el despliegue, como siempre, el primer paso será habilitar las APIs utilizadas:
En la consola web, navega a **APIs y servicios > Biblioteca** y busca y habilita las siguientes APIs:
- Cloud Build
- Cloud Functions
- Cloud Run
- Cloud Storage
- Container Registry
- Wortflows

Configura las siguientes variables de entorno:
1. `export SERVICE_ACCOUNT=workflows-sa`
1. `export REGION=europe-west1`

Establece la región de `europe-west1` como predeterminada para todos los servicios:
- `gcloud config set run/region $REGION`
- `gcloud config set workflows/location $REGION`

Crea una cuenta de servicio para Workflows con los permisos necesarios para invocar al resto de servicios:
- `gcloud iam service-accounts create $SERVICE_ACCOUNT`
- `gcloud projects add-iam-policy-binding [ID_PROYECTO] --member "serviceAccount:$SERVICE_ACCOUNT@[ID_PROYECTO].iam.gserviceaccount.com" --role "roles/run.invoker"`

#### Despliega la primera función de Cloud Functions
Esta primera función simplemente genera un nº aleatorio entre [1-100] y lo devuelve en formato JSON.
Esta función será desarrollada con Python 3 y estará desplegada sobre Cloud Functions.

Para ello, sigue estos pasos:
1. Crea un directorio en Cloud Shell o tu entorno local para la primera función: `mkdir ~/randomgen && cd ~/randomgen`
1. Crea un script de Python llamado `main.py` con el siguiente código: [main.py](https://github.com/GoogleCloudPlatform/workflows-demos/blob/HEAD/service-chaining/randomgen/main.py)
1. Crea un archivo de requisitos llamado `requirements.txt` con el siguiente contenido: `flask>=1.0.2`
1. Despliega la función en Cloud Functions con un activador HTTP: `gcloud functions deploy randomgen --runtime python37 --trigger-http --allow-unauthenticated`
1. Una vez desplegada, comprueba la función: `curl $(gcloud functions describe randomgen --format='value(httpsTrigger.url)')`

De esta forma hemos desplegado la primera función que genera un nº aleatorio y lo devuelve.

#### Despliega la segunda función de Cloud Functions
Esta segunda función recibe el nº aleatorio generado por la primera, lo multiplica por 2 y lo devuelve también en formato JSON.
Esta función también será desarrollada con Python 3 y estará desplegada sobre Cloud Functions.

Para ello, sigue estos pasos:
1. Crea un directorio en Cloud Shell o tu entorno local para la primera función: `mkdir ~/multiply && cd ~/multiply`
1. Crea un script de Python llamado `main.py` con el siguiente código: [main.py](https://github.com/GoogleCloudPlatform/workflows-demos/blob/HEAD/service-chaining/multiply/main.py)
1. Crea un archivo de requisitos llamado `requirements.txt` con el siguiente contenido: `flask>=1.0.2`
1. Despliega la función en Cloud Functions con un activador HTTP: `gcloud functions deploy multiply --runtime python37 --trigger-http --allow-unauthenticated`
1. Una vez desplegada, comprueba la función: `curl $(gcloud functions describe multiply --format='value(httpsTrigger.url)') -X POST -H "content-type: application/json" -d '{"input": 5}'`

De esta forma hemos desplegado la segunda función que recibe el nº aleatorio de la primera, lo multiplica por 2 y lo devuelve.

#### Despliega un servicio de Cloud Run
Este microservicio será el último, que recibirá el resultado del paso anterior y redondeará ese número hacia abajo. El resultado del paso anterior vendrá desde una API pública que realizará una operación matemática a partir del resultado de la segunda función, mientras que Workflows se encargará de llamar a dicha API y recoger su resultado.

Para ello, sigue estos pasos:
1. Crea un directorio en Cloud Shell o tu entorno local para la primera función: `mkdir ~/floor && cd ~/floor`
1. Crea un script de Python llamado `app.py` con el siguiente código: [app.py](https://github.com/GoogleCloudPlatform/workflows-demos/blob/HEAD/service-chaining/floor/app.py)
1. Crea un archivo llamado `Dockerfile` con el siguiente contenido: [Dockerfile](https://github.com/GoogleCloudPlatform/workflows-demos/blob/HEAD/service-chaining/floor/Dockerfile)
1. Construye la imagen de contenedor remotamente con Cloud Build: `gcloud builds submit --tag gcr.io/[ID_PROYECTO]/floor`
1. Despliega la función en Cloud Functions con un activador HTTP: `gcloud functions deploy multiply --runtime python37 --trigger-http --allow-unauthenticated`
1. Una vez desplegada, copia la URL del endpoint del servicio

#### Despliega el workflow
Por último vamos a configurar y desplegar el flujo de trabajo de Workflows, que se encargarár de ejecutar los 4 pasos uno a uno, llamando a las 2 funciones, la API pública y el servicio de Cloud Run.

Para ello, sigue estos pasos:
1. Navega al directorio inicial, donde están los 3 directorios creados: `cd ..`
1. Crea un archivo con la definición del flujo de trabajo llamado `workflow.yaml` con el siguiente contenido: xxx
1. Despliega el flujo de trabajo: `gcloud workflows deploy number_flow --source workflow.yaml`
1. Ejecuta el flujo de trabajo: `gcloud workflows run number_flow`

Tanto en la respuesta del comando como en la consola web puedes comprobar el estado final: `state: SUCCEDED`

*ENTREGABLE: M2U6-4-tarea_1-archivo_1-captura_1.jpg: Captura de pantalla mostrando el resultado del flujo de trabajo en la línea de comandos o la consola web.*

## Resumen de entregas
1. M2U6-4-tarea_1-archivo_1-captura_1.jpg: Captura de pantalla mostrando el resultado del flujo de trabajo en la línea de comandos o la consola web.

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. Elimina el servicio de Cloud Run.
1. Elimina las 2 funciones de Cloud Functions.
1. Elimina el flujo de trabajo de Workflows.
