# Integración y despliegue contínuo
Unidad M2U7 - Ejercicio 4

## ¿Qué vamos a hacer?
1. Crear triggers automáticos en un repositorio remoto para ejecutar pipelines de CI/CD.
1. Utilizar pipelines de CI/CD para desplegar una aplicación serverless.
1. Utilizar pipelines de CI/CD para desplegar una aplicación basada en contenedores.
1. Utilizar pipelines de CI/CD para desplegar una aplicación basada en instancias de VM.

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

Para responder a todas las preguntas del ejercicio de forma agrupada, puedes crear antes de empezar un archivo a entregar llamado "M2U7-4-preguntas.txt". Recuerda identificar y ordenar cada pregunta seguida de su respuesta.

Encontrarás las preguntas entre el texto en cursiva: *PREGUNTA: ¿Cómo se llama el servicio de instancias de VMs de Google Cloud?*

### Tarea 1: CI/CD para aplicaciones serverless
xxx

#### Crear un repositorio
- crear repositorio local
- crear un repo en CSR
- inicializar repo local
- push a CSR

#### Crear aplicación de App Engine
- descargar código
- probar app en local
- commit a repo con tag
- crear app
- desplegar app manualmente
- testear app

#### Crear pipeline de CI/CD
- crear pipeline de Cloud Build
- modificar versión de app
- tag y commit a repo
- testear pipeline manualmente
- crear trigger en cloud build
- modificar versión de app
- tag y commit a repo
- comprobar logs de CB

### Tarea 2: CI/CD para contenedores
xxx

#### Crear aplicación contenerizada
- descargar código
- contenerizar app localmente
- probar app en local con docker en cloud shell
- commit a repo con tag
- crear vm con contenedor
- desplegar vm manualmente - gcloud
- testear vm/app

#### Crear pipeline de CI/CD
- crear pipeline de Cloud Build
- modificar versión de app
- tag y commit a repo
- testear pipeline manualmente
- crear trigger en cloud build
- modificar versión de app
- tag y commit a repo
- comprobar logs de CB

### Tarea 3: CI/CD para instancias de VM
xxx

#### Crear aplicación basada en VM
- descargar código
- probar app en local en cloud shell
- commit a repo con tag
- desplegar vm manualmente - gcloud
- testear vm/app

#### Crear pipeline de CI/CD
- crear pipeline de Cloud Build
- modificar versión de app
- tag y commit a repo
- testear pipeline manualmente
- crear trigger en cloud build
- modificar versión de app
- tag y commit a repo
- comprobar logs de CB

## Resumen de entregas
1. M2U7-4-preguntas.txt: Respuestas a todas las preguntas planteadas en el ejercicio.
1. [nombre de archivo]: descripción

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. paso1
1. paso2
unir labs en uno largo