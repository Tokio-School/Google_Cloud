# Cloud Run
Unidad M2U2 - Ejercicio 8

## ¿Qué vamos a hacer?
1. Desplegar una aplicación web en un contenedor serverless en Cloud Run.
1. Desplegar una pipeline de CI/CD integrada.
1. Actualizar la aplicación y gestionar el tráfico entre versiones.

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

*Nota: No hay preguntas para este ejercicio.*

### Tarea 1: Desplegar una aplicación web
En esta primera tarea vamos a desplegar un contenedor serverless con una aplicación web Python desde nuestro entorno de desarrollo local.

Para construir el contenedor podríamos optar por varias vías:
1. Generar la imagen de contenedor en local y subirla a un repositorio de un registro de imágenes de contenedor en Cloud Artifact Registry.
1. Generar la imagen de contenedor con Cloud Build y subirla a Cloud Artifact Registry.
1. Generar la imagen de contenedor y desplegarla en Cloud Run en un único paso utilizando la línea de comandos.

Para ahorrarnos los pasos de generar la imagen de contenedor de forma manual, vamos a desplegar el contenedor usando el Cloud SDK.

Antes de comenzar, habilita las APIs de Cloud Run, Cloud Build, Artifact Registry y Cloud Source Repositories:
1. En la consola, navega a **API y servicios > Biblioteca**.
1. Busca las APIs de Cloud Run, Cloud Build, Artifact Registry y Cloud Source Repositories y habilítalas o comprueba que ya están habilitadas.

*Nota:* Te recomendamos utilizar Cloud Shell pero también puedes utilizar un entorno local con Docker, Python 3 y las demás dependencias instaladas.

#### Comprobar app en local
Primero vamos a crear el entorno local y comprobar la aplicación en el mismo.

