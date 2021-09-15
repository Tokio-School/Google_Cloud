# Cloud Functions
Unidad M2U2 - Ejercicio 9

## ¿Qué vamos a hacer?
1. Desplegar una función serverless con Cloud Functions.
1. Desplegar una función activada en base a mensajes.
1. Desplegar una función activada en base a eventos.

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

Para responder a todas las preguntas del ejercicio de forma agrupada, puedes crear antes de empezar un archivo a entregar llamado "M2U2-9-preguntas.txt". Recuerda identificar y ordenar cada pregunta seguida de su respuesta.

Encontrarás las preguntas entre el texto en cursiva: *PREGUNTA: ¿Cómo se llama el servicio de instancias de VMs de Google Cloud?*

### Tarea 1: Desplegar una función con endpoint HTTP
En esta tarea vamos a desplegar una función serverless con endpoint para responder a peticiones HTTP.

Antes de comenzar, habilita las APIs de Cloud Functions y Cloud Build:
1. En la consola, navega a **API y servicios > Biblioteca**.
1. Busca las APIs de Cloud Functions y Cloud Build y habilítalas o comprueba que ya están habilitadas.

#### Comprobar la función en local
Primero vamos a comprobar la función en local, lo cual nos permite comprobar nuestro código o realizar pruebas en un entorno de CI/CD antes de desplegar la función en la nube, y tener que esperar a que se despliegue, que el endpoint se actualice, etc.

*Nota:* Te recomendamos utilizar Cloud Shell pero también puedes utilizar un entorno local con Python 3 y las demás dependencias instaladas.

