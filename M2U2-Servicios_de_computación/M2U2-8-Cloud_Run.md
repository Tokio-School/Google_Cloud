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

Para responder a todas las preguntas del ejercicio de forma agrupada, puedes crear antes de empezar un archivo a entregar llamado "M2U2-8-preguntas.txt". Recuerda identificar y ordenar cada pregunta seguida de su respuesta.

Encontrarás las preguntas entre el texto en cursiva: *PREGUNTA: ¿Cómo se llama el servicio de instancias de VMs de Google Cloud?*

### Tarea 1: Desplegar una aplicación web
xxx

#### Comprobar app en local
- descargar app y dockerfile
- comprobar app en local

#### Desplegar la aplicación
- build de app o desplegar directamente si se puede
- desplegar app en cloud run (esperar a que se habilite la API)
- comprobar app

### Tarea 2: Configurar una pipeline de CI/CD integrada
xxx

#### Configurar la pipeline
- desplegar pipeline - se puede actualizar o hay que desplegar otro servicio?
- habilitar API de cloud run y CSR
- crear repo de CSR

#### Actualizar la aplicación
- modificar app a nueva versión
- commit a repo
- desplegar sin tráfico
- comprobar app
- enviar 50/50 de tráfico - si no se puede en CI/CD, dividir tráfico manualmente
- comprobar app
- migrar 100% de tráfico a v2
- comprobar app

## Resumen de entregas
1. M2U2-8-preguntas.txt: Respuestas a todas las preguntas planteadas en el ejercicio.
1. [nombre de archivo]: descripción

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. paso1
1. paso2
