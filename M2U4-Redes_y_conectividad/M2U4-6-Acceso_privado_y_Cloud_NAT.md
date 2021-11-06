# Opciones de acceso privado y Cloud NAT
Unidad M2U4 - Ejercicio 6

## ¿Qué vamos a hacer?
1. Crear instancias de VM internas, sin IP externa.
1. Habilitar Private Google Access para poder acceder a servicios como Cloud Storage.
1. Desplegar Cloud NAT para poder acceder a destinos externos sin necesidad de IP externa.

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

Para responder a todas las preguntas del ejercicio de forma agrupada, puedes crear antes de empezar un archivo a entregar llamado "M2U4-6-preguntas.txt". Recuerda identificar y ordenar cada pregunta seguida de su respuesta.

Encontrarás las preguntas entre el texto en cursiva: *PREGUNTA: ¿Cómo se llama el servicio de instancias de VMs de Google Cloud?*

### Tarea 1: Crear el despliegue interno
Como habitualmente, comenzamos por crear la topología de red y una instancia de VM privada, sin IP externa:

1. Crea una red VPC con las siguientes características:
    - Nombre: `vpc-cloud`.
        - Modo de creación de subred: Personalizados.
        - Subred: `vpc-cloud-subnet`.
            - Región: europe-west1.
            - Rango de direcciones IP: 10.10.0.0/20
        - Modo de enrutamiento dinámico: global.
1. Crea la siguiente regla de cortafuegos:
    - Nombre: `allow-ssh-iap-all`.
        - Dirección de tráfico: Entrada.
        - Acción en caso de coincidencia: Permitir.
        - Destinos: Todas las instancias de la red.
        - Filtro de origen: 35.235.240.0/20
        - Prioridad: 1000.
        - Protocolos y puertos: TCP:22.
1. Por último, crea las siguientes instancias de VM:
    - Nombre: `vm-cloud`.
        - Zona: europe-west1-b.
        - Tipo de máquina: e2-micro.
        - Red: `vpc-cloud`.
        - IP externa: Sin IP externa.

Una vez creada, comprueba que puedes conectarte por SSH a la instancia utilizando Cloud Shell o una instalación de Cloud SDK local:
1. Conéctate utilizando Cloud IAP (veremos más sobre él en el siguiente ejercicio): `gcloud compute ssh vm-cloud --zone europe-west1-b --tunnel-through-iap`.
1. Si te pide confirmar para continuar, confirma con `Y`.
1. Si te pide una contraseña o "passphrase", simplemente pulsa la tecla **ENTER** para continuar.
1. Comprueba que la instancia no tiene conexión externa, al no tener IP externa: `ping -c 2 www.google.com`.

Por último, vamos a crear un bucket de Cloud Storage y subir un archivo de muestra:
1. Crea un bucket con un nombre único, como `ID_PROYECTO-m2u4-6`.
1. En Cloud Shell o una instalación local de Cloud SDK, crea un archivo y súbelo al bucket: `echo "hola mundo!" > sample.txt` y `gsutil cp gs://NOMBRE_BUCKET/sample.txt`.
1. Comprueba el archivo subido: `gsutil cat gs://NOMBRE_BUCKET/sample.txt`.

### Tarea 2: Habilitar acceso privado a servicios de Google Cloud
Ahora vamos a habilitar el acceso privado a servicios de Google Cloud o Private Google Access, para poder acceder a Cloud Storage desde la instancia privada.

Primero comprueba que por ahora la instancia no puede conectarse a Cloud Storage:
1. Conéctate a la instancia por SSH de nuevo: `gcloud compute ssh vm-cloud --zone europe-west1-b --tunnel-through-iap`.
1. Comprueba que no puedes copiar el archivo desde Cloud Storage: `gsutil cp gs://NOMBRE_BUCKET/sample.txt .`.

#### Habilitar Private Google Access
Ahora vamos a habilitar el Private Google Access para crear una ruta a través de la pasarela por defecto de internet y que la instancia privada se pueda conectar con Cloud Storage:
1. Navega a **Red de VPC > Redes de VPC**, pulsa sobre `vpc-cloud` y sobre `vpc-cloud-subnet`.
1. Pulsa sobre **Editar** y activa el acceso privado a Google.

