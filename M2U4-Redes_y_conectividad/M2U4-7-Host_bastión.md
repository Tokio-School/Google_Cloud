# Hosts bastión y Cloud IAP
Unidad M2U4 - Ejercico 7

## ¿Qué vamos a hacer?
1. Crear una instancia interna y un host bastión para habilitar la conectividad desde y hasta la misma.
1. Establecer Cloud IAP como alternativa al host bastión.

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

Para responder a todas las preguntas del ejercicio de forma agrupada, puedes crear antes de empezar un archivo a entregar llamado "M2U4-7-preguntas.txt". Recuerda identificar y ordenar cada pregunta seguida de su respuesta.

Encontrarás las preguntas entre el texto en cursiva: *PREGUNTA: ¿Cómo se llama el servicio de instancias de VMs de Google Cloud?*

### Tarea 1: Crear el despliegue interno con host bastión
De nuevo comenzamos por crear la topología de red, una instancia de VM privada, sin IP externa, y una instancia de host bastión.

#### Crear entorno
Para ello, vamos a seguir los siguientes pasos:
1. Crear una instancia interna, sin IP interna.
1. Asignarle unas rutas y reglas de cortafuegos a la instancia interna, tal que:
    - Se permita la conexión desde la instancia bastión y, opcionalmente, otros orígenes internos.
    - La ruta haga que cualquier conexión hacia el exterior vaya siempre a través del host bastión.
1. Asignar una regla de cortafuegos al host bastión para limitar los orígenes de conexiones SSH.
1. En la instancia bastión configuramos:
    - Reenvío de direcciones IP.
    - Gestión de claves SSH con Cloud SDK y OS Login.
    - Permiso para que la cuenta de servicio del host bastión se conecte por SSH a la instancia interna (necesario para el OS Login).

1. Comienza por crear una red VPC con las siguientes características:
    - Nombre: `vpc-cloud`.
    - Modo de creación de subred: Personalizados.
    - Subred: `vpc-cloud-subnet`.
        - Región: europe-west1.
        - Rango de direcciones IP: 10.10.0.0/20
1. Ahora crea la instancia de VM interna:
    - Nombre: `vm-internal`.
    - Zona: europe-west1-b.
    - Tipo de máquina: e2-micro.
    - Metadatos: clave `enable-oslogin`, valor `TRUE`.
    - Red: `vpc-cloud`.
    - Etiqueta de red: `internal-vm`.
    - IP externa: Sin IP externa.

#### Desplegar un host bastión
Ahora vamos a crear la instancia de host bastión:
1. Crea una cuenta de servicio:
    - Nombre: `sa-bastion-host`.
    - Acceso al proyecto/funciones (roles): Compute Engine > Acceso a SO de Compute.
    - Acceso a esta cuenta de servicio: Función de los usuarios de cuentas de servicio > tu cuenta de Google.
    - *Nota:* Verás estos accesos como el segundo y tercer paso, respectivamente, en el asistente de creación de la cuenta de servicio.
1. Ahora crea la instancia de host bastión:
    - Nombre: `bastion-host`.
    - Zona: europe-west1-b.
    - Tipo de máquina: e2-small.
    - Cuenta de servicio: `sa-bastion-host`.
    - Red: `vpc-cloud`.
    - Etiquetas de red: `bastion-host`.
    - Herramientas de redes > Reenvío de IP: Habilitar.

#### Configurar el despliegue
Una vez creados los recursos, vamos a configurar las rutas y reglas de cortafuegos:

1. Crea las siguientes reglas de cortafuegos:
    - Nombre: `allow-all-bastion-internal`.
        - Red: `vpc-cloud`.
        - Dirección de tráfico: Entrada.
        - Acción en caso de coincidencia: Permitir.
        - Destinos: etiquetas de red, `internal-vm`.
        - Filtro de origen: etiquetas de origen, `bastion-host`
        - Prioridad: 100.
        - Protocolos y puertos: Todos.
    - Nombre: `allow-all-internet-bastion`.
        - Red: `vpc-cloud`.
        - Dirección de tráfico: Entrada.
        - Acción en caso de coincidencia: Permitir.
        - Destinos: cuentas de servicio, `sa-bastion-host`.
        - Filtro de origen: 0.0.0.0/0 (*nota:* para ser más específico, podríamos sustituirlo por la IP de Cloud Shell o un rango de IP públicas de la empresa).
        - Prioridad: 1000.
        - Protocolos y puertos: Todos.
