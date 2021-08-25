# Opciones de acceso privado y Cloud NAT
Unidad M2U5

## ¿Qué vamos a hacer?
1. Crear instancias de VM internas, sin IP externa.
1. Habilitar Private Google Access para poder acceder a servicios como Cloud Storage.
1. Desplegar Cloud NAT para poder acceder a destinos externos sin necesidad de IP externa.

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

Para responder a todas las preguntas del ejercicio de forma agrupada, puedes crear antes de empezar un archivo a entregar llamado "M2U4-6-preguntas.txt". Recuerda identificar y ordenar cada pregunta seguida de su respuesta.

Encontrarás las preguntas entre el texto en cursiva: *PREGUNTA: ¿Cómo se llama el servicio de instancias de VMs de Google Cloud?*

### Tarea 1: Crear el despliegue interno
xxx

#### Crear una red interna
- crear la red custom
- crear 1 subred

#### Crear una instancia de VM interna
- crear instancia interna
- comprobar ssh a la instancia - Cloud IAP, ver en siguiente lab

### Tarea 2: Habilitar acceso privado a servicios de Google Cloud
xxx

#### Subir archivos a un bucket de Cloud Storage
- crear bucket de GCS en consola
- generar y subir archivo desde local con gsutil
- comprobar archivo desde consola
- comprobar que no hay acceso desde instancia - SSH por consola

#### Habilitar Private Google Access
- habilitar en subred
- comprobar de nuevo desde instancia

### Tarea 3: Habilitar NAT a destinos externos
xxx

- intentar conectarse a destinos exteriores desde vm
- intentar actualizar vm con apt-get update - falla para externos, sí puede a paquetes de Google

#### Configurar Cloud NAT
- crear cloud NAT
- crear cloud router
- intentar conectarse a destinos exteriores desde vm
- intentar actualizar vm con apt-get update - falla para externos, sí puede a paquetes de Google
- comprobar ip con la que salen

#### Habilitar registros para Cloud NAT
- habilitar logging
- link a Cloud logging
- generar logs con tráfico desde vm
- comprobar logs

## Resumen de entregas
1. M2U4-6-preguntas.txt: Respuestas a todas las preguntas planteadas en el ejercicio.
1. [nombre de archivo]: descripción

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. paso1
1. paso2