Crea en entorno en local y descarga el código:
1. Crea un directorio de trabajo local, p. ej. `mkdir ~/M2U2-9-Cloud_Functions`.
1. Crea un subdirectorio para la primera tarea, p. ej. `mkdir ~/M2U2-9-Cloud_Functions/tarea_1`.
1. Accede al subdirectorio de la primera tarea, p. ej. `cd ~/M2U2-9-Cloud_Functions/tarea_1`.
1. Crea un script con el contenido del código de la función `hello_http` (sólo el de dicha función y la línea que importa `from flask import escape`), en el archivo `functions/helloworld/main.py` del [repositorio de GitHub](https://github.com/GoogleCloudPlatform/python-docs-samples/blob/HEAD/functions/helloworld/main.py).
1. Crea un entorno virtual de Python: `python3 -m venv env` y `source env/bin/activate`.
1. Instala la librería de Function Frameworks para poder testear la aplicación en un entorno de desarrollo: `pip install functions-framework` (o `pip3`, en función de tu instalación de Python).
1. Crea un archivo de dependencias `requirements.txt` con el contenido del archivo `functions/helloworld/requirements.txt` del [repositorio de GitHub](https://github.com/GoogleCloudPlatform/python-docs-samples/blob/6f17aaaf8962b1ec540ec4afcf61f1c784bcac89/functions/helloworld/requirements.txt).
1. Instala las dependencias de la función especificadas: `pip install -r requirements.txt`.

Comprueba la función ejecutándola en local:
1. Despliega la función en un servidor local con el comando `functions_framework --target hello_ttp --port 8080 --signature_type http`.
1. Ahora comprueba la función:
    1. Desde la línea de comandos: `curl localhost:8080`.
    1. Desde la previsualización web de Cloud Shell o en tu navegador local: `http://localhost:8080`.

De esta forma podemos comprobar nuestra función realizando pruebas en un entorno de desarrollo local o de CI/CD.

*ENTREGABLES*:
1. M2U2-9-tarea_1-archivo_1-captura_1.jpg: Captura de la respuesta del comando `curl localhost:8080`.

#### Desplegar la función en la nube
Por último, vamos a desplegar la función en la nube:

1. En la consola, navega a **Cloud Functions** y pulsa **Crear función**.
1. Revisa las diferentes opciones y crea una función siguiendo estas características:
    - Región: europe-west1.
    - Activador: HTTP
    - Autenticación: Permitir invocaciones sin autenticar.
    - Entorno de ejecución: Python 3.9
    - Código fuente: usa el editor directo para copiar el código del script (incluyendo importaciones) y del archivo de dependencias.
1. Una vez desplegada la función, vamos a comprobarla:
    1. Accede a la URL directamente para recibir el mensaje.
    1. La función permite recibir un nombre dentro de un JSON. Puedes desplegar el menú de la función en la columna **Acciones**, pulsar sobre **Probar función** y enviar un JSON de prueba con la clave `name` y el valor que quieras.

De esta forma podemos desplegar nuestra función en la nube y realizar pruebas desde la consola web.

*ENTREGABLES*:
1. M2U2-9-tarea_1-archivo_2-captura_2.jpg: Captura del navegador con la respuesta de la función, mostrando la URL del endpoint.

### Tarea 2: Desplegar una función activada por mensajes
En esta tarea vamos a desplegar una función serverless con una activación basada en eventos:

#### Comprobar la función en local
De nuevo, primero vamos a comprobar la aplicación en un entorno de desarrollo local:

1. Crea un subdirectorio para la segunda tarea, p. ej. `mkdir ~/M2U2-9-Cloud_Functions/tarea_2`.
1. Accede al subdirectorio de la segunda tarea, p. ej. `cd ~/M2U2-9-Cloud_Functions/tarea_2`.
1. Crea un script con el contenido del código de la función `hello_pubsub` del archivo `functions/helloworld/main.py` del [repositorio de GitHub](https://github.com/GoogleCloudPlatform/python-docs-samples/blob/HEAD/functions/helloworld/main.py).
1. Crea un entorno virtual de Python: `python3 -m venv env` y `source env/bin/activate` (puedes desactivar tu entorno actual con `deactivate` si es necesario).
1. Instala la librería de Function Frameworks en el nuevo entorno virtual: `pip install functions-framework` (o `pip3`, en función de tu instalación de Python).
1. Crea un archivo de dependencias `requirements.txt` con el contenido del archivo `functions/helloworld/requirements.txt` del [repositorio de GitHub](https://github.com/GoogleCloudPlatform/python-docs-samples/blob/6f17aaaf8962b1ec540ec4afcf61f1c784bcac89/functions/helloworld/requirements.txt), o copia el anterior.
1. Instala las dependencias de la función especificadas: `pip install -r requirements.txt`.

Comprueba la función ejecutándola en local:
1. Despliega la función en un servidor local con el comando `functions_framework --target hello_pubsub --port 8080 --signature_type event`.
1. Ahora comprueba la función desde la línea de comandos con una petición `curl` POST y un evento en formato JSON:
```
# 'world' base64-encoded is 'd29ybGQ='
curl localhost:8080 \
  -X POST \
  -H "Content-Type: application/json" \
  -d '{
        "context": {
          "eventId":"1144231683168617",
          "timestamp":"2020-05-06T07:33:34.556Z",
          "eventType":"google.pubsub.topic.publish",
          "resource":{
            "service":"pubsub.googleapis.com",
            "name":"projects/sample-project/topics/gcf-test",
            "type":"type.googleapis.com/google.pubsub.v1.PubsubMessage"
          }
        },
        "data": {
          "@type": "type.googleapis.com/google.pubsub.v1.PubsubMessage",
          "attributes": {
             "attr1":"attr1-value"
          },
          "data": "d29ybGQ="
        }
      }'
```

De esta forma podemos comprobar nuestra función realizando pruebas en un entorno de desarrollo local o de CI/CD.

*ENTREGABLES*:
1. M2U2-9-tarea_2-archivo_1-captura_1.jpg: Captura de la respuesta del comando `curl localhost:8080`.

#### Desplegar la función en la nube
Para desplegar la función, sigue los pasos anteriores para desplegar el código desde la consola web.

1. Despliega una nueva función con estas características:
    - Región: europe-west1.
    - Activador: Cloud Pub/Sub.
        - Crea un nuevo tema de Pub/Sub
    - Copia el código de la función y del archivo `requirements.txt` en el editor en línea.
1. Para comprobar la función, navega a **Pub/Sub**, accede al tema recién creado y pulsa en **Publicar mensajes**.
    1. En el campo de cuerpo del mensaje, añade un atributo `data` con el mensaje `d29ybGQ=` ("mundo" codificado en base64).
    1. Luego, navega a la función de Cloud Functions de vuelta, pulsa sobre **Acciones** para tu función y luego sobre **Ver registros**.

De esta forma hemos desarrollado y desplegado una función activada por mensajes o eventos.

*ENTREGABLES*:
1. M2U2-9-tarea_2-archivo_2-captura_2.jpg: Captura de pantalla de los logs de la función, mostrando la información del mensaje de Cloud Pub/Sub.

### Tarea 3: Desplegar una función activada por eventos de Cloud Storage
Por ultimo, vamos a comprobar y desplegar una función activada en base a eventos de Cloud Storage.

Para ello, repite los pasos anteriores para comprobar y desplegar la función en la nube con lo siguientes cambios o consideraciones:
1. Crea otra subcarpeta y entorno virtual de Python.
1. Utiliza el código de la función `hello_gcs` del mismo archivo de Python anterior.
1. Como `--signature_type`, usa de nuevo `event`.
1. Compruébala en local con:
```
curl localhost:8080 \
  -X POST \
  -H "Content-Type: application/json" \
  -d '{
        "context": {
          "eventId": "1147091835525187",
          "timestamp": "2020-04-23T07:38:57.772Z",
          "eventType": "google.storage.object.finalize",
          "resource": {
             "service": "storage.googleapis.com",
             "name": "projects/_/buckets/MY_BUCKET/MY_FILE.txt",
             "type": "storage#object"
          }
        },
        "data": {
          "bucket": "MY_BUCKET",
          "contentType": "text/plain",
          "kind": "storage#object",
          "md5Hash": "...",
          "metageneration": "1",
          "name": "MY_FILE.txt",
          "size": "352",
          "storageClass": "MULTI_REGIONAL",
          "timeCreated": "2020-04-23T07:38:57.230Z",
          "timeStorageClassUpdated": "2020-04-23T07:38:57.230Z",
          "updated": "2020-04-23T07:38:57.230Z"
        }
      }'
```
1. Crea otra función con activación de eventos de Cloud Storage y crea un bucket de Cloud Storage vinculado a la función.
1. Para testear la función en la nube, sube un archivo al bucket y revisa los logs de la función de Cloud Functions.

*ENTREGABLES*:
1. M2U2-9-tarea_3-archivo_1-captura_1.jpg: Captura de la respuesta del comando `curl localhost:8080`.
1. M2U2-9-tarea_3-archivo_2-captura_2.jpg: Captura de pantalla de los logs de la función, mostrando la información del objeto de Cloud Storage.

## Resumen de entregas
1. M2U2-9-tarea_1-archivo_1-captura_1.jpg: Captura de la respuesta del comando `curl localhost:8080`.
1. M2U2-9-tarea_1-archivo_2-captura_2.jpg: Captura del navegador con la respuesta de la función, mostrando la URL del endpoint.
1. M2U2-9-tarea_2-archivo_1-captura_1.jpg: Captura de la respuesta del comando `curl localhost:8080`.
1. M2U2-9-tarea_2-archivo_2-captura_2.jpg: Captura de pantalla de los logs de la función, mostrando la información del mensaje de Cloud Pub/Sub.
1. M2U2-9-tarea_3-archivo_1-captura_1.jpg: Captura de la respuesta del comando `curl localhost:8080`.
1. M2U2-9-tarea_3-archivo_2-captura_2.jpg: Captura de pantalla de los logs de la función, mostrando la información del objeto de Cloud Storage.

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. (Opcional) Elimina los directorios del entorno de trabajo local.
1. Elimina las funciones de Cloud Functions.
1. Elimina el tema de Cloud Pub/Sub y el bucket de Cloud Storage.