Por último vuelve a la conexión SSH a la instancia de VM y comprueba que ahora sí puedes descargar el archivo.

De esta forma podemos permitir que las instancias internas puedan acceder a la mayoría de servicios de Google Cloud, externos a nuestra VPC.

*ENTREGABLE:* M2U4-6-tarea_2-archivo_1-captura_1.jpg: Captura de pantalla mostrando la página de detalles de la subred con PGA activado.

### Tarea 3: Habilitar Cloud NAT a destinos externos
En esta última tarea vamos a habilitar Cloud NAT como NAT administrado para poder disponer de conectividad externa hacia el exterior (sólo en dirección saliente).

Vuelve a la conexión SSH a la instancia:
1. *PREGUNTA: ¿Qué sucede si necesitas actualizar o instalar paquetes de la distribución?*
1. Puedes comprobarlo con el comando `sudo apt update` y `sudo apt install http2`.
1. *Nota:* Puede que diferentes paquetes provengan de diferentes orígenes...

#### Configurar Cloud NAT
Vamos a configurar una puerta de enlace de Cloud NAT:
1. Navega a **Servicios de red > Cloud NAT** y pulsa **Comenzar**.
1. Explora las opciones y crea una puerta de enlace NAT con las siguientes características:
    - Nombre: `nat-euw1`.
    - Red: `vpc-cloud`.
    - Región: europe-west1.
    - Cloud Router: crear router nuevo, `router-euw1`, intervalo de keepalive: 20 s.
    - Fuente (interna): Rangos primarios y secundarios para todas las subredes.
    - Direcciones IP NAT: Automática.

Ahora vuelve de nuevo a comprobar si puedes actualizar los paquetes de la instancia de VM, hacer pings al exterior, etc.

*BONUS:* Puedes comprobar desde qué IP externa sale tu conexión p.ej. con `curl ifconfig.me`.

*ENTREGABLE:* M2U4-6-tarea_3-archivo_1-captura_1.jpg: Captura de pantalla mostrando los detalles de la puerta de enlace NAT.

#### Habilitar registros para Cloud NAT
Para ver los diferentes registros o logs disponibles en las conexiones de Cloud NAT, sigue los siguientes pasos:
1. Navega a **Servicios de red > Cloud NAT**, pulsa sobre el nombre de la puerta de enlace NAT y sobre **Editar**.
1. En configuración avanzada, **Stackdriver logging**, habilita los registros de traducción y errores.

Una vez habilitados, navega a Cloud Logging en la pestaña de **Registros** de la página de detalles de la puerta de enlace NAT:
1. No verás ningún registro porque no se ha producido tráfico desde que se han activado los logs.
1. Vuelve a la instancia y genera algo de tráfico saliente hacia destinos externos, p. ej. con `curl www.google.com`, actualizando de nuevo los paquetes, comprobando la IP, etc., varias veces.
1. Vuelve de nuevo a Cloud Logging, actualiza los logs, y verás los registros de dicho tráfico.

*Nota:* Si no ves aún registros, espera un par de minutos y genera más tráfico de nuevo.

*ENTREGABLE:* M2U4-6-tarea_3-archivo_2-captura_2.jpg: Captura de pantalla mostrando los registros del tráfico a través de Cloud NAT.

De esta forma podemos habilitar una pasarela de enlace de NAT para habilitar una conexión compartida hacia el exterior a instancias que no tienen IP externa asignada directamente.

## Resumen de entregas
1. M2U4-6-preguntas.txt: Respuestas a todas las preguntas planteadas en el ejercicio.
1. M2U4-6-tarea_2-archivo_1-captura_1.jpg: Captura de pantalla mostrando la página de detalles de la subred con PGA activado.
1. M2U4-6-tarea_3-archivo_1-captura_1.jpg: Captura de pantalla mostrando los detalles de la puerta de enlace NAT.
1. M2U4-6-tarea_3-archivo_2-captura_2.jpg: Captura de pantalla mostrando los registros del tráfico a través de Cloud NAT.

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. Elimina la instancia de VM creada.
1. Elimina la pasarela de NAT y el router creados.
1. Elimina la red creada.
