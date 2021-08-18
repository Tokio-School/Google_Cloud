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

Para responder a todas las preguntas del ejercicio de forma agrupada, puedes crear antes de empezar un archivo a entregar llamado "M2U2-5-preguntas.txt". Recuerda identificar y ordenar cada pregunta seguida de su respuesta.

Encontrarás las preguntas entre el texto en cursiva: *PREGUNTA: ¿Cómo se llama el servicio de instancias de VMs de Google Cloud?*

### Tarea 1: Contenerizar una aplicación
- descargar app python en cloud shell
- comprobar código y Dockerfile
- probar en local
- crear imagen de contenedor con tag latest
- probar contenedor
- crear registo en GAR
- subir imagen a GAR
- eliminar imagen local
- probar a descargar imagen
- probar ejecución

### Tarea 2: Crear instancia de VM con contenedor
- crear la instancia de vm
- comrpobar app
- ssh a la instancia y comprobar

### Tarea 3: Actualizar la aplicación
- actualizar app en local
- recrear imagen de contenedor
- subir imagen a GAR con tag latest
- actualizar instancia - reiniciar?
- comprobar app

## Resumen de entregas
1. M2U2-5-preguntas.txt: Respuestas a todas las preguntas planteadas en el ejercicio.
1. [nombre de archivo]: descripción

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. paso1
1. paso2
