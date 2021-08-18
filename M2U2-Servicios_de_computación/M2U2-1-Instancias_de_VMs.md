# Instancias de VMs
Unidad M2U2 - Ejercicio 1

## ¿Qué vamos a hacer?
1. Crear una instancia de VM Linux en la consola web
1. Personalizar la instancia y desplegar una página web
1. Utilizar scripts de inicio y apagado para ejecutar comandos de forma automática al arrancar o detener la instancia
1. Modificar la instancia una vez detenida
1. Crear una instancia Windows Server
1. Explorar los métodos de conexión por SSH y RDP a las instancias

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

Para responder a todas las preguntas del ejercicio de forma agrupada, puedes crear antes de empezar un archivo a entregar llamado "M2U2-1-preguntas.txt". Recuerda identificar y ordenar cada pregunta seguida de su respuesta.

Encontrarás las preguntas entre el texto en cursiva: *PREGUNTA: ¿Cómo se llama el servicio de instancias de VMs de Google Cloud?*

### Tarea 1: Crear una instancia de VM Linux
Vamos a comenzar creando una instancia de VM Linux en el servicio de Google Compute Engine.

#### Crear una instancia de VM Linux
Vamos a crear una instancia de VM Linux y explorar las características disponibles.

1. En la consola web de Google Cloud, navega al servicio "Compute Engine > Instancias de VM", o busca el servicio o la sección en el cuadro de búsqueda de la barra superior azul.
1. Si reenvía a la página de la API de Compute Engine, habilítala o comprueba que esté habilitada, espera a que se habilite y vuelve a la página de "Instancias de VM". Puedes comprobar el progreso de la operación en el botón de notificaciones (icono de campanita o un número a la derecha en la barra superior azul).
1. En la zona superior (o también central si no hay ninguna instancia desplegada), pulsa el botón "Crear instancia".
1. A continuación habrás accedido al menú de creación de instancias. Explora dicho menú para familiarizarte con todas las secciones, menús y opciones disponibles:
    1. Fíjate en las diferentes secciones del menú, como las de nombre, etiquetas y localización, configuración de la máquina, disco de arranque y el menú desplegable final y sus opciones.
    1. Arriba a la derecha verás una estimación del coste mensual de dicha instancia de VM, y los detalles del mismo.
    1. Hay un gran número de configuraciones de máquina disponibles, combinando familia de máquinas, serie y tipo de máquina.
    1. Comienza por mantener la configuración de máquina por defecto (familia: propoósito general, serie: E2, tipo de máquina: e2-medium), cambia la región y fíjate si cambia el precio:
        1. *PREGUNTA: ¿Qué regiones hay disponibles?*
        1. *PREGUNTA: ¿Hay alguna/s región/es notablemente más caras que otras?*
        1. *PREGUNTA: ¿Cuál es la región más cara y la más barata de Europa?*
    1. Ahora selecciona de nuevo la región por defecto de "us-central1" y explora las diferentes configuraciones de máquina disponibles:
        1. Explora primero las familias y series, fijándote en la descripción de cada opción.
        1. Explora ahora los tipos de máquina, fijándote en cómo cambian los precios.
        1. Explora los tipos de máquina personalizados. Prueba varias configuraciones para encontrar sus limitaciones.
        1. Seleccionando una instancia de propósito general E2, en los detalles de costes tienes un enlace a los costes de Compute Engine. Explora los costes mensuales de las instancias "m1-ultramem" y "m2-ultramem", por curiosidad.
        1. Por último, explora varias combinaciones de regiones, zonas y configuración de máquina. *PREGUNTA: ¿Están todas las configuraciones disponibles en todas las regiones y zonas? ¿En qué región y zonas de Europa hay el mayor número de combinaciones disponibles?*
    1. Comprueba los comandos equivalentes para la API REST o Cloud SDK al final de la página. Fíjate que la opción de línea de comandos incluye un botón para copiar e importar ese comando en Cloud Shell, que puede serte útil en alguna ocasión.
1. Crea una instancia de VM con las siguientes características:
    - Nombre: vm-linux
    - Región y zona: europe-west1-b
    - Tipo de máquina: e2-micro
    - Disco de arranque: Debian GNU/Linux
    - Cortafuegos: Permitir tráfico HTTP y HTTPS
    - Deja el resto de opciones con los valores por defecto.
