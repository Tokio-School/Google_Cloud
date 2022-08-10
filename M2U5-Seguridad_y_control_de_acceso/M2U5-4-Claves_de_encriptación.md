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

### Tarea 1: Crear claves de encriptación administradas por el usuario (CMEK)
Como habitualmente, comienza por habilitar la API del servicio, "Cloud KMS API" navegando a **API y servicios > Biblioteca**.

Crea un *llavero* y una *clave* con rotación automática en el mismo siguiendo estas instrucciones: [Creando claves de encriptación simétricas](https://cloud.google.com/kms/docs/creating-keys)

#### CMEK en Cloud Storage
Vamos a utilizar la CMEK para encriptar los datos de Cloud Storage. Para las siguientes instrucciones, básate en la documentación: [Usar CMEK](https://cloud.google.com/storage/docs/encryption/using-customer-managed-keys), para la consola web o CLI:
1. Crea un bucket de GCS y sube un archivo de ejemplo con texto desde local, como hemos visto en anteriores ejercicios, siguiendo el método que prefieras.
1. Asigna la clave al agente de servicio de GCS.
1. Establece la clave como la clave por defecto del bucket.
1. Crea una nueva clave de encriptación en el llavero de Cloud KMS.
1. Rota la clave de encriptación del objeto.
1. Identifica la clave utilizada para encriptar el objeto.

*ENTREGABLE: M2U5-tarea_1-archivo_1-captura_1.jpg: Captura de pantalla de la respuesta del comando, identificando la clave de Cloud KMS utilizada para encriptar el objeto.*

#### CMEK en discos persistentes
Ahora vamos a utilizar la CMEK para encriptar los datos de un disco persistente de Compute Engine. Para las siguientes instrucciones, básate en la documentacion: [Proteger recusos usando claves de Cloud KMS](https://cloud.google.com/compute/docs/disks/customer-managed-encryption)
1. Asigna la clave al agente de servicio de GCE.
1. Crea un disco de arranque encriptado con dicha CMEK.
1. Crea una instancia de VM con dicho disco de arranque.
1. Crea una captura o *snapshot* de dicho disco de arranque encriptado.
1. Conéctate a la instancia y comprueba que puede escribir y leer datos del disco de arranque encriptado.

*ENTREGABLE: M2U5-tarea_1-archivo_2-captura_2.jpg: Captura de pantalla de la consola web, identificando la clave de Cloud KMS utilizada para encriptar el disco de arranque de la instancia de VM.*

### Tarea 2: Claves suministradas por el usuario (CSEK)

#### Generar claves
Primero debemos generar la clave y el archivo de claves para GCE correspondiente:
1. Genera una clave de encriptación RFC4648 en base64, bien la de ejemplo o bien con el script de Bash incluido: [Formato de clave requerido](https://cloud.google.com/compute/docs/disks/customer-supplied-encryption#required_key_format)
1. Genera el archivo de claves JSON siguiendo este ejemplo: [Archivo de claves](https://cloud.google.com/compute/docs/disks/customer-supplied-encryption#key_file)
Genera

#### CSEK en Cloud Storage
Las CSEK en GCS se utilizan objeto a objeto, no a nivel de bucket, y para interactuar con dichos objetos debemos tener disponibles las claves en local a través del archivo de configuración [boto](https://cloud.google.com/storage/docs/boto-gsutil):
1. Comprueba que el archivo boto existe en tu entorno de CLI ([localización](https://cloud.google.com/storage/docs/boto-gsutil#location)) o recréalo ([Ejemplo usando el archivo de configuración](https://cloud.google.com/storage/docs/boto-gsutil#example)).
1. Añade la clave de encriptación CSEK a tu archivo boto.
1. Sube un nuevo objeto encriptado al bucket con gsutil: [Subida con tu clave de encriptación](https://cloud.google.com/storage/docs/encryption/using-customer-supplied-keys#gsutil).
1. Utiliza gsutil para descargar el objeto encriptado de nuevo: [Descarga los objetos que has encriptado](https://cloud.google.com/storage/docs/encryption/using-customer-supplied-keys#download_objects_youve_encrypted).
1. Comprueba con la consola web cómo no puedes acceder al objeto, ya que la petición debería ir acompañada de la CSEK, lo que no está disponible utilizando la consola web.

*ENTREGABLE: M2U5-tarea_2-archivo_1-captura_1.jpg: Captura de pantalla de la consola web, identificando la clave de Cloud KMS utilizada para encriptar el objeto de GCS.*

#### CSEK en discos persistentes
Por último, utiliza la CSEK para encriptar también el contenido de un disco persistente de Compute Engine. Para ello, sigue la documentación: [Encriptar discos con CSEKs](https://cloud.google.com/compute/docs/disks/customer-supplied-encryption):
1. Crea un disco de arranque y luego una instancia o directamente la instancia con un disco de arranque encriptado por dicho archivo JSON conteniendo la CSEK.
1. Comprueba que la instancia de VM puede escribir y leer información en el disco de arranque encriptado.
1. Crea una captura o *snapshot* del disco encriptado.
1. Crea una nueva imagen a partir del disco encriptado.
1. Crea un nuevo disco a partir del snapshot o imagen del disco encriptado.
1. Crea una nueva instancia de VM a partir del nuevo disco.
1. Comprueba que la nueva instancia de VM puede acceder a la información encriptada proveniente del disco de la instancia anterior.

*ENTREGABLE: M2U5-tarea_2-archivo_2-captura_2.jpg: Captura de pantalla de la consola web, identificando la clave de Cloud KMS utilizada para encriptar el disco de arranque de la instancia de VM.*

### Tarea 4: Encriptar y desencriptar información con la API
Por último, vamos a encriptar y desencriptar un archivo local con la clave CMEK creada previamente:
1. Crea un archivo local con un texto de ejemplo.
1. Encripta dicho archivo: [Encriptar datos](https://cloud.google.com/kms/docs/create-encryption-keys?hl=en#encrypt_data)
1. Desencripta y comprueba su contenido: [Desencriptar contenido cifrado](https://cloud.google.com/kms/docs/create-encryption-keys?hl=en#decrypt_ciphertext)

## Resumen de entregas
1. M2U0-0-preguntas.txt: Respuestas a todas las preguntas planteadas en el ejercicio.
1. M2U5-tarea_1-archivo_1-captura_1.jpg: Captura de pantalla de la respuesta del comando, identificando la clave de Cloud KMS utilizada para encriptar el objeto.
1. M2U5-tarea_1-archivo_2-captura_2.jpg: Captura de pantalla de la consola web, identificando la clave de Cloud KMS utilizada para encriptar el disco de arranque de la instancia de VM.
1. M2U5-tarea_2-archivo_1-captura_1.jpg: Captura de pantalla de la consola web, identificando la clave de Cloud KMS utilizada para encriptar el objeto de GCS.
1. M2U5-tarea_2-archivo_2-captura_2.jpg: Captura de pantalla de la consola web, identificando la clave de Cloud KMS utilizada para encriptar el disco de arranque de la instancia de VM.

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. Eliminar las instancias de VM, discos persistentes, snapshots e imágenes.
1. Eliminar el bucket de GCS.
1. Eliminar los objetos en el entorno local.
1. Eliminar las claves de encriptación de Cloud KMS y el archivo `.boto`.
