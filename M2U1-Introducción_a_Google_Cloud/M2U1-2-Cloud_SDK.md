# Cloud SDK
Unidad M2U1 - Ejercicio 2

## ¿Qué vamos a hacer?
1. Instalar e inicializar Cloud SDK en local.
1. Administrar componentes.
1. Crear configuraciones.
1. Conectarse a y montar Cloud Shell en local.

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

*NOTA: No hay preguntas para este ejercicio.*

### Tarea 1: Instalar e inicializar Cloud SDK en local
En esta tarea vamos a instalar e inicializar Cloud SDK en tu PC local. Para ello puedes utilizar cualquier SO: Linux, macOS o Windows.
Si prefieres no instalar nada en tu PC local, puedes saltarte los pasos que necesiten de una instalación local y seguir el resto utilizando Cloud Shell. Los siguientes ejercicios utilizarán siempre Cloud Shell, aunque también podrás seguirlos desde Cloud SDK local si instalas las herramientas y runtimes de lenguajes necesarios.

También puedes seguir los pasos utilizando...
1. [Windows Subsystem for Linux (WSL)](https://docs.microsoft.com/en-us/windows/wsl/install-win10) para Windows (recomendado).
1. Una VM local con Linux y eliminarla tras acabar.

#### Instalar Cloud SDK
Para instalar Cloud SDK, sigue las instrucciones para tu SO de la [documentación](https://cloud.google.com/sdk/docs/install) (revisa las siguientes notas antes).

*NOTAS:*
1. *No inicialices aún Cloud SDK, lo haremos en el siguiente punto.*
1. *Para Debian/Ubuntu también puedes seguir las instrucciones generales para Linux si prefieres gestionar los componentes de Cloud SDK de forma manual con el versionado de Cloud SDK.*
1. *Para Linux y macOs, es recomendable seguir el paso principal para añadir el Cloud SDK al `$PATH`.*
1. *Las instancias de VM de Compute Engine ya tendrán Cloud SDK instalado*.

#### Iniciar y autenticarse en Cloud SDK
*NOTA: Puedes seguir estas instrucciones con Cloud Shell.*

Una vez instalado localmente Cloud SDK, debemos inicializarlo como primer paso.

Inicializarlo creará una configuración base para las herramientas de CLI y nos autenticará con nuestra cuenta de usuario ([docs](https://cloud.google.com/sdk/docs/initializing)).

Inicializa Cloud SDK con el comando `gcloud init`, siguiendo las instrucciones de la terminal.

*NOTA: Si te solicita una región y zona para Compute Engine por defecto, utiliza la región de `europe-west1` y la zona de `europe-west1-b`.*

Al acabar, comprueba tu configuración y autenticación, con el comando `gcloud config list` o `gcloud auth list`.

### Tarea 2: Administrar componentes y configuraciones
*NOTA: Puedes seguir estas instrucciones con Cloud Shell.*

En esta tarea instalaremos y actualizaremos componentes de Cloud SDK y veremos cómo crear y administrar varias configuraciones por defecto.

#### Administrar componentes
Cloud SDK está formado por múltiples componentes, como herramientas de CLI, emuladores, plugins para otras herramientas, etc.

Sigue las siguientes instrucciones para administrarlos:

1. Para comprobar rápidamente la versión actualmente instalada de Cloud SDK y sus componentes, utiliza el comando `gcloud --version`.
1. Comprueba los componentes instalados y disponibles con el comando `gcloud components list`.
1. Actualiza los componentes instalados con el comando `gcloud components update` (puede tardar algunos minutos).
1. Instala algún/os nuevo/s componente/s con el comando `gcloud components install ID_COMPONENTE`.
1. Comprueba los componentes disponibles de nuevo con el comando `gcloud components list`.

#### Administrar configuraciones
Cloud SDK utiliza diferentes configuraciones para mantener valores por defecto. Las configuraciones son útiles si necesitas trabajar en proyectos diferentes, con autenticaciones diferentes, etc. ([docs](https://cloud.google.com/sdk/docs/configurations))

Para trabajar con varios proyectos y/o usuarios, una alternativa es Cloud Shell, ya que la instancia es independiente por usuario y seleccionando un proyecto en la consola o en la pestaña de sesión de Cloud Shell podemos cambiar el proyecto activo para nuestros comandos.

Para administrar las configuraciones disponibles, sigue las siguientes instrucciones:
1. Lista las configuraciones disponibles con el comando `gcloud config configurations list` (mostrará sólo una).
1. Comprueba los parámetros de la configuración actual con el comando `gcloud config list`.
1. Para crear una configuración, nos solicitará incluir opcionalmente una región y zona por defecto. Puedes comprobar las regiones y zonas disponibles con los comandos `gcloud compute regions list` y `gcloud compute zones list`, o en la [documentación](https://cloud.google.com/compute/docs/regions-zones).
1. Crea una nueva configuración con el comando `gcloud config configurations create NOMBRE_CONFIG`, siguiendo las instrucciones.
    1. Utiliza la misma ID de proyecto y usuario, ya que no tendrás ningún otro proyecto o usuario disponible.
    1. Utiliza una nueva combinación de región y zona, p. ej.
1. Cambia la configuración activa a la nueva con el comando `gcloud config configurations activate NOMBRE_CONFIG`.
1. Comprueba los parámetros de la nueva configuración activada con el comando `gcloud config list`.
1. Cambia alguna de las propiedades de la nueva configuración, como la región o la zona, con el comando `gcloud config set PROPIEDAD VALOR_PROPIEDAD`.
1. Comprueba los nuevos parámetros de la nueva configuración activada con el comando `gcloud config list`.
1. Lista las configuraciones disponibles con el comando `gcloud config configurations list` (mostrará ambas en esta ocasión).
1. Activa la configuración original.
1. Elimina la nueva configuración con el comando `gcloud config configurations delete NOMBRE_CONFIG`.

### Tarea 3: Conectar Cloud Shell a tu entorno local
En esta tarea vamos a explorar varias posibilidades para conectarse a Cloud Shell desde la terminal local con Cloud SDK y transmitir archivos en ambas direcciones.

#### Conectarse desde local
*NOTA: Para seguir estas instrucciones debes tener instalado Cloud SDK en un entorno UNIX local.*

Podemos conectarnos desde nuestra terminal local a Cloud Shell por SSH para utilizar las herramientas de Cloud Shell como entorno de desarrollo remoto.

1. Abre tu terminal local: Linux, macOS, WSL, VM Linux, etc.
1. Conéctate a Cloud Shell con el comando `gcloud cloud-shell ssh --authorize-session`.
1. Comprueba que cuentas con las herramientas y runtimes de lenguajes de Cloud Shell, como Cloud SDK, Nano, Git, Python, kubectl, etc.
1. Siempre puedes desconectarte de una sesión SSH con el comando `exit`.

#### Conexión de archivos desde local
Para trabajar desde local con Cloud Shell o para copiar archivos entre local y Cloud Shell, podemos utilizar los siguientes comandos para copiar y montar archivos locales en Cloud Shell:

1. Para las siguientes instrucciones vamos a copiar archivos. Si necesitas crear un nuevo archivo en Linux, lo puedes hacer con el comando `echo "hola mundo" > archivo.txt`.
1. Copia un archivo de local a Cloud Shell con el comando `gcloud cloud-shell scp localhost:~/archivo.txt cloudshell:~/archivo.txt`.
1. Copia un archivo de Cloud Shell a local con el comando `gcloud cloud-shell scp cloudshell:~/archivo.txt localhost:~/archivo.txt`.
1. Comprueba los archivos copiados en local y en Cloud Shell con el comando `ls` en el directorio correspondiente.

*Paso opcional:* Monta el directorio `$HOME` de Cloud Shell en un directorio local:
1. Instala "sshfs" siguiendo las instrucciones del repositorio oficial: [github.com/libfuse/sshfs](https://github.com/libfuse/sshfs).
1. Escoge en local un directorio `MOUNT_PATH` (p. ej. `~/cloud-shell`) para montar el `$HOME` de Cloud Shell en tu file-system local.
1. Monta Cloud Shell en tu file-system local con el comando `gcloud cloud-shell get-mount-command MOUNT_PATH`.
1. En Cloud Shell, crea un archivo en tu `$HOME` y luego comprueba que está disponible en local y puedes editarlo también con un editor de texto local.
1. Si quieres desmontar Cloud Shell de tu file-system local, usa el comando `fusermount -u MOUNT_PATH` (Linux) o `unmount mountpoint` (BSD y macOS).

## Resumen de entregas
*No hay entregas que realizar para este ejercicio.*

## Limpiar recursos
*No hay recursos que eliminar tras este ejercicio.*
