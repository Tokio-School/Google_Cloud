# Cloud Scheduler
Unidad M2U6

## ¿Qué vamos a hacer?
1. Configurar trabajos cron de mensajes a Cloud Pub/Sub y endpoints HTTPS.
1. Realizar llamadas a APIs de Google Cloud directamente.

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

Para responder a todas las preguntas del ejercicio de forma agrupada, puedes crear antes de empezar un archivo a entregar llamado "M2U6-2-preguntas.txt". Recuerda identificar y ordenar cada pregunta seguida de su respuesta.

Encontrarás las preguntas entre el texto en cursiva: *PREGUNTA: ¿Cómo se llama el servicio de instancias de VMs de Google Cloud?*

### Tarea 1: Configurar trabajos cron
xxx

#### Envío de mensajes por Cloud Pub/Sub
- crear topic p/s
- crear cron job que envíe mensaje p/s - schedule cada minuto/30 s?
- confirmar recepción en consola
- bonus: configurar app GAE que reciba - con código

#### Recepción de mensajes en endpoint serverless
- crear CF
- crear cron job que envíe mensaje CF - schedule cada minuto/30 s?
- confirmar recepción en logs consola
- bonus: configurar llamada a API que cree VM

### Tarea 2: Realizar llamadas a APIs de Google Cloud
xxx

- crear SA
- darle roles a SA
- crear VM
- crear cron job que llama a API para encender VM - autenticación
- crear cron job que llama a API para apagar VM - autenticación
- comprobar jobs manualmente

## Resumen de entregas
1. M2U6-2-preguntas.txt: Respuestas a todas las preguntas planteadas en el ejercicio.
1. [nombre de archivo]: descripción

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. paso1
1. paso2
