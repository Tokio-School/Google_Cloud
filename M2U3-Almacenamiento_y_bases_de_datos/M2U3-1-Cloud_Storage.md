# Cloud Storage
Unidad M2U3

## ¿Qué vamos a hacer?
1. Crear un bucket en Cloud Storage.
1. Gestionar objetos desde la consola web.
1. Gestionar objetos y sincronizar directorios desde la línea de comandos.
1. Gestionar las listas de control de acceso a Cloud Storage.
1. Establecer reglas de ciclo de vida.
1. Habilitar el versionado de objetos

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

Para responder a todas las preguntas del ejercicio de forma agrupada, puedes crear antes de empezar un archivo a entregar llamado "M2U3-1-preguntas.txt". Recuerda identificar y ordenar cada pregunta seguida de su respuesta.

Encontrarás las preguntas entre el texto en cursiva: *PREGUNTA: ¿Cómo se llama el servicio de instancias de VMs de Google Cloud?*

### Tarea 1: Gestionar buckets y objetos
xxx

#### Crear un bucket desde la consola
- crear bucket desde la consola
- ver opciones
- establecer acceso granular

#### Gestionar objetos desde la consola
- subir archivo - imagen o texto
- visualizar
- descargar
- eliminar

#### Gestionar objetos con gsutil
- crear archivo - texto
- subir archivo
- visualizar
- descargar
- eliminar

#### Sincronizar directorios
- crear subdirectorio
- crear 2 archivos de texto
- crear subsubdirectorio
- crear 2 archivos de texto
- sincronizar con gsutil rsync
- comprobar en consola y gsutil - no existen subdirectorios
- eliminar en local un arcihvo y crear otro
- sincronizar de nuevo
- comprobar en consola

### Tarea 2: Gestionar las ACL a nivel granular
xxx

#### A nivel de bucket
- revisar permisos
- cambiar permisos - otra cuenta
- eliminar permisos

#### A nivel de objeto
- revisar permisos
- cambiar permisos - otra cuenta
- eliminar permisos

#### Hacer objeto público
- cambiar permisos
- revisar permisos, url pública, navegador privado
- eliminar permisos

### Tarea 3: Reglas de ciclo de vida y versionado de objetos
xxx

#### Reglas de ciclo de vida
- establecer con consola
- 30 días a nearline, 90 días a coldline, 1 año a archive, eliminar a 4 años
- comprobar con gsutil

#### Habilitar versionado de objetos
- habilitar versionado de objetos - se podrá con consola o sólo gsutil?
- subir más versión de archivo
- comprobar
- establecer regla de ciclo de vida
- subir otra versión
- ver cómo se eliminan versiones antiguas

## Resumen de entregas
1. M2U3-1-preguntas.txt: Respuestas a todas las preguntas planteadas en el ejercicio.
1. [nombre de archivo]: descripción

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. paso1
1. paso2
