# Discos y snapshots
Unidad M2U2 - Ejercicio 2

## ¿Qué vamos a hacer?
1. Trabajar con el disco de arranque de una instancia de VM.
1. Reasignar el disco de arranque de una instancia a otra instancia diferente.
1. Asignar discos adicionales a una instancia en modo lectura y escritura y sólo lectura a varias instancias a la vez.
1. Ampliar la reserva de almacenamiento de un disco en caliente.
1. Crear snapshots de discos de forma manual y automática y recrear instancias a partir de ellos.
1. Trabajar con SSD locales.

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

Para responder a todas las preguntas del ejercicio de forma agrupada, puedes crear antes de empezar un archivo a entregar llamado "M2U2-2-preguntas.txt". Recuerda identificar y ordenar cada pregunta seguida de su respuesta.

Encontrarás las preguntas entre el texto en cursiva: *PREGUNTA: ¿Cómo se llama el servicio de instancias de VMs de Google Cloud?*

### Tarea 1: Discos de arranque
En esta tarea, vamos a explorar cómo trabajar con discos de arranque o "boot" para nuestras instancias de VM.

#### Crear una instancia de VM
Vamos a crear una instancia de VM, revisar las imágenes públicas disponibles para el disco de arranque, y montar el disco de arranque en otra VM sin eliminar la primera instancia de VM:

1. En el menú de navegación, navega a **Compute Engine > Instancias de VM**.
1. Pulsa sobre el botón **Crear instancia** para abrir el asistente.
1. Crea una instancia con las siguientes características (*Nota:* siempre que no se indiquen valores para alguna característica significa que uses el valor por defecto):
    - Zona: europe-west1-b.
    - Tipo de máquina: e2-micro.
    - Disco de arranque: Debian GNU/Linux 10 u 11.
    - Firewall: Permitir tráfico HTTP y HTTPS.
1. Una vez creada la instancia de VM, conéctate a ella por SSH con el botón **SSH** de la consola o cualquier otra de las vías que hemos visto anteriormente.
1. Vamos a personalizar el disco de arranque instalando el servidor web Apache:
    1. Actualiza el listado de paquetes disponibles: `sudo apt-get update`.
    1. Instala el servidor web Apache: `sudo apt-get install apache2`.
    1. Comprueba la instalación y su funcionamiento: `sudo systemctl status apache2`.
    1. Comprueba la web en local: `curl localhost:80`.
1. Una vez comprobado todo, puedes cerrar la conexión SSH con `exit`.

Ahora vamos a crear otra instancia de VM para comprobar cómo mover el disco de arranque de la primera a la segunda:

1. Crea una instancia con las siguientes características:
    - Nombre: usa un nombre diferente a la anterior.
    - Zona: europe-west1-b.
    - Tipo de máquina: e2-micro.
    - Disco de arranque: Debian GNU/Linux 10 u 11.
    - Firewall: Permitir tráfico HTTP y HTTPS.
1. Conéctate por SSH a la segunda instancia y comprueba que funciona correctamente.

Desmontamos el disco de arranque de la primera instancia:

1. Detén la primera imagen seleccionándola y pulsando el botón **DETENER**.
1. *PREGUNTA: ¿Por qué tarda mucho más en detenerse una instancia que en arrancarse? ¿Cuál era el límite para ejecutar scripts de apagado en instancias regulares?.*
1. Espera a que se detenga la instancia (*Nota*: puedes revisar el botón de notificaciones en la campanita o número a la derecha de la barra superior azul).
1. Pulsa sobre el nombre de la instancia de VM para acceder a su página de detalles, y pulsa el botón **Editar** para editarla.
1. En la sección **Boot Disk**, busca el disco de arranque (*Nota:* Tendrá el mismo nombre que la instancia generalmente) y pulsa sobre el botón de **X** para desvincular el disco de arranque de la misma.
1. Guarda los cambios de la instancia pulsando el botón **Guardar** al final de la página.
1. Revisa que el disco de arranque sigue disponible navegando a **Compute Engine > Discos** (o pulsando sobre **Discos** en la columna izquierda), y revisando la columna **En uso por** para dicho disco.

*PREGUNTA: ¿Se puede iniciar la primera instancia sin un disco de arranque asociado?*

Montamos el disco de arranque en la segunda instancia, sustituyendo al disponible:

1. Navega a **Compute Engine > Instancias de VM** y detenla.
1. Una vez detenida, pulsa sobre el nombre de la segunda instancia y pulsa sobre el botón **Editar**.
1. Desvincula su disco de arranque y selecciona el disco de arranque de la primera instancia.
1. Guarda los cambios con el botón **Guardar** al final de la página.
1. Inicia la segunda instancia de nuevo y conéctate a ella por SSH.
1. El servidor web Apache2 debe estar disponible con el comando `curl localhost:80`.

De esta forma has podido comprobar cómo cambiar el disco de arranque de una instancia de VM. Dicho disco se puede usar para crear una nueva instancia, cambiarlo por el de otra instancia o montarlo como disco persistente adicional en otra instancia.

*ENTREGABLES:*
1. M2U2-2-tarea_1-archivo_1-captura_1.jpg: Captura de pantalla de la sección de detalles de la primera instancia, mostrando que no tiene disco de arranque asociado.
1. M2U2-2-tarea_1-archivo_2-captura_2.jpg: Captura de pantalla de la sección de detalles de la segunda instancia, mostrando el disco de arranque asociado.
1. M2U2-2-tarea_1-archivo_3-captura_3.jpg: Captura de pantalla de la sección de discos de Compute Engine, mostrando ambos discos.

### Tarea 2: Discos persistentes adicionales
Vamos a comprobar cómo crear discos persistentes adicionales para una instancia de VM.

#### Crear un disco persistente adicional
1. Comienza a crear una tercera instancia con las siguientes características, sin terminar de crearla:
    - Nombre: usa un nombre diferente a la anterior.
    - Zona: europe-west1-b.
    - Tipo de máquina: e2-micro.
    - Disco de arranque: Debian GNU/Linux 10 u 11.
1. **Administración, seguridad, discos, redes y usuario único > Discos > Discos adicionales**: Pulsa en **Agregar nuevo disco** y revisa las opciones disponibles.
1. Crea un disco persistente adicional con las siguientes características:
    - Tipo equilibrado.
    - Tamaño = 10 GB.
    - Modo: lectura/escritura.

