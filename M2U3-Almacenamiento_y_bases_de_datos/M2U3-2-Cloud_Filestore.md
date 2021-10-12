# Cloud Filestore
Unidad M2U3 - Ejercicio 2

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

*Nota: No hay preguntas para este ejercicio.*

### Tarea 1: Montar una instancia de Cloud Filestore en varias instancias de VMs
Vamos a crear una instancia de Cloud Filestore y montarla en 2 instancias de VMs en modo lectura/escritura.

#### Crear una instancia de Cloud Filestore
Empezamos por crear una instancia de Cloud Filestore:
1. En la consola, navega a **Filestore > Instancias** y pulsa **Crear instancia**.
1. Explora las opciones y crea una instancia con las siguientes características:
    - ID de la instancia: `nfs-instance`.
    - Tipo de instancia: Básico.
    - Tipo de almacenamiento y capacidad: 1 TB HDD.
    - Zona: europe-west1-b.
    - Rango de IP asignada: automático.
    - Nombre del archivo/volumen compartido: `nfs_volume`.
    - Control de acceso: a todos los clientes de la VPC.

#### Crear las instancias de VM
Crea 2 instancias de VM con las siguientes características:
    - Zona: europe-west1-b.
    - Tipo de máquina: e2-micro.
    - Disco de arranque: basado en Debian 11.
    - Cuenta de servicio: cuenta de servicio por defecto de GCE.
    - Permiso de acceso: configurar el acceso para cada API con acceso de lectura y escritura para Google Storage.

#### Montar la instancia de Cloud Filestore como NFS en las 2 instancias de VM
Ahora conéctate a ambas instancias por SSH y monta el volumen de Cloud Filestore en ambas:
1. Instala NFS: `sudo apt-get update` y `sudo apt-get install -y nfs-common`.
1. Crea un directorio como punto de montaje: `sudo mkdir -p /mnt/nfs`.
1. Monta la instancia en dicho directorio: `sudo mount IP_FILESTORE:/nfs_volume /mnt/nfs`.
1. Habilita permisos de lectura y escritura en el diretorio de montaje: `sudo chmod go+rw /mnt/nfs`.

De esta forma hemos creado una instancia de NFS de Cloud Filestore y la hemos montado en modo R/W en 2 instancias de VM.

#### Comprobar el acceso conjunto
Ahora vamos a comprobar el acceso compartido al volumen NFS desde ambas instancias de VM:

1. Conéctate a la primera instancia por SSH (recomendamos por el botón SSH de la consola y dejar la ventana abierta).
1. Crea un archivo en el NFS compartido: `echo "hola mundo!" > /mnt/nfs/sample_file.txt`.
1. Comprueba la lectura desde la primera instancia: `cat /mnt/nfs/sample_file.txt`.
1. Conéctate a la segunda instancia (de nuevo recomendamos por el botón SSH de la consola y dejar la ventana abierta junto a la primera).
1. Comprueba la lectura desde la segunda instancia: `cat /mnt/nfs/sample_file.txt`.
1. Crea un nuevo archivo en el NFS compartido desde la segunda instancia: `echo "hola mundo! 2" > /mnt/nfs/sample_file2.txt`.
1. Comprueba la lectura desde la primera instancia: `cat /mnt/nfs/sample_file2.txt`.

De esta forma podemos comprobar el acceso a un NFS compartido entre ambas instancias.

*ENTREGAS:* M2U3-2-tarea_1-archivo_1-captura_1.jpg: Captura de pantalla de la página de detalles de la instancia de Cloud Filestore.

### Tarea 2: Copiar datos desde local y Cloud Storage al NFS compartido
Ahora vamos a ver cómo copiar archivos entre local, Cloud Storage y el NFS compartido.

#### Copiar archivos desde local
*Nota:* Puedes seguir las siguientes instrucciones en Cloud Shell o en una instalación local de Cloud SDK.

Vamos a copiar archivos desde un entorno local al NFS compartido directamente:
1. Crea un archivo local: `echo "hola mundo!" > local_file.txt`.
1. Cópialo al NFS a través de una de las instancias: `gcloud compute scp local_file.txt NOMBRE_PRIMERA_INSTANCIA:/mnt/nfs --zone=europe-west1-b`.
1. Conéctate a la segunda instancia por SSH y comprueba el contenido del NFS compartido: `cat /mnt/nfs/local_file.txt`.

De esta forma podemos copiar archivos al NFS a través de alguna de las instancias donde esté montado.