1. Crea la siguiente ruta estática personalizada:
    - Nombre: `route-all-bastion-internal`.
    - Red: `vpc-cloud`.
    - Rango de IP de destino: 0.0.0.0/0
    - Prioridad: 100.
    - Etiquetas de instancia: `internal-vm`.
    - Siguiente salto: especificar una instancia, `bastion-host`.

De esta forma hemos podido habilitar el tráfico entrante al host bastión desde el exterior y a la instancia interna desde el host bastión y especificar que la conexión hacia el exterior de las instancias internas pasen por el host bastión.

#### Comprobar el despliegue
Por último, vamos a comprobar el despliegue desde Cloud Shell o una instalación local de Cloud SDK:
1. Conéctate a la instancia de host bastión: `gcloud compute ssh bastion-host`.
1. Comprueba que puedes conectarte a la instancia interna, desde el host bastión: `gcloud compute ssh vm internal --internal-ip`.
1. Comprueba tu conectividad al exterior: `ping -c 2 www.google.com` y `curl www.google.com`.
1. Averigua la IP de salida de la instancia interna: `curl ipconfig.me`.

*PREGUNTA: ¿Corresponde dicha IP a la IP externa del host bastión?*

*ENTREGABLE:* M2U4-7-tarea_1-archivo_1-captura_1.jpg: Captura de pantalla mostrando la conexión SSH a la instancia interna y el resultado de los comandos `ping` o `curl`.

### Tarea 2: Habilitar Cloud IAP
Para esta tarea vamos a sustituir los host bastión por el uso de Cloud IAP, que nos permitirá no tener que depender de dichos bastiones, sus inconveniencias y necesidades de mantenimiento.

Cloud IAP actuará como proxy que permite controlar el acceso a una instancia de VM, creando túneles a sus puertos TCP, permitiendo conexiones HTTP, SSH, etc.

#### Habilitar Cloud IAP
Comenzamos por crear una regla de cortafuegos para permitir la conexión desde el proxy a las instancias internas:
- Nombre: `allow-ssh-iap-all`.
- Dirección de tráfico: Entrada.
- Acción en caso de coincidencia: Permitir.
- Destinos: Todas las instancias de la red.
- Filtro de origen: 35.235.240.0/20
- Prioridad: 100.
- Protocolos y puertos: TCP:22.

Como tenemos acceso como propietarios de proyecto, tenemos ya asignados los permisos correspondientes para poder utilizar Cloud IAP para acceder por SSH:
1. Comprueba que puedes conectarte desde Cloud Shell o una instalación local de Cloud SDK: `gcloud compute ssh vm internal --internal-ip`.
1. Navega a **Compute Engine > Instancias de VM** y comprueba que puedes utilizar el botón SSH para crear una conexión SSH a la instancia.

De esta forma tan sencilla podemos aprovechar la integración con Cloud IAP para conectarnos a una instancia de VM interna sin necesidad de un host bastión.

*ENTREGABLES:* M2U4-7-tarea_2-archivo_1-captura_1.jpg: Captura de pantalla de la página de listado de instancias mostrando que la instancia no tiene IP externa y el botón SSH activo.

## Resumen de entregas
1. M2U4-7-preguntas.txt: Respuestas a todas las preguntas planteadas en el ejercicio.
1. M2U4-7-tarea_1-archivo_1-captura_1.jpg: Captura de pantalla mostrando la conexión SSH a la instancia interna y el resultado de los comandos `ping` o `curl`.
1. M2U4-7-tarea_2-archivo_1-captura_1.jpg: Captura de pantalla de la página de listado de instancias mostrando que la instancia no tiene IP externa y el botón SSH activo.

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. Elimina las instancias creadas.
1. Elimina la cuenta de servicio creada.
1. Elimina la red VCP creada, lo que eliminará el resto de recursos de red.