Crea en entorno en local y descarga el código:
1. Crea un directorio de trabajo local, p. ej. `mkdir ~/M2U2-8-Cloud_Run`.
1. Crea un subdirectorio para la primera tarea, p. ej. `mkdir ~/M2U2-8-Cloud_Run/tarea_1`.
1. Accede al subdirectorio de la primera tarea, p. ej. `cd ~/M2U2-8-Cloud_Run/tarea_1`.
1. Crea un script llamado `main.py` con el contenido del archivo `run/helloworld/main.py` del [repositorio de GitHub](https://github.com/GoogleCloudPlatform/python-docs-samples/blob/HEAD/run/helloworld/main.py).
1. Crea un entorno virtual de Python: `python3 -m venv env` y `source env/bin/activate`.
1. Crea un archivo de dependencias `requirements.txt` con el contenido del archivo `run/helloworld/requirements.txt` del [repositorio de GitHub](https://github.com/GoogleCloudPlatform/python-docs-samples/blob/501c09072fa07e04d0923a0a121b1af69c305c13/run/helloworld/requirements.txt).
1. Instala las dependencias de la función especificadas: `pip install -r requirements.txt`.
1. Repite los pasos para crear un archivo `Dockerfile` ([GitHub](https://github.com/GoogleCloudPlatform/python-docs-samples/blob/501c09072fa07e04d0923a0a121b1af69c305c13/run/helloworld/Dockerfile)) y `.dockerignore` ([GitHub](https://github.com/GoogleCloudPlatform/python-docs-samples/blob/501c09072fa07e04d0923a0a121b1af69c305c13/run/helloworld/.dockerignore)).

Comprueba la función ejecutándola en local:
1. Despliega la función en un servidor local con el comando `gunicorn --bind :8080 --workers 1 --threads 8 --timeout 0 main:app`.
1. Ahora comprueba la función:
    1. Desde la línea de comandos: `curl localhost:8080`.
    1. Desde la previsualización web de Cloud Shell o en tu navegador local: `http://localhost:8080`.
1. Puedes detener el servidor local con `CTRL + C`.

De esta forma podemos comprobar nuestra función realizando pruebas en un entorno de desarrollo local o de CI/CD.

*ENTREGABLES*:
1. M2U2-8-tarea_1-archivo_1-captura_1.jpg: Captura de pantalla del navegador mostrando la aplicación y la URL.

De esta forma hemos podido preparar nuestro entorno local, descargar el código y comprobar la aplicación antes de desplegarla.

#### Desplegar la aplicación
Ahora vamos a desplegar la aplicación en Cloud Run utilizando Cloud SDK. La ventaja de utilizar el Cloud SDK es que podemos desplegar directamente nuestro código y Dockerfile, mientras que utilizando la consola tendríamos que haber generado la imagen de contenedor y subirla a un repositorio de forma manual.

Primero, modifica la aplicación web, modificando la línea final del archivo `main.py`, cambiando `debug=True` por `debug=False`.

Luego despliega tu aplicación usando Cloud SDK desde Cloud Shell o tu instalación local:
1. Ejecuta el comando `gcloud run deploy`:
    1. Cuando te pregunte por la localización del código fuente, pulsa `ENTER` para indicar el directorio actual.
    1. Escoge un nombre de servicio como p. ej. `helloworld`.
    1. Escoge `europe-west1` como región.
    1. Escoge crear un repositorio de Artifact Registry y permitir las invocaciones no autenticadas.
1. Una vez desplegado el servicio, accede a la URL del endpoint mostrado en tu navegador y comprueba tu aplicación.

*ENTREGABLES*:
1. M2U2-8-tarea_1-archivo_2-captura_2.jpg: Captura de pantalla de la aplicación en el navegador mostrando la URL del endpoint de Cloud Run.

### Tarea 2: Configurar una pipeline de CI/CD integrada
Ahora vamos a configurar el servicio/aplicación con una pipeline de CI/CD integrada con Cloud Build para redesplegar la aplicación de forma automática desde un repositorio de código.

Primero crea un repositorio de código:
1. Crea un repositorio en Cloud Source Repositories: `gcloud source repos create helloworld`.
1. Crea un nuevo subdirectorio de trabajo: `mkdir ~/M2U2-8-Cloud_Run/tarea2` y `cd ~/M2U2-8-Cloud_Run/tarea2`.
1. Clona el repositorio de Cloud Source Repositories: `gcloud source repos clone helloworld`.

Luego configura la implementación contínua de Cloud Run:
1. En la consola, navega a **Cloud Run**, pulsa sobre el servicio **helloworld** y luego sobre **Configurar implementación continua**.
1. Como **Proveedor de repositorio**, escoge **Cloud Source Repositories**.
1. Como **Repositorio**, escoge **helloworld**.
1. Como **Rama**, escoge __.*__ para seleccionar cualquier rama.
1. Escoge **Dockerfile** y como localización **/Dockerfile**.
1. Pulsa **Guardar**.

Por último, actualiza la aplicación y ejecuta la pipeline de CI/CD con un commit al repositorio:
1. Copia los archivos de la aplicación al nuevo subdirectorio:
    1. `cp ~/M2U2-8-Cloud_run/tarea_1/Dockerfile .`.
    1. `cp ~/M2U2-8-Cloud_run/tarea_1/.dockerignore .`.
    1. `cp ~/M2U2-8-Cloud_run/tarea_1/requirements.txt .`.
    1. `cp ~/M2U2-8-Cloud_run/tarea_1/main.py .`.
1. Edita el código de la aplicación `main.py` y añade `- v2` a la respuesta del servidor para dejarlo como `return "Hello {} - v2!".format(name)`.
1. Añade los archivos y haz un commit: `git add .`, `git commit -m "version 2"` y `git push origin master`.
1. En la consola, dentro de la página de detalles del servicio **helloworld** de Cloud Run, selecciona la pestaña **Revisiones** y espera a que finalice el despliegue de la nueva revisión.
1. Una vez desplegado el servicio, accede a la URL del servicio en tu navegador y comprueba tu aplicación.

De esta forma hemos podido desplegar de una forma sencilla una pipeline de CI/CD integrada con Cloud Build.

*ENTREGABLES*:
1. M2U2-8-tarea_1-archivo_3-captura_3.jpg: Captura de pantalla de la aplicación en el navegador mostrando la URL del endpoint de Cloud Run.

### Tarea 3: Actualizar la aplicación
Al igual que en otros servicios, podemos dividir el tráfico entre versiones:

1. En la consola, dentro de la página de detalles del servicio **helloworld** de Cloud Run, selecciona la pestaña **Revisiones**.
1. Pulsa sobre **Administrar el tráfico** y divídelo al 50/50 entre ambas revisiones.
1. Una vez actualizado, accede a la URL del servicio en tu navegador y comprueba que redirige a ambas revisiones con un ratio 50/50.

De esta forma hemos podido gestionar el tráfico de nuestra aplicación dividiéndolo entre varias revisiones, para hacer actualizaciones canary o tests A/B.

## Resumen de entregas
1. M2U2-8-tarea_1-archivo_1-captura_1.jpg: Captura de pantalla del navegador mostrando la aplicación y la URL.
1. M2U2-8-tarea_1-archivo_2-captura_2.jpg: Captura de pantalla de la aplicación en el navegador mostrando la URL del endpoint de Cloud Run.
1. M2U2-8-tarea_1-archivo_3-captura_3.jpg: Captura de pantalla de la aplicación en el navegador mostrando la URL del endpoint de Cloud Run.

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. (Opcional) Elimina los directorios del entorno de trabajo local.
1. Elimina el servicio de Cloud Run.
1. Elimina los buckets de Cloud Storage creados automáticamente.
1. Elimina los repositorios de Cloud Artifact Registry y Cloud Source Repositories.
