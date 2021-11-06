# Redes y subredes
Unidad M2U4 - Ejercicio 1

## ¿Qué vamos a hacer?
1. Explorar las posibilidades de creación de redes y subredes.
1. Comprobar la conectividad entre regiones.
1. Comprobar la conectividad entre recursos en la misma red y en distintas redes.
1. Modificar los rangos de direccionamiento y crear rangos de IP alias.
1. Analizar los logs de tráfico de subred.

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

Para responder a todas las preguntas del ejercicio de forma agrupada, puedes crear antes de empezar un archivo a entregar llamado "M2U4-1-preguntas.txt". Recuerda identificar y ordenar cada pregunta seguida de su respuesta.

Encontrarás las preguntas entre el texto en cursiva: *PREGUNTA: ¿Cómo se llama el servicio de instancias de VMs de Google Cloud?*

### Tarea 1: Creación de redes y subredes
En esta primera tarea vamos a explorar la creación de redes y subredes, y los diferentes modos de redes disponibles.

#### Analizar la red "default"
Vamos a comenzar analizando la red default:
1. En la consola, navega a **Red de VPC > Redes de VPC**.
1. Localiza la red `default`, que se crea por defecto al crear cada nuevo proyecto.
1. *PREGUNTAS:*
    1. *¿Cuántas subredes tiene?*
    1. *¿Falta alguna región disponible, o se repite alguna?*
    1. *¿Cuál es la localización de la red?*
    1. *¿Cuál es el rango de direcciones IP de la red?*
    1. *¿Cuál es la extensión de los rangos de direcciones IP de las subredes?*
    1. *¿Hay algún solapamiento de direcciones de IP entre subredes?*
    1. *¿Cuál es el "modo" de la red?*
1. Navega a **Red de VPC > Firewall**.
    1. *PREGUNTA: Analizando las reglas de firewall aplicadas, ¿te parece una red segura que se pueda utilizar en producción?*

#### Crear una red de modo automático
Ahora vamos a crear una red de modo automático:
1. Navega a **Red de VPC > Redes de VCP** y pulsa sobre **Crear red de VPC**.
1. Usa `auto-mode-net` como nombre de la red VPC.
1. Selecciona `Automático` como **Modo de creación de subred** y fíjate en las regiones y rangos de IP utilizados. Lo llamamos modo `Automático` porque automáticamente se crea una subred para cada región.
1. En **Reglas de firewall**, analiza las reglas disponibles para importar, que serán iguales a varias de las disponibles en la red `default`:
    1. Fíjate en las 2 últimas. Son las 2 reglas de firewall implícitas aplicadas a toda red con mínima prioridad.
    1. Seleccionar para importar todas las reglas.
1. Como **Modo de enrutamiento dinámico**, fíjate en la descripción y escoge `Global`.
1. Crea la red VPC.

La creación de la red VPC tardará unos 2 o 3 minutos.

De esta forma rápida y sencilla podemos crear una nueva red VPC con subredes ya disponibles automáticamente en varias regiones.

*Nota:* A veces se muestran de forma incorrecta unas 15 subredes antes de crear la red, aunque finalmente se crean una por región.

*PREGUNTAS:*
- *¿Se podrían emparejar 2 redes de modo automático entre si, como las `default` y `auto-mode-net`?*
- *¿Y qué quiere decir la abreviatura VPC?*

#### Crear una red de modo personalizado
En esta ocasión vamos a crear una red de modo personalizado:
1. Pulsa de nuevo sobre **Crear red de VPC**.
1. Crea una red VPC con las siguientes características:
    - Nombre: `custom-mode-net`.
    - Modo de creación de subred: Personalizados.
        - Subred 1: `custom-mode-net-subnet-euw1`, región: europe-west1, rango de direcciones IP: 10.10.0.0/20.
        - Subred 2: `custom-mode-net-subnet-euw2`, región: europe-west2, rango de direcciones IP: 10.20.0.0/20.
    - Modo de enrutamiento dinámico: Global.

Una vez creada la VPC, vamos a añadir una subred extra:
1. Pulsa sobre el nombre de la VPC, `custom-mode-net`.
1. Explora la información y pestañas, y finalmente pulsa `Agregar subred`.
1. Agrega una nueva subred:
    - Nombre: `custom-mode-net-subnet-euw3`, región: europe-west3, rango de direcciones IP: 10.30.0.0/20.

De esta forma podemos crear una red en modo personalizado, controlando todas sus características, y se añaden nuevas subredes a posteriori.

*ENTREGABLE*: M2U4-1-tarea_1-archivo_1-captura_1.jpg: Captura de pantalla mostrando la página de detalles de la red `custom-mode-net`.

#### Explorar las rutas
Una vez creadas las redes, navega a **Red de VCP > Rutas** y explora las rutas para cada una de las redes.

*PREGUNTA: ¿Debemos administrar redes de forma manual, o se crean de forma automática para cada subred, también para la última subred añadida?*

### Tarea 2: Comprobación de conectividad
En esta tarea vamos a comprobar la conectividad entre instancias, en función de su localización y red seleccionada.