#### Crear archivos en Cloud Storage
Ahora vamos a crear un bucket para sincronizarlo con el NFS compartido:
1. Crea un bucket de GCS con las siguientes características:
    - Nombre: nombre globalmente único.
    - Localización: región de europe-west1.

#### Sincronizar archivos hacia y desde Cloud Storage
Ahora vamos a sincronizar directorios hacia y desde GCS:
1. Conéctate a una de las instancias por SSH.
1. Sincroniza el contenido del NFS con el bucket de GCS: `gsutil -m rsync /mnt/nfs/ gs://NOMBRE_BUCKET`.
1. Comprueba el contenido del bucket: `gsutil ls gs://NOMBRE_BUCKET`.
1. Modifica el contenido del archivo `local_file.txt`:
    1. Hazlo desde el entorno donde lo creaste originalmente, o
    1. Descarga el archivo: `gsutil cp gs://NOMBRE_BUCKET/local_file.txt .`.
    1. Modifíca el contenido.
    1. Cópialo de nuevo al bucket: `gsutil cp local_file.txt gs://NOMBRE_BUCKET/local_file.txt`. 
1. Sincroniza de nuevo el contenido del NFS con el bucket de GCS en la dirección contraria: `gsutil -m rsync gs://NOMBRE_BUCKET /mnt/nfs/`.
1. Desde la otra instancia, comprueba el contenido del NFS compartido: `ls /mnt/nfs`.

De esta forma podemos sincronizar directorios entre el NFS compartido y un bucket de GCS en ambas direcciones.

*ENTREGAS:*
1. M2U3-2-tarea_2-archivo_1-captura_1.jpg: Captura de pantalla de una conexión SSH a la primera instancia mostrando el contenido del directorio `/mnt/nfs`.
1. M2U3-2-tarea_2-archivo_2-captura_2.jpg: Captura de pantalla de una conexión SSH a la segunda instancia mostrando el contenido del directorio `/mnt/nfs`.
1. M2U3-2-tarea_2-archivo_3-captura_3.jpg: Captura de pantalla de una conexión SSH a alguna de las instancias segunda mostrando el contenido del bucket.

### Tarea 3: Trabajar con backups de Cloud Filestore
En esta tarea vamos a trabajar creando y restaurando backups de Cloud Filestore.

#### Crear un backup de una instancia
Vamos a crear un backup con la información del NFS compartido:

1. En la consola, navega a **Filestore > instancias** y, para la instancia creada, en el menú de 3 puntos verticales, pulsa en **Crear copia de seguridad**.
1. Explora las diferentes opciones y crea un backup con las siguientes características:
    1. Región: europe-west1.
1. Una vez creado el backup, aparecerá en **Filestore > copias de seguridad**.

De esta forma podemos crear un backup de la instancia con una copia de seguridad de la información.

#### Restaurar un backup a una instancia
Ahora vamos a restaurar un backup a la instancia de Cloud Filestore creada:

Primero vamos a modificar el contenido del NFS para comprobar los cambios:
1. Accede por SSH a alguna de las instancias de VM.
1. Crea un nuevo archivo para comprobar luego si aparece tras recuperar el backup: `echo "nueva informacion" > /mnt/nfs/new_file.txt`.
1. Comprueba el contenido del archivo: `cat /mnt/nfs/new_file.txt`.

Ahora vamos a restaurar el backup a la instancia ya creada:
1. En la consola, navega a **Filestore > instancias** y pulsa sobre la instancia.
1. Pulsa sobre **Restaurar desde una copia de seguridad** y selecciona el backup creado.
1. Desde una conexión SSH a alguna de las instancias de VM, comprueba el contenido del NFS compartido: `cat /mnt/nfs/new_file.txt` (no debe existir el archivo).

De esta forma podemos restaurar la información desde una copia de seguridad.

## Resumen de entregas
1. M2U3-2-tarea_1-archivo_1-captura_1.jpg: Captura de pantalla de la página de detalles de la instancia de Cloud Filestore.
1. M2U3-2-tarea_2-archivo_1-captura_1.jpg: Captura de pantalla de una conexión SSH a la primera instancia mostrando el contenido del directorio `/mnt/nfs`.
1. M2U3-2-tarea_2-archivo_2-captura_2.jpg: Captura de pantalla de una conexión SSH a la segunda instancia mostrando el contenido del directorio `/mnt/nfs`.
1. M2U3-2-tarea_2-archivo_3-captura_3.jpg: Captura de pantalla de una conexión SSH a alguna de las instancias segunda mostrando el contenido del bucket.

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. Elimina la instancia de Cloud Filestore y el backup.
1. Elimina las instancias de VM.
