# Claves de encriptación
Unidad M2U5 - Ejercicio 4

## ¿Qué vamos a hacer?
1. Utilizar claves de encriptación administradas por el usuario (CMEK) para Cloud Storage y discos persistentes.
1. Utilizar claves de encriptación suministradas por el usuario (CSEK) para Cloud Storage y discos persistentes.
1. Utilizar la API para encriptar y desencriptar información.

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

Para responder a todas las preguntas del ejercicio de forma agrupada, puedes crear antes de empezar un archivo a entregar llamado "M2U0-0-preguntas.txt". Recuerda identificar y ordenar cada pregunta seguida de su respuesta.

Encontrarás las preguntas entre el texto en cursiva: *PREGUNTA: ¿Cómo se llama el servicio de instancias de VMs de Google Cloud?*

### Tarea 1: Crear claves de encriptación administradas por el usuario
- habilitar API
- crear keyring
- crear 2 llaves con rotación automática

### Tarea 2: Claves administradas por el usuario (CMEK)

#### CMEK en Cloud Storage
- dar acceso a GCS SA a usar la clave 1
- crear bucket y establecer CMEK
- subir objeto
- comprobar clave encriptación que usan
- encriptar objeto con 2ª key y subir con gsutil
- comprobar con "gsutil ls -L gs://file_path"
- rotar key 1 manualmente

#### CMEK en discos persistentes
- crear disco y usar CMEK
- crear vm y asignar disco
- comprobar - escribir datos y leer

### Tarea 3: Claves suministradas por el usuario (CSEK)

#### Generar claves
- generar CSEK - nombre incluye bucket y nº pseudo-aleatorio
- guardar clave en archivo

#### CSEK en Cloud Storage
- crear archivo .boto y actualizar con clave
- subir objeto a GCS y listar
- eliminar, descargar y comprobar en local
- generar 2ª clave
- rotar claves en .boto
- subir 2º objeto y listar
- eliminar, descargar y comprobar en local
- reencriptar 1er objeto y resubir
- eliminar CSEK y comprobar acceso a objetos

#### CSEK en discos persistentes
- wrap clave
- crear disco con CSEK wrapped
- crear instancia con CSEK wrapped
- tomar captura con CSEK wrapped

### Tarea 4: Encriptar y desencriptar información con la API
- encriptar con API
- desencriptar con API

## Resumen de entregas
1. M2U0-0-preguntas.txt: Respuestas a todas las preguntas planteadas en el ejercicio.
1. [nombre de archivo]: descripción

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. Eliminar la instancia de VM y discos persistentes.
1. Eliminar el bucket de GCS.
1. Eliminar los objetos en el entorno local.
1. Eliminar las claves de encriptación de Cloud KMS y el archivo `.boto`.
