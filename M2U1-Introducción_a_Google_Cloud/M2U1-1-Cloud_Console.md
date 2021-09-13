# Cloud Console
Unidad M2U1 - Ejercicio 1

## ¿Qué vamos a hacer?
1. Explorar la consola de Google Cloud.
1. Explorar las opciones de Cloud Shell.

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

*NOTA: No hay preguntas para este ejercicio.*

### Tarea 1: Consola de Google Cloud
En esta tarea explorarás por la consola, nevegarás por las partes principales y establecerás tu configuración deseada.

#### Comprobar el usuario y proyecto
Comienza este ejercicio en la página principal de la consola. Siempre puedes volver a la misma pulsando sobre **Google Cloud Platform** a la izquierda de la barra superior azul.

**Dashboard principal**
1. Comprueba el dashboard principal de la consola, verás que tiene bastante información disponible.
1. Analiza las diferentes tarjetas que lo componen.
1. Localiza las partes más importantes de la consola, que analizaremos en siguientes puntos:
    1. Menú de navegación: menú lateral con botón de "hamburguesa" en la esquina superior izquierda.
    1. Botón para volver al dashboard principal: **Google Cloud Platform** a continuación del anterior.
    1. Menú de selección de proyecto: 3 hexágonos blancos y el nombre del proyecto o "Selecciona un proyecto" a continuación del anterior.
    1. Caja de búsqueda principal para servicios o recursos: a continuación, en el centro de la barra superior azul.
    1. Botón de activar Cloud Shell: botón **>_** en la zona derecha de la barra superior azul.
    1. Notificaciones: botón de campanilla a continuación del anterior.
    1. Preferencias: botón con 3 puntos verticales a continuación del anterior..
    1. Usuario logueado: botón más a la derecha de la barra superior azul, con el avatar de tu cuenta de Google.
    1. Pestañas de **Dashboard, actividad y recomendaciones**, en la zona superior izquierda del dashboard, bajo la barra superior azul.

**Comprobar usuario**
1. En el botón de usuario (mencionado en el punto anterior), comprueba el usuario con el que estás logueado.
1. Si lo necesitas, agrega otra cuenta, loguéate con otra o cierra sesión para entrar de nuevo.

**Comprobar proyecto**
1. En el menú desplegable de selección de proyecto (mencionado en el primer punto de la tarea), comprueba si tienes tu proyecto seleccionado.
1. Comprueba que aparece bajo **Sin organización** si estás utilizando ninguna cuenta de Google Workspace o Cloud Identity para Google Cloud. Si no, tal vez no aparezca el desplegable.
1. Si tienes varios proyectos, opcionalmente puedes pulsar sobre el icono de estrella de tu proyecto para añadirlo a listado de **Destacados**.
1. Recuerda verificar tu usuario y proyecto siempre como primer paso al ir a trabajar en Google Cloud.
1. Si necesitas cambiar de proyecto en alguna ocasión, aquí podrás encontrar la opción.
1. En el dashboard principal de la consola, localiza la tarjeta de **Información del proyecto** y revísala.
    1. Intenta memorizar la ID del proyecto, o recuerda siempre que aquí o en el menú de selección de proyecto podrás encontrarla rápidamente.
    1. Explora el enlace de la tarjeta de **Ir a la configuración del proyecto**.

#### Navegar por los servicios
Ahora vamos a navegar por el menú lateral de servicios:

**Menú de navegación**
1. Localiza de nuevo el menú de navegación lateral.
1. Navega por el mismo con cálma, tomándote tu tiempo. Navega por los distintos servicios, subsecciones, agrupaciones de opciones, etc. Intenta hacerte una buena idea de las opciones disponibles y dónde está localizada las opciones o servicios más comunes.
1. Junto a cada servicio en el menú aparecerá un botón de **Fijar** al pasar el ratón por encima. Fija un par de servicios a la cabecera del menú para probarlo, desfíjalos si quieres y recuerda dicha opción si durante el curso utilizas principalmente algunos servicios o te cuesta encontrar alguno.
1. Recuerda siempre que también puedes utilizar la barra de búsqueda en la barra superior azul para buscar servicios y secciones.
1. Para replegar el menú de nuevo puedes pulsar sobre su botón de "hamburguesa" en la esquina superior izquierda.

