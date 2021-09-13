# Librerías de cliente
Unidad M2U1 - Ejercicio 4

## ¿Qué vamos a hacer?
1. Instalar librerías de cliente de Google Cloud Client Libraries y Google APIs.
1. Explorar las opciones de autenticación.
1. Utilizarlas para administrar objetos en Cloud Storage como ejemplo.

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

Para responder a todas las preguntas del ejercicio de forma agrupada, puedes crear antes de empezar un archivo a entregar llamado "M2U1-4-preguntas.txt". Recuerda identificar y ordenar cada pregunta seguida de su respuesta.

Encontrarás las preguntas entre el texto en cursiva: *PREGUNTA: ¿Cómo se llama el servicio de instancias de VMs de Google Cloud?*

### Tarea 1: Google Cloud Client Libraries
En esta primera tarea, vamos a explorar y utilizar las librerías de cliente de Google Cloud.

#### Instalar librería de cliente
Primero explora las librerías disponibles: [Cloud Client Libraries](https://cloud.google.com/apis/docs/cloud-client-libraries).

1. Escoge una librería y explórala en profundidad. Las siguientes instrucciones se referirán a la librería de Python, pero puedes utilizar la que prefieras.
1. Crea un directorio de trabajo en Cloud Shell o en local, p. ej. con `mkdir m2u1-4-librerias_de_cliente && cd m2u1-4-librerias_de_cliente`.
1. Instala la librería de cliente general o de Cloud Storage (depende del lenguaje y librería) en tu entorno. Si utilizas un entorno virtual, créalo en una subcarpeta de dicho directorio de trabajo.
    1. P. ej., las instrucciones para instalar la librería de Cloud Storage para Python: [Python Client for Google Cloud Storage](https://github.com/googleapis/python-storage)

#### Autenticación
En el módulo 5 sobre "Seguridad y control de acceso" veremos en profundidad cómo gestionar la autenticación y autorización de usuarios y aplicaciones.

Por ahora, crearemos una cuenta de servicio como identidad para el script que usa la librería de cliente, y le daremos un acceso amplio al proyecto.

1. Crea una nueva cuenta de servicio con el comando `gcloud iam service-accounts create ID_SA --description="DESCRIPCION_SA" --display-name="NOMBRE_SA"`, sustituyendo los valores indicados en mayúsculas (nota: utiliza comillas en los parámetros si utilizas espacios o carácteres especiales en los mismos).
1. Dale el rol de "editor" a la cuenta de servicio para darle un acceso amplio a realizar acciones en el proyecto, con el comando `gcloud projects add-iam-policy-binding ID_PROYECTO --member="serviceAccount:ID_SA@ID_PROYECTO.iam.gserviceaccount.com" --role="roles/editor"`, sustituyendo los valores indicados en mayúsculas ([docs](https://cloud.google.com/iam/docs/creating-managing-service-accounts#iam-service-accounts-create-gcloud)).
1. Crea y descarga un archivo de credenciales para la cuenta de servicio con el comando `gcloud iam service-accounts keys create NOMBRE_ARCHIVO --iam-account=ID_SA@ID_PROYECTO.iam.gserviceaccount.com` ([docs](https://cloud.google.com/iam/docs/creating-managing-service-account-keys)).
1. Localiza el archivo JSON `NOMBRE_ARCHIVO.json` con las credenciales de la cuenta de servicio descargado.
1. Mueve el archivo `NOMBRE_ARCHIVO.json` al directorio de trabajo que has creado.

#### Script de ejemplo para Cloud Storage
Ahora vamos a ejecutar un pequeño script local que utilice la librería de cliente para crear un bucket de Cloud Storage.

Antes de comenzar, comprueba en la consola web que la API de Cloud Storage está habilitada, o habilítala si es necesario. Para ello, navega a **APIs y servicios > Biblioteca** y busca la API.

1. Crea un archivo y copia el código de ejemplo para crear un bucket de Cloud Storage de la librería que vas a utilizar: [docs](https://cloud.google.com/storage/docs/creating-buckets#storage-create-bucket-code_samples).
    1. Como nombre de bucket, utiliza un nombre globalmente único como `ID_PROYECTO-M2U1-4-tarea1-bucket` o alguna variación para crear varios buckets.
1. Ejecuta el script, p. ej. para python con el comando `python3 NOMBRE_SCRIPT.py`.
    1. Cuidado, el script de python de ejemplo necesita una línea que ejecute la función al final, como `print(create_bucket_class_location('NOMBRE_BUCKET'))`.
1. Comprueba el resultado, *¿qué ha ocurrido?*
    1. Puedes comprobar los buckets disponibles con el comando `gsutil ls`.
    1. Si el script ha funcionado:
        1. *PREGUNTA: ¿En qué proyecto se ha creado?*
        1. *PREGUNTA: ¿Con qué autenticación/usuario se ha ejecutado el script y operación?*
        1. Para ello, puedes comprobar en la pestaña de **ACTIVIDAD** del dashboard principal de la consola quién ha realizado esa acción.
    1. Si no ha funcionado, puede suceder al trabajar en local, es porque no se han establecido unas credenciales por defecto de aplicación o "ADC". Continúa con los siguientes puntos y no te preocupes.
1. Añade unas credenciales específicas para el script: Para ello, sigue las instrucciones de autenticación de la documentación de la librería a la hora de crear el cliente para añadir unas credenciales de cuenta de servicio específicas para la aplicación ([docs](https://cloud.google.com/docs/authentication/production#passing_code)).
1. Ejecuta de nuevo el script y comprueba el resultado. *NOTA: Si previamente se creó el bucket correctamente, necesitarás usar un nombre de bucket diferente.*
    1. *ENTREGABLE*: M2U1-4-tarea_1-archivo_1-script_auth.[py|js|etc]: script con autenticación explícita.
1. Ahora elimina las credenciales del script para establecer unas credenciales por defecto de aplicación o "ADC".
1. Establece las "ADC" con una variable de entorno, p. ej. en Linux `export GOOGLE_APPLICATION_CREDENTIALS="PATH_ARCHIVO_CREDENCIALES"` ([docs](https://cloud.google.com/docs/authentication/production#passing_variable)).
1. Ejecuta de nuevo el script y comprueba el resultado. *NOTA: Si previamente se creó el bucket correctamente, necesitarás usar un nombre de bucket diferente.*
    1. *ENTREGABLE*: M2U1-4-tarea_1-archivo_2-script_adc.[py|js|etc]: script con autenticación por "ADC".
1. Comprueba el resultado de las anteriores acciones en la consola web, navegando a **Storage > Navegador**.

### Tarea 2: Librerías de Google APIs
*BONUS: ¿Te atreverías a repetir los pasos para las librerías de cliente de Google APIs?* [Google API Client Libraries](https://developers.google.com/api-client-library/)

*ENTREGABLE OPCIONAL*: M2U1-4-tarea_2-archivo_1-script.[py|js|etc]: script con autenticación por "ADC" o explícita.

## Resumen de entregas
1. M2U1-4-preguntas.txt: Respuestas a todas las preguntas planteadas en el ejercicio.
1. M2U1-4-tarea_1-archivo_1-script_auth.[py|js|etc]: script con autenticación explícita.
1. M2U1-4-tarea_1-archivo_2-script_adc.[py|js|etc]: script con autenticación por "ADC".
1. (*Opcional*) M2U1-4-tarea_2-archivo_1-script.[py|js|etc]: script con autenticación por "ADC" o explícita.

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. En la consola web, navega a **Storage > Navegador** y elimina todos los buckets creados.
1. En la consola, navega a **IAM > Cuentas de servicio** y elimina la cuenta de servicio creada.
