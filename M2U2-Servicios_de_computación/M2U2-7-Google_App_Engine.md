# Google App Engine
Unidad M2U2 - Ejercicio 7

## ¿Qué vamos a hacer?
1. Desplegar una aplicación web serverless en Google App Engine.
1. Actualizar la aplicación a una nueva versión con un despliegue por pasos.
1. Desplegar una aplicación en el entorno flexible de GAE.
1. Personalizar el runtime de la aplicación en el entorno flexible.

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

Para responder a todas las preguntas del ejercicio de forma agrupada, puedes crear antes de empezar un archivo a entregar llamado "M2U2-7-preguntas.txt". Recuerda identificar y ordenar cada pregunta seguida de su respuesta.

Encontrarás las preguntas entre el texto en cursiva: *PREGUNTA: ¿Cómo se llama el servicio de instancias de VMs de Google Cloud?*

*Nota:* Para comprobar estas aplicaciones en local necesitas tener Python 3 y Docker instalado. Puedes instalarlo en tu entorno de Cloud SDK local o utilizar Cloud Shell (recomendado).

### Tarea 1: App Engine, entorno estándar
Vamos a comenzar desplegando una aplicación web serverless de Python 3 en App Engine.

#### Comprobar la aplicación web
Vamos a clonar el repositorio con un aplicación de ejemplo y comprobarla localmente ([GitHub](https://github.com/GoogleCloudPlatform/python-docs-samples/tree/master/appengine/standard)):
1. Activa tu entorno de Cloud Shell o instalación de Cloud SDK local.
1. Clona el repositorio de GitHub con el código fuente: `git clone https://github.com/GoogleCloudPlatform/python-docs-samples.git`.
1. Accede al directorio con la aplicación de ejemplo: `cd python-docs-samples/appengine/standard_python3/hello_world`.
1. Explora los archivos de configuración y código del repositorio.
1. Prueba la aplicación web en local:
    1. Crea un entorno virtual de Python: `python3 -m venv env` y `source env/bin/activate`
    1. Instala las dependencias: `pip3 install -r requirements.txt`
    1. Despliega la aplicación web en local: `python 3 main.py`.
    1. Comprueba la aplicación web:
        1. Si utilizas Cloud Shell, activa la previsualización en el puerto 8080.
        1. Si utilizas un entorno local, accede en tu navegador a `http://localhost:8080`
    1. Una vez comprobada, detén el servidor web local con `CTRL + C`.

Así podemos explorar cómo desarrollar y comprobar una aplicación web local antes de desplegarla en App Engine.

#### Desplegar la aplicación web
Ahora vamos a desplegar la aplicación web en App Engine:

1. En la consola, navega a **APIs y servicios > Biblioteca** y busca y activa la API de App Engine.
1. Navega a **App Engine > Panel**, pulsa en **Crear aplicación** y sigue las instrucciones para crear una aplicación o entorno de App Engine en europe-west3.
1. Finalmente, vuelve a la terminal y despliegua tu aplicación en GAE: `gcloud app deploy`.
1. Una vez desplegada, accede a la URL que devuelve el comando y comprueba tu aplicación en el navegador.
    1. También puedes recuperar la URL con `gcloud app browse`.

De esta forma tan sencilla podemos desplegar el código de nuestra aplicación web a App Engine entorno estándar.

#### Actualizar la aplicación web
Para actualizar la aplicación web, no tenemos más que modificar el código fuente, comprobar la aplicación en local y redesplegarla en App Engine. En este caso, además, vamos a desplegar una nueva versión sin migrar el tráfico a la misma, vamos a realizar un test A/B con el 50% de tráfico a cada versión y finalmente vamos a migrar todo el tráfico a la nueva versión.

Actualiza tu aplicación web:
1. En la carpeta con la aplicación, edita el archivo `main.py` y cambia la línea `return 'Hello World!'` por `return 'Hello World! - v2'`.
1. Sigue los pasos anteriores para probar de nuevo tu aplicación en local.
1. Una vez comprobada y funcionando correctamente, vamos a desplegarla sin migrar todo el tráfico a la nueva versión `v2` con el comando `gcloud app deploy --version v2 --no-promote`.
1. Cuando se despliegue, navega a **App Engine > Versiones** y comprueba que ambas versiones están desplegadas y el 100% del tráfico está dirigido a la primera versión.
1. Puedes pulsar sobre la primera y segunda versión para que se abra la URL en tu navegador y comprobarlas ambas.
1. *PREGUNTAS:*
    - *¿Cuál es el link canónico de la aplicación, el que utilizaste para acceder a la misma cuando la desplegaste por primera vez, y el que se muestra en `gcloud app deploy`?*
    - *¿Cuál es el link de la segunda versión `v2`? ¿Se puede acceder a la versión por defecto y al resto de versiones en todo momento?*
    - *¿Qué estructura sigue el link canónico de la versión por defecto y de las versiones indicadas explícitamente?*.

Ahora vamos a realizar un test A/B con ambas versiones, dirigiendo el 50% de tráfico a cada versión:
1. En **App Engine > Versiones**, selecciona ambas versiones y pulsa en **Dividir tráfico**.
1. Selecciona **Cookie** como método de división y asigna el 50% del tráfico a cada versión.
1. En **App Engine > Panel** o **App Engine > Servicios**, encuentra el link canónico de la aplicación (sin ninguna versión concreta indicada en el subdominio).
1. Abre una nueva ventana de incógnito y comprueba la aplicación. Repite la operación varias veces, con nuevas ventanas de incógnito, para comprobar que redirecciona a ambas aplicaciónes (*nota:* usamos las ventanas de incógnito porque la sesión se mantiene a una versión única por afinidad con una cookie incluida automáticamente en la primera respuesta).

Finalmente, una vez comprobada la nueva versión, vamos a migrar todo el tráfico a la nueva versión:
1. En **App Engine > Versiones**, selecciona la versión `v2` y pulsa en **Migrar tráfico** para comenzar a migrarlo.
1. Una vez migrado, accede a la aplicación por el enlace canónico a la versión por defecto y comprueba que ahora siempre envía a la nueva versión.

En esta ocasión hemos desplegado una nueva versión sin redireccionarle tráfico para poder dividirlo y migrarlo posteriormente, pero si queremos que automáticamente todo el tráfico se redireccione a la nueva versión cuando la actualicemos, podemos hacerlo con el comando `gcloud app deploy --version NOMBRE_VERSION` sin la opción `--no-promote`.

*ENTREGABLES*:
1. M2U2-7-tarea_1-archivo_1-captura_1.jpg: Captura de la sección de versiones mostrando ambas versiones.
1. M2U2-7-tarea_1-archivo_2-captura_2.jpg: Captura del navegador web mostrando la respuesta de la aplicación web y su URL.

### Tarea 2: App Engine, entorno flexible
Ahora vamos a desplegar una aplicacón con runtime personalizado basado en un contenedor con NGINX en el entorno flexible, desplegándola con un Dockerfile.

#### Comprobar la aplicación en local
*NOTA:* Para este paso necesitamos Docker instalado en local, por lo que se recomienza Cloud Shell.

Para comprobar la aplicación en local, sigue los siguientes pasos ([GitHub](https://github.com/GoogleCloudPlatform/appengine-custom-runtimes-samples/tree/master/nginx)):
1. Clona el repositorio en local, en una carpeta fuera del repositorio del paso anterior:
    1. P. ej., vuelve al directorio de usuario `cd $HOME`.
    1. Clona el repositorio: `git clone https://github.com/GoogleCloudPlatform/appengine-custom-runtimes-samples.git`.
1. Accede al directorio con la aplicación de ejemplo: `cd appengine-custom-runtimes-samples/nginx`.
1. Construye una imagen de contenedor con el Dockerfile: `docker build -t nginx_app .`
1. Ejecuta el contenedor en local para comprobar la aplicación:
    1. `docker run -d -p 8080:80 nginx_app --name test_app nginx -g 'daemon off;'`
    1. Accede al puerto 8080 en la previsualización de Cloud Shell o en tu navegador si usas un entorno local con `http://localhost:8080`.
    1. Puedes detener el contenedor con `docker stop test_app`.

#### Desplegar la aplicación web
Ahora despliega la aplicación web en un nuevo servicio, para estructurar nuestra aplicación en micro-servicios:
1. Edita el archivo `app.yaml` y añade esta línea: `service: servicio-nginx`
1. Despliega la aplicación como la primera versión de un nuevo servicio con `gcloud app deploy`.
1. Una vez desplegada, accede a la URL que devuelve el comando y comprueba tu aplicación en el navegador.
    1. También puedes recuperar la URL con `gcloud app browse`.
1. Navega a **App Engine > Servicios** y comprueba que hay un nuevo servicio `servicio-nginx` junto al `default`.
1. Navega a **App Engine > Versiones** y comprueba las versiones disponibles de ambos servicios.

De esta forma hemos despledado una aplicación web con un runtime personalizado con un Dockerfile en el entorno flexible de App Engine.

*ENTREGABLES*:
1. M2U2-7-tarea_2-archivo_1-captura_1.jpg: Captura de la sección de versiones mostrando la versión del servicio `servicio-nginx`.
1. M2U2-7-tarea_2-archivo_2-captura_2.jpg: Captura del navegador web mostrando la respuesta de la aplicación web y su URL.

## Resumen de entregas
1. M2U2-7-preguntas.txt: Respuestas a todas las preguntas planteadas en el ejercicio.
1. M2U2-7-tarea_1-archivo_1-captura_1.jpg: Captura de la sección de versiones mostrando ambas versiones.
1. M2U2-7-tarea_1-archivo_2-captura_2.jpg: Captura del navegador web mostrando la respuesta de la aplicación web y su URL.
1. M2U2-7-tarea_2-archivo_1-captura_1.jpg: Captura de la sección de versiones mostrando la versión del servicio `servicio-nginx`.
1. M2U2-7-tarea_2-archivo_2-captura_2.jpg: Captura del navegador web mostrando la respuesta de la aplicación web y su URL.

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. Para eliminar todos los servicios y versiones de App Engine, navega a **App Engine > Configuración** y deshabilita la aplicación. Todos los recursos se eliminarán.
1. Algunos buckets de Cloud Storage se crean automáticamente con el código, artefactos de la aplicación y archivos estáticos. Puedes navegar a **Storage > Navegador** y eliminarlos manualmente.