Como ejemplos, busca en el menú y con la barra de búsqueda las siguientes secciones:
1. **Compute Engine > Instancias de VM**
1. **Cloud Storage > Navegador**
1. **Red de VPC > Redes de VPC**
1. **IAM y administración > IAM**
1. **Facturación**
1. **IAM y administración > Cuotas**
1. **Compute Engine > Discos**
1. **Red de VPC > Firewall**
1. **Servicios de red > Balanceo de carga**

#### Configuración de la consola
Vamos a configurar la consola para que establezcas tu configuración por defecto:

1. En la barra superior azul, a la derecha, accede a **Configuración y utilidades (3 puntos verticales) > Preferencias**.
1. Navega y explora por las secciones disponibles y establece tus preferencias.
1. Asegúrate de establecer tus preferencias en **Idioma y región**. Para este curso intentaremos incluir en las instrucciones los nombres en español de las secciones y opciones, aunque muchas veces en el trabajo diario puede ser más recomendable usar la consola en inglés para evitar problemas con las traducciones.

Vuelve al dashboard principal para poder configurarlo:
1. Arriba a la derecha del mismo encontrarás el botón **Personalizar**.
1. Configura las tarjetas a tu gusto, activando y desactivándolas, y ordenándolas según tus preferencias. Recuerda que siempre puedes cambiar esta personalización durante el curso.

Dentro de la consola hay múltiples tutoriales disponibles en cada sección. En ocasiones, al utilizar un servicio nuevo, se activará el menú lateral de **Aprendizaje** con tutoriales y/o enlaces a la documentación, que siempre puedes cerrar si no lo quieres utilizar en ese momento.

Para encontrar de nuevo los tutoriales, puedes buscar el botón de **Aprendizaje** en las páginas y servicios donde estén disponibles.

Por ejemplo, busca dicho botón en las siguientes páginas:
1. **Compute Engine > Plantillas de instancias**
1. **Cloud Storage > Navegación**
1. **App Engine > Panel**

#### Habilitar APIs
Vamos a explorar las APIs de los servicios disponibles y cómo habilitarlas:

1. Navega a **APIs y servicios > Panel** y explora el panel principal.
1. Navega a **Bibliotca de API** y explora las opciones disponibles.
1. Para habilitar una API cuando nos lo pidan las instrucciones, debemos buscarla en la **Biblioteca de API** y habilitarla.
1. También podemos buscarla en la barra de búsqueda de la barra superior azul.
1. Busca, explora la información y habilita o comprueba que están ya habilitadas las siguientes APIs:
    1. Compute Engine API
    1. Cloud Storage API
    1. Kubernetes Engine API

### Tarea 2: Cloud Shell
En esta tarea vamos a explorar las posibilidades de Cloud Shell.

