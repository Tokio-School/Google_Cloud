# Imágenes de disco y de instancia
Unidad M2U2 - Ejercicio 3

## ¿Qué vamos a hacer?
1. Crear imágenes de disco a partir de un disco y de un snapshot.
1. Clonar una instancia a partir de una imagen de disco.
1. Crear una imagen de instancia y usarla para recrear una instancia a partir de una imagen de instancia.

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

*No hay preguntas para este ejercicio.*

### Tarea 1: Imágenes de disco
Vamos a descubrir cómo trabajar con imágenes de disco para recrear discos e instancias.

#### Crear una instancia de VM personalizada
Lo primero que necesitaremos será una instancia de VM personalizada base:

1. En el menú de navegación, navega a **Compute Engine > Instancias de VM**.
1. Pulsa sobre el botón **Crear instancia** para abrir el asistente.
1. Crea una instancia con las siguientes características (*Nota:* siempre que no se indiquen valores para alguna característica significa que uses el valor por defecto):
    - Zona: europe-west1-b.
    - Tipo de máquina: e2-micro.
    - Disco de arranque: Debian GNU/Linux 11.
1. Una vez creada la instancia de VM, conéctate a ella por SSH con el botón **SSH** de la consola o cualquier otra de las vías que hemos visto anteriormente.
1. Vamos a personalizar el disco creando un archivo en el mismo: `echo "hola mundo!" > $HOME/sample_file.txt`.
1. Una vez comprobado todo, puedes cerrar la conexión SSH con `exit`.

#### Crear un snapshot de la instancia
Vamos a crear un snapshot de la instancia, como hicimos en el ejercicio anterior, para poder utilizarla al trabajar con imágenes de disco:

1. Accede a la página del disco de arranque de la instancia. Puedes acceder desde varios puntos:
    1. Navegando a **Compute Engine > Discos** y pulsando sobre el disco correspondiente (mismo nombre que la instancia), o
    1. Navegando a **Compute Engine > Instancias de VM**, pulsando sobre la instancia y, en la página de detalles, sobre su disco de arranque.
1. Crea una instantánea o "snapshot" del disco, con el botón **Crear instantánea** en la página de detalles del disco.
    1. *Nota:* En la sección anterior del listado de discos, también puedes encontrar un botón equivalente en la columna **Acciones**.
1. Crea una instantánea del disco con las siguientes opciones:
    - Disco de origen: disco de arranque de la tercera instancia.
    - Ubicación: multi-regional "eu".

*ENTREGABLE:* M2U2-3-tarea_1-archivo_1-captura_1.jpg: Captura de la sección de instantáneas, mostrando la instantánea creada.

#### Crear una imagen de disco a partir del disco
Vamos a crear una imagen de disco a partir del disco de arranque de la instancia. Con dicha imagen de disco podremos crear nuevos discos e instancias con la misma configuración y datos para recrear, clonar o mover la instancia:

1. En la consola, navega a **Compute Engine > Imágenes** y pulsa **Crear imagen**.
1. Crea una imagen a partir del disco de la instancia con las siguientes características:
    - Origen: disco de arranque de la instancia.
    - Ubicación: multi-regional "eu".
    - Familia: `imagen-app`.
    - Seguir ejecutando instancia.

Con estos sencillos pasos, tras un par de minutos tendremos nuestra imagen de disco disponible.

#### Crear una imagen de disco a partir del snapshot de la instancia
Ahora vamos a comprobar cómo podemos crear una imagen también a partir de un snapshot de disco:

1. En la consola, navega a **Compute Engine > Imágenes** y pulsa **Crear imagen**.
1. Crea una imagen a partir del disco de la instancia con las siguientes características:
    - Nombre: dale un nombre diferente a la anterior.
    - Origen: la instantánea o "snapshot" anteriormente creado.
    - Ubicación: multi-regional "eu".
    - Familia: `imagen-app`.

Así tendremos una segunda imagen disponible de la misma familia, en este caso con la misma información.

Las familias de imágenes se pueden utilizar para almacenar varias imágenes relacionadas de forma versionada, p. ej. de la misma aplicación.

*ENTREGABLE:* M2U2-3-tarea_1-archivo_2-captura_2.jpg: Captura de la sección de imágenes, mostrando las imagenes creadas.