1. Pulsa el botón de crear instancia y espera a que finalice su creación. *PREGUNTA: ¿Cuánto tarda apróx. tarda en crearse una instancia de núcleo compartido?*
1. Cuando la instancia de VM se haya creado, en el listado haz click sobre su nombre para comprobar los detalles de la misma. Explora todas las opciones, tanto las características y configuración de la instancia como los botones de acción superiores.

*ENTREGA: Toma una captura de pantalla de la página de detalles de la instancia.*

#### Explorar los métodos de conexión
Para instancias de VM Linux tenemos 2 métodos de conexión disponibles: SSH y puerto serie, siendo éste último utilizado normalmente sólo para debuguear instancias a las que no nos podemos conectdar por SSH o no se inician correctamente.

Tenemos varias formas de conectarnos por SSH a una instancia Linux:
- Usando la consola web directamente.
- Desde Cloud Shell o Cloud SDK local.
- Usando un cliente o terminal SSH local, desde la terminal de Linux/macOS o p. ej. PuTTy para Windows.

Intenta utilizar las 3 formas para explorar las posibilidades de conexión por SSH a la instancia:
1. **Cloud Console:** En las páginas de listado de instancias y de detalles de la instancia, encontrarás un botón de SSH que te permitirá abrir una terminal en el navegador directamente a la instancia, creando y subiendo un conjunto de claves público-privada.
1. **Cloud SDK/Cloud Shell:** Utiliza el comando "gcloud compute ssh NOMBRE_INSTANCIA" ([referencia](https://cloud.google.com/sdk/gcloud/reference/compute/ssh)).
1. **BONUS: Cliente local/PuTTy:** Crea una clave SSH y añádela manualmente a la instancia ([instrucciones](https://cloud.google.com/compute/docs/instances/connecting-advanced#provide-key)) y úsala para conectarte desde tu terminal local, WSL o PuTTY ([instrucciones](https://cloud.google.com/compute/docs/instances/connecting-advanced#thirdpartytools)).
    1. NOTA: Como nombre de usuario, utiliza tu dirección de correo electrónico excepto "@dominio.com", sólo el nombre.
    1. NOTA: Aunque utilizar OS Login sea lo más recomendable en una organización, por sencillez en el ejercicio utilizaremos en su lugar el método manual.

*ENTREGA: Toma una captura de pantalla de la terminal SSH abierta con la consola web y con Cloud SDK local o Cloud Shell.*

Una vez exploradas las distintas opciones, mantén una ventana de terminal SSH abierta a la instancia para continuar con el siguiente paso.

#### Personalizar la instancia con una página web
Vamos a desplegar un servidor web con su página por defecto como ejemplo de personalización de la instancia.

Usando tu conexión de SSH a la misma, ejecuta los siguientes comandos:
1. Actualiza el listado de paquetes disponibles: "sudo apt-get update"
1. Instala el servidor web Apache: "sudo apt-get install apache2"
1. Comprueba la instalación y su funcionamiento: "sudo systemctl status apache2"
1. Comprueba la web en local: "curl localhost:80"

Por último, comprueba la conexión externa a la web:
1. En la consola, en "Compute Engine > Instancias de VM", accede a la página de detalles de la instancia pulsando sobre su nombre.
1. Comprueba que las reglas de cortafuegos de habilitar tráfico HTTP y HTTPS están activas.
1. Busca el listado de las interfaces de red de la instancia y pulsa en "nic0" o en "ver detalles" de la nic0.
1. Explora los detalles de la interfaz de red de la instancia. Esta página te ayudará a debuguear problemas de conectividad si se presentan en el futuro.
    1. Comprueba que la instancia tiene 2 etiquetas de red asignadas, "http-server" y "https-server".
    1. En cuanto a detalles de reglas de cortafuegos y rutas, revisa las reglas de cortafuegos relacionadas con dichas etiquetas de red ("default-allow-http" y "default-allow-https") y comprueba qué protocolos y puertos habilitan.
    1. En "Análisis de la red > Análisis de entrada", revisa los puertos y protocolos habilitados según origen.
    1. *PREGUNTA: ¿Debería la instancia de VM poder aceptar tráfico externo a su web en el puerto TCP:80?*
1. Vuelve a la página del listado de instancias en "Compute Engine > Instancias de VM", busca la IP externa de la instancia y pulsa sobre la misma para que se abra en otra pestaña de tu navegador.
1. Comprueba que la conexión se realiza por HTTP al puerto 80. Los navegadores actuales generalmente redireccionan a HTTPS por puerto 443 y nuestra web sólo es accesible por HTTP y puerto 80.
1. La página por defecto del servidor Apache se debe mostrar, confirmando que hemos podido personalizar la instancia y permitir su tráfico desde el exterior.

*ENTREGA: Toma una captura de pantalla de la web del servidor, donde se vea también la IP de la instancia en la URL del navegador.*

**BONUS:** Haz un ping desde tu terminal local o Cloud Shell a la IP externa de la instancia para comprobar la conectividad con el comando "ping IP_INSTANCIA".

### Tarea 2: Inicio y apagado de la instancia
Vamos a explorar algunas opciones relacionadas con el inicio y apagado de la instancia, como son los scripts de inicio y apagado y los cambios de configuración disponibles en estado de ejecución y de deteeención.

#### Scripts de inicio y apagado
Los scripts de inicio y apagado se ejecutan al iniciar o apagar la instancia (siempre que sea razonablemente posible) respectivamente.

Los scripts se pueden añadir en el momento de creación de la instancia o en un momento posterior, y se registran como valores en el servidor de metadatos para la instancia.

Primero vamos a comprobar cómo se añadirían en la creación de una nueva instancia y luego los añadiremos a la instancia ya creada:

##### En la creación de una nueva instancia
1. En la consola web, navega a "Compute Engine > Instancias de VM" y pulsa "Crear instancia".
1. Expande el menú desplegable de "Administración, seguridad, discos, redes, usuario único" al final de las opciones.
1. Localiza el cuadro de texto donde se añadiría el script de inicio en "Administración > Automatización > Secuencia de comandos de inicio".
1. El script de apagado se añadiría como clave-valor en un campo de metadatos, de forma manual.
1. No crees la instancia de VM, puesto que en su lugar vamos a añadir los scripts a la instancia ya creada.

##### En una instancia ya creada
1. En "Compute Engine > Instancias de VM", localiza la instancia creada y pulsa en su nombre.
1. Pulsa el botón "Editar".
1. En "Metadatos personalizados", introduce 2 campos, uno para el script de inicio y otro para el script de apagado, indicados a continuación.
    1. Clave: "startup-script", valor: el contenido del script mostrado más abajo.
    1. Clave: "shutdown-script", valor: el contenido del script mostrado más abajo.
1. Guarda los cambios en la instancia

Script de inicio:
```
#! /bin/bash
apt update
apt -y install apache2
cat <<EOF > /var/www/html/index.html
<html><body><p>Script de inicio añadido correctamente</p></body></html>
```

Script de apagado:
```
#! /bin/bash
echo "+++ Apagando instancia +++" >> /var/tmp/shutdown_script_output.txt
```

Para comprobar los scripts de inicio y apagado:
1. En la página del listado de instancias o de detalles de la instancia, detenla e iníciala de nuevo una vez detenida.
1. Para comprobar el script de inicio:
    1. Accedee de nuevo a través de la IP externa de la instancia su página web.
    1. Comprueba el nuevo contenido de la misma.
1. Para comprobar el script de apagado:
    1. Conéctate por SSH a la instancia a través de alguno de los métodos utilizados anteriormentee.
    1. Ejecuta el siguiente comando para comprobar el contenido del archivo creado por el script: "ls /var/tmp" y "cat /var/tmp/shutdown_script_output.txt"

*ENTREGA: Toma una captura de pantalla del nuevo contenido de la web y del resultado del anterior comando "cat".*

#### Detener y modificar la configuración de la instancia
Vamos a comprobar qué posibilidades tenemos a la hora de modificar la instancia en ejecución y para qué casos debemos antes detener la instancia.

1. Navega a la página de detalles de la instancia ("Compute Engine > Instancias de VM" > nombre de la instancia).
1. Vamos a revisar las opciones de configuración que podemos modificar de la instancia en estado de ejecución y de detención, para lo cual responderás unas preguntas a continuación.
1. Pulsa "Editar" y revisa toda la configuración que puedes modificar de la instancia sin detenerla, respondiendo a las preguntas correspondientes.
1. Detén la instancia y, una vez detenida, pulsa "Editar" para revisar la configuración que puedes modificar una vez detenida.

**Preguntas a responder:**
Para cada una responde si se puede modificar en ejecución, detenida o no es posible.

1. *PREGUNTA: ¿Es posible cambiar el tipo de máquina?*
1. *PREGUNTA: ¿Es posible cambiar la zona?*
1. *PREGUNTA: ¿Es posible añadir o modificar las etiquetas?*
1. *PREGUNTA: ¿Es posible añadir una nueva interfaz de red?*
1. *PREGUNTA: ¿Es posible modificar las etiquetas de red?*
1. *PREGUNTA: ¿Es posible modificar el disco de arranque?*
1. *PREGUNTA: ¿Es posible añadir discos adicionales?*
1. *PREGUNTA: ¿Es posible cambiar los metadatos personalizados?*
1. *PREGUNTA: ¿Es posible cambiar la cuenta de servicio asignada?*

Por último, modifica el tipo de máquina de la instancia a "e2-standard-2" e iníciala de nuevo.

### Tarea 3: Crear una instancia de VM Windows Server
Vamos a crear una instancia de VM con Windows Server y comprobar las opciones de acceso a la misma.

#### Crear una instancia de VM Windows Server
Para crear una instancia Windows Server, sigue las siguientes instrucciones:

1. En la consola, navega a "Compute Engine > Instancias de VM" y pulsa "Crear instancia"
1. Crea una instancia con la siguiente configuración:
    - Nombre: vm-windows
    - Región y zona: europe-west1-b
    - Tipo de máquina: e2-standard-2
    - Imágen de disco: Windows Server 2019 Datacenter
    - Disco de arranque: Disco persistente estándar de 50 GB
    - Deja el resto de opciones con los valores por defecto.

*ENTREGA: Toma una captura de pantalla de la ventana de detalles de la instancia de Windows Server.*

#### Explorar los métodos de conexión
Para conectarnos a una instancia de Windows Server necesitamos establecer la contraseña de Windows, tener acceso a un cliente RDP y conectarnos por RDP a la misma.

Primero, establece la contraseña de Windows para la instancia:
1. Navega hasta la página de detalles de la instancia Windows Server.
1. En "Acceso remoto", pulsa sobre "Configurar contraseña de Windows".
1. Escribe tu nombre de usuario o de un nuevo usuario (por defecto aparecerá el de tu usuario actual).
1. Pulsa "Establecer".
1. Copia la contraseña auto-generada y guárdala, ya que no podrás acceder de nuevo a ella por la consola.

Para conectarte por RDP a la instancia:
1. Si utilizas Chrome como navegador, puedes descargarte el plugin de [Chrome RDP for Google Cloud Platform](https://chrome.google.com/webstore/detail/chrome-rdp-for-google-clo/mpbbnannobiobpnfblimoapbephgifkm/). El plugin es compatible con el botón "RDP" en la página de instancias o de detalles de la instancia, similar al botón "SSH" para Linux.
1. Si no, puedes utilizar cualquier [cliente de RDP remoto](https://docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/clients/remote-desktop-clients).

En ambos casos, utiliza la IP externa de la instancia, tu nombre de usuario y contraseña para conectarte a la instancia.

*ENTREGA: Toma una captura de pantalla del escritorio remoto de Windows Server por conexión RDP.*

## Resumen de entregas
1. M2U2-1-preguntas.txt: Respuestas a todas las preguntas planteadas en el ejercicio.
1. M2U2-1-tarea_1-captura_1.jpg: Captura de pantalla de la página de detalles de la instancia.
1. M2U2-1-tarea_1-captura_2.jpg: Captura de pantalla de la terminal SSH abierta con la consola web.
1. M2U2-1-tarea_1-captura_3.jpg: Captura de pantalla de la terminal SSH abierta con Cloud SDK local o Cloud Shell.
1. M2U2-1-tarea_1-captura_4.jpg: Captura de pantalla de la web del servidor, donde se vea también la IP de la instancia en la URL del navegador.
1. M2U2-1-tarea_1-captura_5.jpg: Captura de pantalla del nuevo contenido de la web.
1. M2U2-1-tarea_1-captura_6.jpg: Captura de pantalla del resultado del anterior comando "cat".
1. M2U2-1-tarea_2-captura_1.jpg: Captura de pantalla de la ventana de detalles de la instancia de Windows Server.
1. M2U2-1-tarea_2-captura_2.jpg: Captura de pantalla del escritorio remoto de Windows Server por conexión RDP.

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. Selecciona ambas instancias creadas y elimínalas.
