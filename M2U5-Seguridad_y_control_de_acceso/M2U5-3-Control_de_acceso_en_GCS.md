# Control de acceso en Cloud Storage
Unidad M2U5 - Ejercicio 3

## ¿Qué vamos a hacer?
1. Controlar el acceso con las ACLs a nivel de bucket y objeto.
1. Permitir acceso temporal con las URL firmadas.
1. Permitir subir archivos desde un formulario de una web estática.

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

Para responder a todas las preguntas del ejercicio de forma agrupada, puedes crear antes de empezar un archivo a entregar llamado "M2U0-0-preguntas.txt". Recuerda identificar y ordenar cada pregunta seguida de su respuesta.

Encontrarás las preguntas entre el texto en cursiva: *PREGUNTA: ¿Cómo se llama el servicio de instancias de VMs de Google Cloud?*

### Tarea 1: Controlar acceso con las ACLs

#### Crear cuenta de servicio y bucket de Cloud Storage
- Crear cuenta de servicio
- comprobar permisos a SA
- crear bucket GCS

#### A nivel de bucket
- comprobar acceso
- cambiar con consola
- cambiar con gsutil
- comprobar con gsutil impersonando "gsutil -i "service-account@google.com" ls gs://pub"

#### A nivel de objeto
- comprobar acceso
- cambiar con consola
- cambiar con gsutil
- comprobar con gsutil impersonando "gsutil -i "service-account@google.com" ls gs://pub"

### Tarea 2: URL firmadas
- crear URL firmada
- crear VM con scope sin GCS
- subir objeto a GCS
- comprobar acceso a objeto con URL firmada

### Tarea 3: Documentos de política firmados
- crear signed policy doc
- crear web en local con HTML muestra
- comprobar subida de objeto

## Resumen de entregas
1. M2U0-0-preguntas.txt: Respuestas a todas las preguntas planteadas en el ejercicio.
1. [nombre de archivo]: descripción

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. Eliminar el bucket de Cloud Storage creado.
1. Eliminar la cuenta de servicio creada.
1. Eliminar la web en el entorno local.