#### Crear de las instancias de VM
Vamos a comenzar por crear las instancias.
Crea 3 instancias con las siguientes características:
1. Nombre: `auto-mode-net-vm1`.
    - Región: europe-west1.
    - Serie y tipo de máquina: e2-micro.
    - Red VPC: `auto-mode-net`.
1. Nombre: `auto-mode-net-vm2`.
    - Región: europe-west3.
    - Serie y tipo de máquina: e2-micro.
    - Red VPC: `auto-mode-net`.
1. Nombre: `custom-mode-net-vm1`.
    - Región: europe-west1.
    - Serie y tipo de máquina: e2-micro.
    - Red VPC: `custom-mode-net`.

*Nota:* Encontrarás la configuración de redes desplegando el menú de **Herramientas de redes, discos, seguridad, administración, usuario único**, en **Herramientas de redes > Interfaces de red**, modificando la interfaz `default`.

#### Comprobar la conectividad
Por último, vamos a comprobar la conectividad entre las instancias.

Primero vamos a crear una regla de cortafuegos que habilite el tráfico SSH e ICMP entrante en la red `custom-mode-net`.

Para ello, en Cloud Shell o tu instalación de Cloud SDK local ejecuta el comando `gcloud compute firewall-rules create allow-ingress-ssh-icmp --direction ingress --action allow --rules tcp:22,icmp --network custom-mode-net`

Para comprobar la conectividad:
1. Navega a **Compute Engine > Instancias de VM**.
1. Pulsa sobre el botón **SSH** de la instancia origen.
1. Comprueba la conexión a la instancia destino: `ping IP_INSTANCIA_DESTINO`.
1. Cancela el comando con `CTRL + C`.

Ahora responde a las siguientes preguntas comprobando la conectividad indicada. *PREGUNTAS:*
1. *Entre las IPs internas de `auto-mode-net-vm1` y `auto-mode-net-vm2`, aunque estén en regiones diferentes.*
1. *Entre las IPs internas de `auto-mode-net-vm2` y `auto-mode-net-vm3`, aunque estén en regiones diferentes.*
1. *Entre las IPs externas de `auto-mode-net-vm1` y `auto-mode-net-vm2`.*
1. *Entre las IPs internas de `auto-mode-net-vm1`y `custom-mode-net-vm1`, estando en la misma región pero en redes diferentes.*
1. *Entre las IPs externas de `auto-mode-net-vm1`y `custom-mode-net-vm1`, estando en redes diferentes.*

De esta forma podemos comprobar que hay conectividad privada entre instancias en la misma red, independientemente de la región y subred donde se ubiquen, pero no si están en redes diferentes, y que siempre que las reglas de firewall lo permitan, habrá conectividad sólamente pública entre instancias en redes diferentes.

*Nota:* Conectividad privada se refiere a utilizar IPs privadas (genrealmente internas), y pública a utilizar IPs públicas (generalmente externas).

#### Tests de conectividad
Ahora vamos a utilizar la herramienta de test de conectividad automáticos.

En la consola, navega a **Inteligencia de red > Pruebas de conectividad** y utiliza la herramienta para replicar los test anteriores.

*Nota:*
- Te pedirá habilitar la API de Network Management.
- Para las IPs internas, puedes seleccionar la instancia, pero para las externas, introduce la IP externa como fuente o destino.
- Utiliza el protocolo ICMP.

*PREGUNTA: ¿Qué resultado arrojan las 5 pruebas?*.

### Tarea 3: Direccionamiento
En esta tarea vamos a explorar el direccionamiento de IPs internas, cómo ampliar los rangos y utilizar IPs de alias.

#### Ampliar rangos de direcciones IP
Vamos ver cómo a ampliar el rango de direcciones de las subredes. Recordamos que las redes de modo auto tienen subredes con rango `/20` y se pueden ampliar hasta `/16`, mientras que las de tipo personalizado no tienen limitaciones al respecto.

Amplía el rango de direcciones de una subred:
1. En la consola, navega a **Red de VPC > Redes de VPC** y pulsa sobre la red de modo auto `auto-mode-net`.
1. En el listado de subredes, comprueba que sus rangos de direcciones son `/20`.
1. Pulsa sobre una subred cualquiera y luego sobre **Editar**.
1. Cambia su rango a uno más restringido, p. ej. `/22`. *PREGUNTA: ¿Ha habido algún problema? ¿Cuál?*
1. Intenta ahora cambiar el rango a uno más amplio, p. ej. `/18`.
1. Una vez cambiado, intenta cambiarlo a un `/14`. *PREGUNTA: ¿Ha habido algún problema ahora? ¿Cuál?*
1. Por último, cambia de nuevo el rango a `/16`.

De esta forma podemos ampliar el rango de una subred en cualquier momento.

Ahora repite la operación para una de las subredes de la red `custom-mode-net`, la subred `custom-mode-net-subnet-euw1`, ampliándola hasta `/16`.

