# Automatización con scripts
Unidad M2U7 - Ejercicio 3

## ¿Qué vamos a hacer?
1. Automatizar infraestructura con scripts de Bash y Python.
1. Desplegar scripts automatizados serverless con Cloud Functions.

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

Para responder a todas las preguntas del ejercicio de forma agrupada, puedes crear antes de empezar un archivo a entregar llamado "M2U7-3-preguntas.txt". Recuerda identificar y ordenar cada pregunta seguida de su respuesta.

Encontrarás las preguntas entre el texto en cursiva: *PREGUNTA: ¿Cómo se llama el servicio de instancias de VMs de Google Cloud?*

### Tarea 1: Scripts de bash y Cloud SDK
xxx

#### Opciones de comandos de Cloud SDK
- desplegar vms - con gcloud - labels (varias combinaciones)
- recuperar info con gcloud
- filtrar por label
- format wide - parsear
- format yaml - parsear
- format json - parsear

#### Scripts de bash
- crear SA
- ADC
- capturar argumentos
- parsear salida
- variables con valores de datos respuesta
- ifs
- bucles for
- ejemplo completo - crear/levantar/apagar vms - arg nombre y zona - comprobar si ya están creadas/levantadas/apagadas

### Tarea 2: Scripts de Python
1. formats - flags - verbose - labels
1. script python
1. script bash
1. script para crear vms
1. script para encender y apagar vms
1. SA y ADC
1. subir script a CF y llamar directamente o con Cloud Scheduler

## Resumen de entregas
1. M2U7-3-preguntas.txt: Respuestas a todas las preguntas planteadas en el ejercicio.
1. [nombre de archivo]: descripción

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. paso1
1. paso2