#### Activar y comprobar Cloud Shell
Comienza por activar Cloud Shell con su botón en la consola:
1. Recuerda que puedes utilizar Cloud Shell incorporado en la misma ventana de navegador de la consola o en una pestaña diferente.
1. Modifica el tamaño del desplegable de Cloud Shell pulsando sobre su borde superior.
1. Minimiza y maximiza el desplegable con el botón de las 2 flechas, tercer botón a la derecha en la barra blanca de Cloud Shell.
1. Abre Cloud Shell en una pestaña/ventana nueva con el 2º botón a la derecha y comprueba que se ha recuperado tu sesión, y que en la ventana/pestaña de la consola aparece la sesión como transferida (puedes cerrarla en la consola).
1. Para acceder directamente a Cloud Shell en tu navegador, también puedes utilizar el enlace directo: [shell.cloud.google.com](https://shell.cloud.google.com/).

Comprueba la configuración de Cloud Shell. Esta comprobación es recomendable hacerla cada vez que abrimos Cloud Shell:
1. Comprueba que está seleccionado el proyecto correcto. Muchas veces, aunque el proyecto esté seleccionado en la consola, al abrir Cloud Shell no está seleccionado correctamente. Puedes hacerlo de las siguientes maneras:
    1. Comprobando que la ID del proyecto aparece en amarillo en el "prompt" de la terminal y también en amarillo en el texto de bienvenida de la terminal.
    1. Comprobando que la ID del proyecto aparece en el nombre de la pestaña o sesión de la terminal.
    1. Comprobando que la ID del proyecto aparece al ejecutar el comando `gcloud config list`, que contiene toda la configuración del Cloud SDK.
    1. Comprobando que la ID del proyecto aparece como variable de entorno al ejecutar el comando `echo $DEVSHELL_PROJECT_ID`, creada al inicializar Cloud Shell.
1. Para cambiar de proyecto, la opción más sencilla es abrir una nueva pestaña de Cloud Shell:
    1. Abre una nueva pestaña con la misma configuración con el botón de **+** en la barra superior de Cloud Shell.
    1. Abre una nueva pestaña seleccionando un nuevo proyecto con el botón de flecha hacia abajo junto a **+** y seleccionado el proyecto deseado, en la barra superior de Cloud Shell.
    1. Recuerda, al abrir una nueva sesión no tendrás la configuración de la anterior, como historial y *especialmente las variables de entorno*, que tendrás que declarar de nuevo.

Comprueba también los componentes disponibles en Cloud Shell:
1. Comprueba que el Cloud SDK está instalado y actualizado con `gcloud --version`.
1. Comprueba los componentes de Cloud SDK instalados con `gcloud components list`.
    1. Recuerda que puedes instalar o actualizar cualquier componente si es necesario con `gcloud components install ID_COMPONENTE`.
1. Comprueba las herramientas, lenguajes y sus versiones que aparecen en la documentación: [Cloud Shell: herramientas disponibles](https://cloud.google.com/shell/docs/how-cloud-shell-works#tools).
    1. En especial, intenta familiarizarte con un editor de texto de los 3 disponibles: Nano, Vim o Emacs. Si no conoces ninguno, te recomendamos utilizar Nano.
    1. Puedes buscar referencias, documentación y resolver problemas sobre dichos editores buscando la información en Google o Stack Overflow.
1. Comprueba el almacenamiento disponible y sus puntos de montaje con el comando `df`. Recuerda que los 5 GB máx. del directorio `$HOME` es lo único que persiste entre sesiones de Cloud Shell, y que el resto de almacenamiento se reseteará.

#### IDE
Vamos a trabajar con el IDE integrado en Cloud Shell, basado en [Theia](https://theia-ide.org/), a su vez inspirado en Visual Studio Code.

1. Activa el editor:
    1. En el botón de Cloud Shell de un lápiz en la barra superior blanca.
    1. Con la URL directa a [ide.cloud.google.com](https://ide.cloud.google.com).
1. Explora la página de bienvenida del editor, donde podrás acceder a tus "workspaces" o a tu directorio `$HOME` de Cloud Shell.
1. Accede a tu directorio `$HOME` y revisa las opciones del editor.
1. Usando el IDE, crea un nuevo archivo en tu directorio `$HOME` (correspondiente a `/home/NOMBRE_USUARIO`), p. ej. `hola_mundo.txt` con un texto cualquiera.
1. Comprueba que tienes las opciones de tener abierto la terminal de Cloud Shell, el IDE o ambas, jugando con sus botones en la zona superior derecha.
1. Vuelve a la terminal de Cloud Shell y comprueba que puedes acceder al archivo creado con los comandos `ls` y `cat hola_mundo.txt`.
1. Crea un archivo en la terminal de Cloud Shell con el comando `echo "hola mundo de nuevo!" > hola_mundo2.txt`.
1. Revisa el archivo creado con el IDE.
1. Siempre puedes editar un archivo de la terminal de Cloud Shell en el IDE directamente, si lo prefieres a utilizar un editor de texto de CLI como Nano, con el comando `edit hola_mundo2.txt` o `cloudshell edit hola_mundo2.txt`.

De esta forma puedes comprobar cómo trabajar a la vez con la terminal e IDE de Cloud Shell.

#### Acciones de Cloud Shell
Vamos a comprobar algunas acciones disponibles en Cloud Shell:

**Reiniciar la VM**
1. En el menú de 3 puntos verticales de la terminal de Cloud Shell (barra superior blanca), pulsa en **Reiniciar**.

**Subir y descargar archivos**
1. Para subir un archivo, en el menú de 3 puntos verticales de la terminal de Cloud Shell (barra superior blanca), pulsa en **Subir**.
1. Sube cualquier archivo local desde tu PC.
1. Comprueba el archivo con el comando `ls PATH_AL_ARCHIVO`.
    1. Puedes eliminarlo con `rm PATH_AL_ARCHIVO`.
1. Para descargar un archivo, p. ej. uno de los creados anteriormente, puedes optar por:
    1. Desde la terminal de Cloud Shell, en el menú de 3 puntos verticales de la terminal de Cloud Shell (barra superior blanca), pulsa en **Descargar** e introduce el path al archivo o selecciónalo en la lista.
    1. Desde el IDE de Cloud Shell, abre el archivo a descargar y pulsa en **File > Download**.

**Vista previa web**

Para previsualizar cualquier aplicación web que ejecutemos en local en Cloud Shell, podemos hacerlo con la opción correspondiente. Para copmrobarlo:
1. Instala el servidor web Apache 2 con los comandos `sudo apt-get install httpd` y `sudo service apache2 start` (se descartará automáticamente al no persistir nada fuera del `$HOME` tras la sesión).
1. Modifica el puerto vinculado al servidor web Apache modificando el archivo `/etc/apache2/ports.conf`:
    1. `sudo nano /etc/apache2/ports.conf` (el IDE sólo puede editar archivos en el directorio `$HOME`).
    1. Cambia el puerto de `Listen 80` a `Listen 8080`.
    1. Guarda y cierra el archivo con **Control + X, y, ENTER**.
    1. Reinica el servicio con `sudo service apache2 restart`.
1. Comprueba la web por defecto con `curl localhost:8080`
1. Abre la previsualización web con su botón en la barra superior blanca de Cloud Shell, en el puerto 8080.

Recuerda que, puesto que la instancia de Cloud Shell no se ejecuta en local sino que la conexión es por navegador, no podemos conectarnos directamente a la IP de Cloud Shell como si fuera local, sino que tenemos que utilizar esta previsualización para acceder a cualquier puerto o aplicación en ejecución en Cloud Shell.

**Botón "Abrir en Cloud Shell"**

En ocasiones, en la documentación, tutoriales y recursos online puedes encontrar un botón llamado **Open in Cloud Shell** o "Abrir en Cloud Shell".

Este enlace abrirá la terminal de Cloud Shell y clonará el repositorio enlazado para darte acceso rápidamente al mismo y poder seguir dicho tutorial, ejecutar dicha aplicación o script, etc.

**Comandos equivalentes en gcloud**

A lo largo de la consola, en páginas relacionadas con la creación de recursos, encontrarás en ocasiones un enlace al final de la página con el mensaje **línea de comandos equivalente** y **REST equivalente**.

Si abres dichos enlaces, verás el comando de gcloud y un botón para copiarlo a Cloud Shell.

Para comprobarlo:
1. Navega a **Compute Engine > Instancias de VM**.
1. Pulsa en **Crear instancia**.
1. Al final de la página, pulsa en **línea de comandos equivalente**.
1. Pulsa en **Ejecutar en Cloud Shell**.
    1. *NOTA:* No ejecutes el comando para crear la instancia, o recuerda luego eliminarla en la consola.

Dichos comandos tendrán todas las opciones incluidas, incluso algunas que coinciden con los valores por defecto, por lo que generalmente serán mucho más largos de lo habitual.

## Resumen de entregas
*No hay entregas que realizar para este ejercicio.*

## Limpiar recursos
*No hay recursos que eliminar tras este ejercicio.*