*PREGUNTA BONUS: Utilizando cualquier calculadora de subredes IP online y teniendo en cuenta los rangos de direcciones IP utilizados, ¿hasta qué rango podríamos ampliarlo teóricamente?*

#### Utilizar IP de alias
Por último vamos a crear un rango de IP secundario en una subred y utilizarlo para asignar un rango de IP de alias a una instancia de VM, para poder desplegar varios servicios en la misma.

Vamos a comenzar por añadir un rango de IP secundario a una subred:
1. En la consola, navega a **Red de VPC > Redes de VCP** y pulsa sobre la red de modo auto `custom-mode-net`.
1. En el listado de subredes, comprueba sus rangos de direcciones internos.
1. Pulsa sobre la subred `custom-mode-net-subnet-euw2` y sobre **Editar**.
1. Explora las opciones y añade un rango de IP secundarias:
    - Nombre: `alias-ip-range-euw2`.
    - Rango: `10.40.0.0/26`.
1. En la consola, navega a la página de la red o del listado de todas las redes, y comprueba el rango secundario añadido.

*ENTREGABLE*: M2U4-1-tarea_3-archivo_1-captura_1.jpg: Captura de pantalla mostrando las subredes y sus rangos de la red `custom-mode-net`.

Ahora vamos a asignar un rango de IP de alias a una instancia de VM.

1. En **Compute Engine**, crea una instancia de VM:
    - Nombre: `custom-mode-net-vm2`.
    - Región: europe-west2.
    - Serie y tipo de máquina: e2-micro.
    - Red: `custom-mode-net`, subred: `custom-mode-net-subnet-euw2`.
    - Rangos de alias de IP: rango de subred `alias-ip-range-euw2`, rango de alias de IP `/29`.
1. Una vez creada la instancia, pulsa sobre la misma y, en la página de detalles, revisa su información de **Interfaces de red** y su **rango de alias de IP**.

Para comprobar la conectividad a dicha instancia:
1. Conéctate por SSH a la instancia `custom-mode-net-vm1`.
1. Haz un ping a la IP interna (principal) de la instancia `custom-mode-net-vm2`: `ping -c 3 IP_INTERNA_VM1`.
1. Comprueba también la conectividad a las IPs del rango de alias: `ping -c 3 10.40.0.0` hasta `ping -c 3 10.40.0.7`.

De esta forma podemos asignar un rango de IP de alias a una instancia de VM desde un rango secundario de una subnet.

*ENTREGABLE:* M2U4-1-tarea_3-archivo_2-captura_2.jpg: Captura de pantalla mostrando el resultado de los últimos comandos de ping.

### Tarea 4: Registros de tráfico
En esta última tarea vamos a habilitar los registros de tráfico en una subred, logueando un muestreo del tráfico en cualquier dirección a través de la misma.

Para ello, navega a **Redes de VPC > Red de VPC** y pulsa sobre la subred `custom-mode-net-subnet-euw1`.
1. Activa los **Registros de flujo**.
1. Una vez activados, vuelve a la sesión SSH o establece una nueva.
1. Haz ping a la otra instancia de la misma red `custom-mode-net`, a `www.google.com`, a otros destinos, etc., para generar algo de tráfico en la red.
1. Por último, navega a **Cloud Logging**.
1. En la sección superior (con un botón azul a la derecha de **Ejecutar consulta**), escribe una búsqueda pulsando sobre los menús desplegables de **Recurso** y **Nombre del registro**:
    - Recurso: Subred > Subnetwork_ID: `custom-mode-net-subnet-euw1` > Subnetwork_name: `custom-mode-net-subnet-euw1`.
    - Nombre del registro: Compute Engine > `vpc_flows`.
1. Ejecuta la consulta y revisa los logs.

Si no ves ningún log, amplía el plazo de consulta o vuelve a la sesión SSH y genera más tráfico desde o hacia las instancias de dicha subred.

De esta forma podemos habilitar y consultar los registros de muestreo de tráfico en una subred.

*ENTREGABLE:* M2U4-1-tarea_4-archivo_1-captura_1.jpg: Captura de pantalla mostrando los logs de tráfico de la subred.

## Resumen de entregas
1. M2U4-1-preguntas.txt: Respuestas a todas las preguntas planteadas en el ejercicio.
1. M2U4-1-tarea_1-archivo_1-captura_1.jpg: Captura de pantalla mostrando la página de detalles de la red `custom-mode-net`.
1. M2U4-1-tarea_3-archivo_1-captura_1.jpg: Captura de pantalla mostrando las subredes y sus rangos de la red `custom-mode-net`.
1. M2U4-1-tarea_3-archivo_2-captura_2.jpg: Captura de pantalla mostrando el resultado de los últimos comandos de ping.
1. M2U4-1-tarea_4-archivo_1-captura_1.jpg: Captura de pantalla mostrando los logs de tráfico de la subred.

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. Elimina las instancias de VM creadas.
1. Elimina los tests de conectividad creados.
1. Elimina las redes VPC creadas.

*NOTA:* A veces los recursos pueden tardar en borrarse o indicar que necesitan antes borrar otros de los que dependen.
