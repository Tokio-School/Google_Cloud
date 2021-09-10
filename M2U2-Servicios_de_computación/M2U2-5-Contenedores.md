# Desplegar contenedores en Compute Engine
Unidad M2U2 - Ejercicio 5

## ¿Qué vamos a hacer?
1. Contenerizar una aplicación en local y subirla a un registro de imágenes de contenedor en Artifact Registry.
1. Crear una instancia de VM con el contenedor en ejecución de forma automática.
1. Actualizar la aplicación en la instancia de VM.

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

*Nota: No hay preguntas para este ejercicio.*

*Nota:* Este ejercicio se puede seguir con Cloud Shell o con un entorno local de Cloud SDK que disponga de Docker instalado sobre Linux (Linux, macOS, WSL, etc.).

### Tarea 1: Contenerizar la aplicación
Antes de nada, vamos a comenzar por contenerizar y comprobar la aplicación en local.

#### Comprobar la aplicación
Vamos a clonar el repositorio con la aplicación de ejemplo y comprobarla en local:
1. Puedes revisar el código fuente de la aplicación en el repositorio de [GitHub](https://github.com/docker/labs/tree/master/beginner/flask-app).
1. Clona el repositorio en tu entorno: `git clone https://github.com/docker/labs.git`.
1. Accede al subdirectorio de la aplicación: `cd labs/beginner/flask-app`.
1. Crea un entorno virtual de Python e instala las dependencias de la aplicación en el mismo:
    1. Crea un entorno virtual de Python: `python3 -m venv env` y `source env/bin/activate`.
    1. Instala las dependencias: `pip3 install -r requirements.txt`.
    1. Despliega la aplicación web en local: `python3 app.py`.
    1. Comprueba la aplicación web:
        1. Si utilizas Cloud Shell, activa la previsualización en el puerto 5000.
        1. Si utilizas un entorno local, accede en tu navegador a `http://localhost:5000` (o al puerto correspondiente según la respuesta del servidor web).
    1. Una vez comprobada, detén el servidor web local con `CTRL + C`.

De esta forma hemos descargado y comprobado la aplicación en local.

#### Contenerizar la aplicación
Una vez comprobada la aplicación, vamos a contenerizarla y subirla a un repositorio de imágenes de contenedor en Google Artifact Registry.

Conteneriza la aplicación generando una imagen de contenedor:
1. Revisa el archivo de `Dockerfile` que usaremos para contenerizar la aplicación.
1. Genera una imagen de contenedor de la aplicación `webserver/pywebapp` (o `NOMBRE_REPOSITORIO/NOMBRE_CONTENEDOR`): `docker build -t webserver/pywebapp .`.
1. Comprueba de nuevo la aplicación en local, en esta ocasión contenerizada:
    1. Despliega el contenedor con la aplicación web en local: `docker run -d --name pywebapp -p 8080:5000 webserver/pywebapp`.
    1. Comprueba la aplicación web:
        1. Si utilizas Cloud Shell, activa la previsualización en el puerto 8080.
        1. Si utilizas un entorno local, accede en tu navegador a `http://localhost:8080` (o al puerto correspondiente según la respuesta del servidor web).
    1. Una vez comprobada, detén el contenedor con el servidor web local con `docker stop pywebapp`.

#### Subir la imagen a un repositorio remoto
Crea el repositorio remoto en Google Artifact Registry para subir la imagen y poder descargarla durante la creación de las instancias de VM:
1. En la consola, navega a **Artifact Registry > Repositorios** y pulsa en **Crear repositorio**.
1. Explora las opciones y crea un repositorio con las siguientes características:
    - Nombre: `webserver`.
    - Formato: Docker.
    - Tipo de ubicación: multi-regional "europe".

Sube la imagen al repositorio remoto de Artifact Registry:
1. Configura Docker para que use la autenticación de gcloud al acceder al serviciio de Artifact Registry: `gcloud auth configure-docker europe-docker.pkg.dev`.
    1. *Nota*:* Si el comando responde con un warning arcerca de que ya tiene una autenticación para dicho repositorio, continua sin preocuparte por ello.
1. Etiqueta la imagen de contenedor con el nombre del repositorio remoto para identificarlo: `docker tag webserver/pywebapp europe-docker.pkg.dev/ID_PROYECTO/webserver/pywebapp`.
    1. Comprueba que tienes 2 imágenes de contenedor apuntando a la misma ID de imagen en local: `docker image ls`.
1. Sube la imagen de contenedor al repositorio remoto: `docker push europe-docker.pkg.dev/ID_PROYECTO/webserver/pywebapp`.
1. Comprueba que puedes descargar la imagen: `docker pull europe-docker.pkg.dev/ID_PROYECTO/webserver/pywebapp`.
    1. *Nota:* mostrará que la imagen ya está disponible en local.
1. Comprueba que la imagen está disponible en la consola en **Artifact Registry > Repositorios > webserver**.

De esta forma hemos etiquetado la imagen de contenedor con el nombre del repositorio remoto y lo hemos subido al mismo.

*ENTREGABLES:* M2U2-5-tarea_1-archivo_1-captura_1.jpg: Captura de pantalla del detalle de la imagen de contenedor en la consola (pulsando en el nombre de la imagen de contenedor `pywebapp`).

### Tarea 2: Crear instancia de VM con contenedor
En esta tarea vamos a crear una instancia de VM con la imagen de contenedor desplegada automáticamente.

Primero, localiza la URL de tu imagen de contenedor:
1. En la consola, navega a **Artifact Registry > Repositorios > webserver**.
1. En la zona superior, junto a `pywebapp`, localiza el botón para copiar la URL (2 rectángulos superpuestos) y pulsa sobre el mismo.
    1. Tendrá la forma de `europe-docker.pkg.dev/ID_PROYECTO/webserver/pywebapp`. Comprueba que la URL de la imagen del contenedor se ha copiado al portapapeles correctamente.

Ahora crea la instancia de VM con el contenedor desplegado automáticamente:
1. Navega a **Compute Engine > Instancias de VM** y pulsa sobre **Crear instancia**.
1. En **Contenedor**, confirma **Implementa una imagen de contenedor en esta instancia de VM**, explora las opciones de **Opciones avanzadas de contenedor** e indica las siguientes opciones:
    - Imagen del contenedor: la URL del contenedor antes copiada.
1. Crea la instancia de VM con las siguientes opciones:
    - Zona: europe-west1-b.
    - Tipo de máquina: e2-medium.
1. Crea una regla de cortafuegos para habilitar tráfico al puerto 5000 de la instancia: `gcloud compute firewall-rules create vm-container --allow TCP:5000`.
    1. *Nota:* El Dockerfile expone el puerto 5000 del contenedor, que será también el puerto de la instancia donde escuche.

Tras ello, espera que se cree la instancia para comprobar que la aplicación funciona correctamente:
1. En la página de instancias de VM, pulsa sobre la IP externa de la instancia o cópiala en una pestaña del navegador y comprueba la aplicación web.
    1. Recuerda utilizar el protocolo HTTP y entrar al puerto 5000: `http://IP_EXTERNA_INSTANCIA:5000`.
1. También puedes conectarte por SSH a la instancia y acceder internamente con `curl localhost:5000`.

De esta forma hemos deplegado una instancia que automáticamente descarga la imagen de contenedor y la pone en ejecución.

*ENTREGABLES:*
1. M2U2-5-tarea_2-archivo_1-captura_1.jpg: Captura de pantalla mostrando los detalles de la instancia, donde se vea la URL de la imagen de contenedor.
1. M2U2-5-tarea_2-archivo_2-captura_2.jpg: Captura de pantalla mostrando la aplicación en el navegador y su URL.

### Tarea 3: Actualizar la aplicación
En esta tarea vamos a ver cómo podemos actualizar una instancia con una nueva versión de la aplicación.

Para ello, debemos actualizar la imagen de contenedor, actualizando la aplicación y subiendo una nueva imagen de contenedor al repositorio, y editar y reiniciar la instancia.

Primero, edita el código de la aplicación, contenerízala y súbela al repositorio de nuevo, siguiendo los mismos pasos de tareas anteriores:
1. Edita el archivo `templates/index.html` y añade `<p>versión v2</p>` en una línea entre la lína de `<p><small>Courtesy:...` y la de `</div>`.
1. Conteneriza la aplicación de nuevo con el mismo nombre, `webserver/pywebapp`, lo que le asignará la etiqueta `latest`.
1. Comprueba que dispones de ambas imágenes en local: `docker image ls`.
1. Ejecútala y compruébala en local.
1. Re-etiqueta la imagen de contenedor de `webserver/pywebapp` a `europe-docker.pkg.dev/ID_PROYECTO/webserver/pywebapp` y súbela a repositorio remoto.
1. En la consola, comprueba que están las 2 imágenes disponibles y copia la URL de la nueva imagen de contenedor.

Para actualizar la instancia:
1. Navega a **Compute Engine > Instancias de VM**, detén la instancia y luego iníciala de nuevo para reiniciarla.
1. Una vez reiniciada, comprueba que ha descargado y está ejecutando la nueva versión de la aplicación.

*ENTREGABLES:*
1. M2U2-5-tarea_3-archivo_1-captura_1.jpg: Captura de pantalla mostrando la aplicación en el navegador y su URL.

## Resumen de entregas
1. M2U2-5-tarea_1-archivo_1-captura_1.jpg: Captura de pantalla del detalle de la imagen de contenedor en la consola (pulsando en el nombre de la imagen de contenedor `pywebapp`).
1. M2U2-5-tarea_2-archivo_1-captura_1.jpg: Captura de pantalla mostrando los detalles de la instancia, donde se vea la URL de la imagen de contenedor.
1. M2U2-5-tarea_2-archivo_2-captura_2.jpg: Captura de pantalla mostrando la aplicación en el navegador y su URL.
1. M2U2-5-tarea_3-archivo_1-captura_1.jpg: Captura de pantalla mostrando la aplicación en el navegador y su URL.

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. Elimina la instancia de VM.
1. Elimina el repositorio y las imágenes de contenedor en Artifact Registry.
1. Elimina la regla de cortafuegos `vm-container` en **Red de VPC > Firewall**.
1. Opcional: elimina los contenedores e imágenes de contenedor de tu entorno de local (*nota:* en Cloud Shell se descartarán al cerrarse la sesión).