#### Mover la instancia a otra zona
La operación de mover una instancia a otra zona podemos realizarla de forma manual o automática ([docs](https://cloud.google.com/compute/docs/instances/moving-instance-across-zones)).

Para mover una instancia en ejecución de una zona a otra, sigue los siguientes pasos:
1. Activa Cloud Shell en la consola (asegúrate que tiene el proyecto activo) o utiliza Cloud SDK en local.
1. Para mover la instancia, simplemente ejecuta el comando `gcloud compute instances move NOMBRE_INSTANCIA --zone europe-west1-b --destination-zone europe-west1-c`.
1. La operación llevará un par de minutos. Puedes revisar su estado en la consola, en **Compute Engine > Instancias de VM**.
1. Una vez finalizada, conéctate a ella por SSH.
1. Comprueba la personalización realizada anteriormente con `echo $HOME/sample_file.txt`.
1. Una vez comprobado todo, puedes cerrar la conexión SSH con `exit`.

Con esta opción tan sencilla podemos mover una instancia de VM a un destino en la misma región.

#### Recrear la instancia en otra región
Para "mover" o recrear la instancia en otra región o proyecto, necesitamos seguir los pasos de recreación manual:
1. Para mover una instancia de región en el mismo proyecto, podemos recrear la instancia a partir de un snapshot o imagen.
1. Para mover una instancia de proyecto (independientemente de la zona y región de origen y destino), podemos recrear la instancia únicamente a partir de una imagen, ya que lo snapshots no se pueden compartir con otros proyectos.

Para recrear la instancia en otra región, sigue los siguientes pasos:
1. En el menú de navegación, navega a **Compute Engine > Instancias de VM**.
1. Pulsa sobre el botón **Crear instancia** para abrir el asistente.
1. Crea una instancia con las siguientes características:
    - Zona: europe-west3-c.
    - Tipo de máquina: e2-micro.
    - Disco de arranque: basado en la instantánea creada anteriormente.
    - *Nota:* Fíjate que al seleccionar disco de arranque, puedes hacerlo a partir de las imágenes personalizadas de éste u otros proyectos a los que tengas acceso.
1. Una vez creada la instancia de VM, conéctate a ella por SSH con el botón **SSH** de la consola o cualquier otra de las vías que hemos visto anteriormente.
1. Comprueba la personalización realizada anteriormente con `echo $HOME/sample_file.txt`.
1. Una vez comprobado todo, puedes cerrar la conexión SSH con `exit`.

De esta forma podemos recrear una instancia en cualquier localización o proyecto, a partir de una imagen de disco.

*ENTREGABLE:* M2U2-3-tarea_1-archivo_3-captura_3.jpg: Captura de la sección de instancias, mostrando las instancias movida y recreada.

### Tarea 2: Imágenes de máquina
Las imágenes de máquina son diferentes a las imágenes de disco, ya que aunan la información de la instancia y snapshots de todos los discos. Se utilizan para recrear instancias con toda la información disponible, a diferencia de las imágenes o snapshots *de disco*, que no mantienen la configuración de la instancia, sólo la información de los discos individualmente.

#### Crear una imagen de máquina
Vamos a crear una imagen de máquina a partir de la *máquina* o instancia de VM original en unos sencillos pasos:
1. En la consola, navega a **Compute Engine > Imágenes de máquinas**.
1. Pulsa en **Crear imagen de máquina** y crea una imagen de máquina con las siguientes características:
    - Instancia de VM de origen: instancia de VM previamente creada.
    - Ubicación: multi-regional "eu".
1. La imagen de máquina tardará un par de minutos en crearse, ya que también tiene que tomar un snapshot de los discos de la instancia.

Una vez creada, ya tendremos una imagen de máquina disponible para recrear, clonar o mover la instancia con toda la información incorporada, de forma más rápida y directa.

*ENTREGABLE:* M2U2-3-tarea_2-archivo_1-captura_1.jpg: Captura de la sección de imágenes de máquina, mostrando la imagen de máquina creada.

#### Recrear la instancia a partir de la imagen de máquina
Para recrear una instancia a partir de una imagen de máquina, podemos hacerlo con la misma configuración original o sustituyendo algunas propiedades, si por ejemplo la misma instancia original sigue en ejecución en la misma zona o si queremos recrearla en otra región, donde debemos cambiar las propiedades regionales (subred, etc.).

Para recrear una instancia modificando algunas propiedades, sigue los sigiuentes pasos:
1. Navega a **Compute Engine > Instancias de VM** y pulsa sobre **Crear nueva instancia**.
1. En la columna de la izquierda selecciona **Instancia nueva de VM a partir de una imagen de máquina**.
1. Selecciona tu imagen de máquina y cambia las siguientes propiedades:
    - Nombre (*nota:* aunque 2 instancias en zonas diferentes pueden tener el mismo nombre, lo cambiaremos por claridad).
    - Región: europe-west3.
1. Crea la instancia.

Con estos sencillos pasos podemos recrear una instancia de VM en otra región o recuperar una instancia ante cualquier problema de la forma más rápida y directa posible.

*ENTREGABLE:* M2U2-3-tarea_2-archivo_2-captura_2.jpg: Captura de la sección de instancias, mostrando las instancias creadas.

## Resumen de entregas
1. M2U2-3-tarea_1-archivo_1-captura_1.jpg: Captura de la sección de instantáneas, mostrando la instantánea creada.
1. M2U2-3-tarea_1-archivo_2-captura_2.jpg: Captura de la sección de imágenes, mostrando las imagenes creadas.
1. M2U2-3-tarea_1-archivo_3-captura_3.jpg: Captura de la sección de instancias, mostrando las instancias movida y recreada.
1. M2U2-3-tarea_2-archivo_1-captura_1.jpg: Captura de la sección de imágenes de máquina, mostrando la imagen de máquina creada.
1. M2U2-3-tarea_2-archivo_2-captura_2.jpg: Captura de la sección de instancias, mostrando las instancias creadas.

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. En **Compute Engine > Instancias de VM**, elimina las instancias de VM creadas.
1. En **Compute Engine > Discos**, asegúrate de que los discos se han eliminado.
1. En **Compute Engine > Imágenes de máquinas**, elimina la imagen de máquina creada.
1. En **Compute Engine > Instantáneas**, elimina el snapshot creado.
1. En **Compute Engine > Imágenes**, elimina la imagen de disco creada.
