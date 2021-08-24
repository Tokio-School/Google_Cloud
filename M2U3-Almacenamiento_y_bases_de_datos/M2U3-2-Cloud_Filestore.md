# Cloud Filestore
Unidad M2U3

## ¿Qué vamos a hacer?
1. Montar una instancia de Cloud Filestore en varias instancias de VMs a la vez.
1. Copiar datos desde Cloud Storage y local a Cloud Filestore.
1. Trabajar con backups de Cloud Filestore.

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

Para responder a todas las preguntas del ejercicio de forma agrupada, puedes crear antes de empezar un archivo a entregar llamado "M2U3-2-preguntas.txt". Recuerda identificar y ordenar cada pregunta seguida de su respuesta.

Encontrarás las preguntas entre el texto en cursiva: *PREGUNTA: ¿Cómo se llama el servicio de instancias de VMs de Google Cloud?*

### Tarea 1: Montar una instancia de Cloud Filestore en varias instancias de VMs
xxxx

#### Crear una instancia de Cloud Filestore
- crear la instancia

#### Crear las instancias de VM
- crear las 2 instancias

#### Montar la instancia de Cloud Filestore como NFS en las 2 instancias de VM
- conectarse a vm 1
- montar nfs
- repetir para vm 2

#### Comprobar el acceso conjunto
- conectarse a vm 1
- escribir archivo
- comprobar lectura
- conectarse a vm 2
- escribir archivo
- comprobar lectura de ambos

### Tarea 2: Copiar datos desde Cloud Storage y local al NFS compartido
xxx

#### Crear archivos en Cloud Storage
- crear bucket con gsutil
- crear subdirectorio y archivos
- sincronizar con Cloud Storage desde Cloud Shell

#### Sincronizar archivos desde Cloud Storage
- conectarse a vm 1
- rsync desde GCS a NFS
- comprobar
- modificar info
- rsync
- comprobar con gsutil

#### Copiar archivos desde local
- desde cloud shell, crear directorio y archivos
- copiar a NFS con gcloud compute scp
- conectarse a vm 1
- copmrobar

### Tarea 2: Trabajar con backups de Cloud Filestore
xxx

#### Crear un backup de una instancia
- crear backup
- listar backup

#### Restaurar un backup a una instancia
- conectarse a vm 1
- cambiar info
- restaurar backup
- comprobar

## Resumen de entregas
1. M2U3-2-preguntas.txt: Respuestas a todas las preguntas planteadas en el ejercicio.
1. [nombre de archivo]: descripción

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. paso1
1. paso2