Vamos a montar el disco persistente adicional en la instancia:
1. Una vez creada la instancia, conéctate por SSH.
1. Monta el disco persistente adicional en la instancia:
    1. Lista los discos disponibles con el comando `lsblk`.
    1. Encuentra el nombre del disco no montado, p. ej. "sdb". Lo referiremos como `NOMBRE_DISPOSITIVO`.
    1. Formatea el disco con el comando `sudo mkfs.ext4 -m 0 -E lazy_itable_init=0,lazy_journal_init=0,discard /dev/NOMBRE_DISPOSITIVO` ([docs](https://cloud.google.com/compute/docs/disks/add-persistent-disk#format_and_mount_linux)).
    1. Crea un directorio de montaje en el file-system en la localización que prefieras con p. ej. el comando `sudo mkdir -p /mnt/disks/DIRECTORIO_MONTAJE`.
    1. Antes de continuar, comprueba que se ha creado correctametne con `ls /mnt/disks/`.
    1. Monta el disco en dicho directorio con el comando `sudo mount -o discard,defaults /dev/NOMBRE_DISPOSITIVO /mnt/disks/DIRECTORIO_MONTAJE`.
    1. Configura el acceso de escritura al directorio de montaje con el comando `sudo chmod a+w /mnt/disks/DIRECTORIO_MONTAJE`.
1. Para comprobar que todo funciona correctamente:
    1. Crea un nuevo archivo, p. ej. con el comando `echo "hola mundo!" > /mnt/disks/DIRECTORIO_MONTAJE/sample_file.txt`.
    1. Comprueba el nuevo archivo con el comando `cat /mnt/disks/DIRECTORIO_MONTAJE/sample_file.txt`.
1. Configura el montaje automático del disco al reiniciar la instancia:
    1. Crea un backup de la configuración de `fstab` actual: `sudo cp /etc/fstab /etc/fstab.backup`.
    1. Encuentra el UUID del disco adicional: `sudo blkid /dev/NOMBRE_DISPOSITIVO`. Lo referiremos como `UUID_DISCO_ADICONAL`.
    1. Edita el archivo `/etc/fstab` con el editor de texto que prefieras, p. ej. Nano: `sudo nano /etc/fstab`.
    1. Añade una entrada/línea adicional con la siguiente información: `UUID=UUID_DISCO_ADICONAL /mnt/disks/DIRECTORIO_MONTAJE ext4 discard,defaults,nofail 0 2` (*nota:* la entrada comienza con el texto `UUID=` que no debes modificar, sino `UUID_DISCO_ADICONAL` y `DIRECTORIO_MONTAJE`).
    1. Guarda el archivo y comprueba que se ha editado correctamente: `cat /etc/fstab`.
        1. *Nota:* La operación de guardar y cerrar un archivo en Nano se hace con los pasos **CTRL + X, Y, ENTER**.

Ahora vamos a comprobar cómo se puede ampliar el espacio de almacenamiento reservado para un disco sin detener la instancia o las escrituras:
1. En la terminal SSH, comprueba el tamaño actual del disco adicional con el comando `df -TH` y buscando la entrada correspondiente a `/dev/NOMBRE_DISPOSITIVO`, que será de 10 G apróx.
1. En la consola web, navega a **Compute Engine > Discos**, pulsa sobre el disco de la tercera instancia (actual) y pulsa **Editar** (sin detener la instancia).
1. Cambia el tamaño del disco a un tamaño superior, p. ej. 50 GB.
1. *PREGUNTA: ¿Qué sucede si intentas cambiar el tamaño a uno inferior al actual?*.
1. Para los discos de arranque, en una instancia basada en las imágenes públicas de Compute Engine, el tamaño del file system del disco se ampliará automáticamente si reiniciamos la instancia. Si no queremos reiniciar la instancia, deberíamos seguir los siguientes pasos. Nosotros debemos seguirlos ya que estamos trabajando con un disco persistente adicional, no con uno de arranque ([docs](https://cloud.google.com/compute/docs/disks/working-with-persistent-disks#resize_partitions)):
    1. Utiliza el comando `lsblk` para identificar los discos disponibles y busca el disco adicional.
    1. Utiliza el comando `df -Th` para comprobar cuánto almacenamiento del disco adicional está disponible en el file system.
    1. *PREGUNTA: ¿Hay alguna discrepancia entre ambos valores?*.
    1. Para extender el file system, utiliza el comando `sudo resize2fs /dev/NOMBRE_DISPOSITIVO`.
    1. Comprueba de nuevo el almacenamiento disponible en dicho disco con `df -Th`.
    1. Comprueba que el archivo creado originalmente sigue disponible: `cat /mnt/disks/DIRECTORIO_MONTAJE/sample_file.txt`.

*ENTREGABLES*:
1. M2U2-2-tarea_2-archivo_1-captura_1.jpg: Captura de la sección de detalles de la instancia, mostrando ambos discos persistentes asociados.
1. M2U2-2-tarea_2-archivo_2-captura_2.jpg: Captura de la terminal SSH con únicamente el resultado de los comandos `lsblk`, `sudo blkid /dev/NOMBRE_DISPOSITIVO` y `cat /etc/fstab` (*nota:* puedes limpiar la terminal de otros comandos con `clear`).

#### Montar un disco persistente adicional en varias instancias en modo sólo lectura
Ahora vamos a comprobar cómo podemos montar el mismo disco persistente en más de una instancia en modo sólo lectura. Actualmente no podemos montar un disco en varias instancias en modo lectura/escritura.

1. En la consola, navega a **Compute Engine > Instancias de VM**, pulsa en la tercera instancia y pulsa en **Editar**.
1. En **Discos adicionales**, cambia el modo a "sólo lectura" y guarda los cambios del disco y la instancia.

Ahora monta el disco en la segunda instancia en modo lectura:
1. En la consola, navega a **Compute Engine > Instancias de VM**, pulsa en la segunda instancia y pulsa en **Editar**.
1. En **Discos adicionales**, añade el disco adicional en modo "sólo lectura" y guarda los cambios del disco y la instancia (*Nota:* no te confundas con el disco de arranque liberado anteriormente).
1. Sigue los pasos indicados anteriormente para montar el disco, en este caso en la segunda instancia, con las siguientes instrucciones:
    1. No formatees el disco.
    1. No asignes permisos de lectura al directorio de montaje.
1. Finalmente, ccomprueba que el archivo creado originalmente sigue disponible: `cat /mnt/disks/DIRECTORIO_MONTAJE/sample_file.txt`.

*Nota:* Si surge cualquier problema, puedes intentar...
1. Reiniciar las instancias.
1. Desmontar el disco de la primera instancia y montarlo desde el principio en modo lectura en ambas.
1. *Si todo lo demás falla, no te preocupes, a veces administrar discos en Linux no es sencillo, puedes pasar al siguiente punto.*

*ENTREGABLES*:
1. M2U2-2-tarea_2-archivo_3-captura_3.jpg: Captura de la sección de detalles de la instancia, mostrando ambos discos persistentes asociados para una instancia.
1. M2U2-2-tarea_2-archivo_4-captura_4.jpg: Captura de la sección de detalles de la instancia, mostrando ambos discos persistentes asociados para la otra instancia.
1. M2U2-2-tarea_2-archivo_5-captura_5.jpg: Captura de la terminal SSH con únicamente el resultado de los comandos `lsblk`, `sudo blkid /dev/NOMBRE_DISPOSITIVO` y `cat /etc/fstab` para una instancia.
1. M2U2-2-tarea_2-archivo_6-captura_6.jpg: Captura de la terminal SSH con únicamente el resultado de los comandos `lsblk`, `sudo blkid /dev/NOMBRE_DISPOSITIVO` y `cat /etc/fstab` para la otra instancia.

Para desmontar el disco y dejar las instancias listas para las siguientes tareas, repite estas instrucciones en ambas instancias:
1. Edita el archivo `fstab` con `sudo nano fstab` u otro editor de texto.
1. Elimina la línea de montaje del disco adicional, que tenía esta forma: `UUID=UUID_DISCO_ADICONAL /mnt/disks/DIRECTORIO_MONTAJE ext4 discard,defaults,nofail 0 2`
    1. Puedes encontrar el UUID del disco con los comandos `lsblk` y `sudo blkid /dev/NOMBRE_DISPOSITIVO`.
1. Desmonta el disco con `sudo umount /mnt/disk/DIRECTORIO_MONTAJE`.
1. En la consola, navega a **Compute Engine > Instancias de VM**, edita ambas instancias y en **Discos adicionales** desvincula el disco adicional.

### Tarea 3: Snapshots
Vamos a ver cómo trabajar con snapshots de disco para realizar backups de nuestros discos, tanto de discos de arranque como discos adicionales. Podemos crear snapshots manuales, snapshots automáticos o recurrentes y usar snapshots para recrear un disco o crear una instancia a partir del mismo.

*Nota: Para configurar el montaje automático de un disco adicional, debemos modificar el archivo `/etc/fstab` de una instancia, por lo que debemos eliminar dicha información antes de crear un snapshot que fuéramos a utilizar en el futuro para crear una nueva instancia sin dicho disco.

#### Snapshot manual
Vamos a crear un "snapshot" o instantánea del disco:

1. Accede a la página del disco de arranque de la tercera instancia. Puedes acceder desde varios puntos:
    1. Navegando a **Compute Engine > Discos** y pulsando sobre el disco correspondiente (mismo nombre que la instancia), o
    1. Navegando a **Compute Engine > Instancias de VM**, pulsando sobre la instancia y, en la página de detalles, sobre su disco de arranque.
1. Crea una instantánea o "snapshot" del disco, con el botón **Crear instantánea** en la página de detalles del disco.
    1. *Nota:* En la sección anterior del listado de discos, también puedes encontrar un botón equivalente en la columna **Acciones**.
1. Explora las opciones de la página y crea una instantánea del disco con las siguientes opciones:
    - Disco de origen: disco de arranque de la tercera instancia.
    - Ubicación: multi-regional "eu".

#### Snapshots recurrentes
Ahora vamos a crear una política de snapshot recurrentes y asignarla a dicho disco:

1. Navega a **Compute Engine > Instantáneas** y pulsa **Crear programación de instantáneas**.
1. Explora las opciones y crea una programación con las siguientes características:
    - Región: europe-west1.
    - Ubicación: multi-regional "eu".
    - Frecuencia diaria, de 4:00 a 5:00
1. Ahora navega a **Compute Engine > Discos**, pulsa sobre el disco de arranque de la tercera instantánea y luego sobre **Editar**.
1. En **Programación de instantáneas**, asígnale la programación creada y guarda los cambios.

De esta forma podemos crear snapshots o instantáneas de forma manual y automática para hacer un backup de nuestros datos o clonar una instancia.

*Nota:* Clonaremos instancias a partir de snapshots en el siguiente ejercicio.

*ENTREGABLES*:
1. M2U2-2-tarea_3-archivo_1-captura_1.jpg: Captura de la sección de detalles de la instantánea creada manualmente.
1. M2U2-2-tarea_3-archivo_2-captura_2.jpg: Captura de la sección de detalles del disco, mostrando la programación de instantáneas asociada.


### Tarea 4: Discos SSD locales
Ahora vamos a crear una nueva instancia con un disco SSD local disponible. Recuerda las restricciones de dichos discos SSD locales.

Por causa de dichas restricciones, sólo se puede añadir un disco SSD local al crear la instancia y no se puede utilizar como disco de arranque.

#### Creación de la instancia
1. Crea una nueva instancia con las siguientes características:
    - Nombre: usa un nombre diferente a las anteriores.
    - Zona: europe-west1-b.
    - Tipo de máquina: n1-standard-2 (*nota:* los SSD locales no están disponibles para instancias E2 ni de núcleo compartido).
    - Disco de arranque: Debian GNU/Linux 11.
    - **Administración, seguridad, discos, redes y usuario único > Discos**: Añade 1 disco SSD local NVMe (con 375 GB/disco).

Al igual que para cualquier otro disco, primero debemos formatear y montar el disco, siguiendo pasos similares a los anteriores:
1. Una vez creada la instancia, conéctate por SSH a la misma.
1. Formatea el disco SSD local:
    1. Lista los discos disponibles con el comando `lsblk`.
    1. Encuentra el nombre del disco no montado, p. ej. "nvme0n1". Lo referiremos como `NOMBRE_DISPOSITIVO`.
    1. Formatea el disco con el comando `sudo mkfs.ext4 -F /dev/NOMBRE_DISPOSITIVO` ([docs](https://cloud.google.com/compute/docs/disks/local-ssd#formatindividual)).
1. Monta el disco persistente adicional en la instancia:
    1. Crea un directorio de montaje en el file-system en la localización que prefieras con p. ej. el comando `sudo mkdir -p /mnt/disks/DIRECTORIO_MONTAJE`.
    1. Antes de continuar, comprueba que se ha creado correctametne con `ls /mnt/disks/`.
    1. Monta el disco en dicho directorio con el comando `sudo mount /dev/NOMBRE_DISPOSITIVO /mnt/disks/DIRECTORIO_MONTAJE`.
    1. Configura el acceso de escritura al directorio de montaje con el comando `sudo chmod a+w /mnt/disks/DIRECTORIO_MONTAJE`.
1. Configura el montaje automático del disco al reiniciar la instancia:
    1. Crea un backup de la configuración de `fstab` actual: `sudo cp /etc/fstab /etc/fstab.backup`.
    1. Encuentra el UUID del disco adicional: `sudo blkid /dev/NOMBRE_DISPOSITIVO`. Lo referiremos como `UUID_DISCO_ADICONAL`.
    1. Edita el archivo `/etc/fstab` con el editor de texto que prefieras, p. ej. Nano: `sudo nano /etc/fstab`.
    1. Añade una entrada/línea adicional con la siguiente información: `UUID=UUID_DISCO_ADICONAL /mnt/disks/DIRECTORIO_MONTAJE ext4 discard,defaults,nofail 0 2` (*nota:* la entrada comienza con el texto `UUID=` que no debes modificar, sino `UUID_DISCO_ADICONAL` y `DIRECTORIO_MONTAJE`).
    1. Guarda el archivo y comprueba que se ha editado correctamente: `cat /etc/fstab`.
        1. *Nota:* La operación de guardar y cerrar un archivo en Nano se hace con los pasos **CTRL + X, Y, ENTER**.
1. Para comprobar que todo funciona correctamente:
    1. Crea un nuevo archivo, p. ej. con el comando `echo "hola mundo!" > /mnt/disks/DIRECTORIO_MONTAJE/sample_file.txt`.
    1. Comprueba el nuevo archivo con el comando `cat /mnt/disks/DIRECTORIO_MONTAJE/sample_file.txt`.

#### Flujo de trabajo habitual
Puesto que la información en un disco SSD local no persiste si la instancia se detiene, si la instancia sufre algún problema y no puede recuperarse podemos perder la información del mismo. Por tanto, dichos discos sólo se utilizan como discos de scratch mientras se ejecutan las operaciones y luego se copia la información a un sistema persistente como un disco persistente o Cloud Storage.

Para simular dicho flujo de trabajo, sigue los siguientes pasos:
1. Vuelve a la terminal SSH a la nueva instancia.
1. Crea un archivo original en el disco persistente de la instancia, que simulará un archivo a procesar, p. ej. con el comando `echo "datos a procesar" > $HOME/sample_file.txt`.
1. Copia el archivo de datos a procesar al disco local SSD: `cp $HOME/sample_file.txt /mnt/disks/DIRECTORIO_MONTAJE/sample_file.txt`.
1. Simula el procesado de dichos datos en el disco de scratch añadiendo una nueva línea al mismo `echo "datos procesados" >> /mnt/disks/DIRECTORIO_MONTAJE/sample_file.txt`.
1. Comprueba el archivo procesado: `cat /mnt/disks/DIRECTORIO_MONTAJE/sample_file.txt`.
1. Una vez terminado el procesado de datos, copia el archivo al disco persistente: `cp /mnt/disks/DIRECTORIO_MONTAJE/sample_file.txt $HOME/sample_file.txt`
1. Comprueba el archivo con los datos procesados: `cat $HOME/sample_file.txt`.

*ENTREGABLE:* M2U2-2-tarea_4-archivo_1-captura_1.jpg: Captura de la sección de detalles de la instancia, mostrando el disco persistente de arranque y el disco local SSD.

#### Simulado de detención de VM
Como hemos comentado, una instancia con un disco SSD local no puede detenerse, y si se detiene por algún problema imprevisto, no podrá iniciarse de nuevo, por lo que no podemos "reiniciar" una instancia.

Podemos simular un problema de hardware o cualquier otro que detuviera la instancia deteniéndola desde el SO:
1. En la terminal SSH de la instancia, detenla con el comando `sudo shutdown now`.
    1. *Nota:* Al detener la instancia, inmediatamente se perderá la conexión SSH a la misma.
1. En la consola, en la sección **Compute Engine > Instancias de VM**, verás cómo la nueva instancia aparece detenida y no se puede iniciar de nuevo.
    1. *Nota:* En ocasiones, tardará un poco en llegar a ese estado. Actualiza la página en la consola, recarga la página, entra en los detalles de la instancia, etc., y finalmente se mostrará como tal.

## Resumen de entregas
1. M2U2-2-preguntas.txt: Respuestas a todas las preguntas planteadas en el ejercicio.
1. M2U2-2-tarea_1-archivo_1-captura_1.jpg: Captura de pantalla de la sección de detalles de la primera instancia, mostrando que no tiene disco de arranque asociado.
1. M2U2-2-tarea_1-archivo_2-captura_2.jpg: Captura de pantalla de la sección de detalles de la segunda instancia, mostrando el disco de arranque asociado.
1. M2U2-2-tarea_1-archivo_3-captura_3.jpg: Captura de pantalla de la sección de discos de Compute Engine, mostrando ambos discos.
1. M2U2-2-tarea_2-archivo_1-captura_1.jpg: Captura de la sección de detalles de la instancia, mostrando ambos discos persistentes asociados.
1. M2U2-2-tarea_2-archivo_2-captura_2.jpg: Captura de la terminal SSH con únicamente el resultado de los comandos `lsblk`, `sudo blkid /dev/NOMBRE_DISPOSITIVO` y `cat /etc/fstab` (*nota:* puedes limpiar la terminal de otros comandos con `clear`).
1. M2U2-2-tarea_2-archivo_3-captura_3.jpg: Captura de la sección de detalles de la instancia, mostrando ambos discos persistentes asociados para una instancia.
1. M2U2-2-tarea_2-archivo_4-captura_4.jpg: Captura de la sección de detalles de la instancia, mostrando ambos discos persistentes asociados para la otra instancia.
1. M2U2-2-tarea_2-archivo_5-captura_5.jpg: Captura de la terminal SSH con únicamente el resultado de los comandos `lsblk`, `sudo blkid /dev/NOMBRE_DISPOSITIVO` y `cat /etc/fstab` para una instancia.
1. M2U2-2-tarea_2-archivo_6-captura_6.jpg: Captura de la terminal SSH con únicamente el resultado de los comandos `lsblk`, `sudo blkid /dev/NOMBRE_DISPOSITIVO` y `cat /etc/fstab` para la otra instancia.
1. M2U2-2-tarea_3-archivo_1-captura_1.jpg: Captura de la sección de detalles de la instantánea creada manualmente.
1. M2U2-2-tarea_3-archivo_2-captura_2.jpg: Captura de la sección de detalles del disco, mostrando la programación de instantáneas asociada.
1. M2U2-2-tarea_4-archivo_1-captura_1.jpg: Captura de la sección de detalles de la instancia, mostrando el disco persistente de arranque y el disco local SSD.

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. En **Compute Engine > Instancias de VM**, elimina todas las instancias creadas.
1. En **Compute Engine > Instantáneas**, elimina todos los snapshot y la programación de snapshot.
1. En **Compute Engine > Discos**, asegúrate de que se han eliminado todos los discos al eliminar las instancias, y elimina los que no.
