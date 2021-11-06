# Cloud VPN y Cloud Router
Unidad M2U4 - Ejercicio 5

## ¿Qué vamos a hacer?
1. Crear una conexión por VPN entre 2 redes con Cloud VPN.
1. Comprobar el enrutamiento dinámico usando con Cloud Router.

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

Para responder a todas las preguntas del ejercicio de forma agrupada, puedes crear antes de empezar un archivo a entregar llamado "M2U4-5-preguntas.txt". Recuerda identificar y ordenar cada pregunta seguida de su respuesta.

Encontrarás las preguntas entre el texto en cursiva: *PREGUNTA: ¿Cómo se llama el servicio de instancias de VMs de Google Cloud?*

### Tarea 1: Desplegar la topología de red
Como habitualmente, comenzamos por crear la topología de red, que en este caso simulará un entorno en la nube y un entorno "on-premise":

1. Crea una red VPC con las siguientes características:
    - Nombre: `vpc-cloud`.
        - Modo de creación de subred: Personalizados.
        - Subred: `vpc-cloud-subnet`.
            - Región: europe-west1.
            - Rango de direcciones IP: 10.10.0.0/20
        - Modo de enrutamiento dinámico: global.
    - Nombre: `vpc-on-prem`.
        - Modo de creación de subred: Personalizados.
        - Subred: `vpc-on-prem-subnet`.
            - Región: europe-west1.
            - Rango de direcciones IP: 10.20.0.0/20
        - Modo de enrutamiento dinámico: global.
1. Crea las siguientes reglas de cortafuegos en cada una de las redes:
    - Nombre: `allow-ssh-ping-internet-all`.
        - Dirección de tráfico: Entrada.
        - Acción en caso de coincidencia: Permitir.
        - Destinos: Todas las instancias de la red.
        - Filtro de origen: 0.0.0.0/0
        - Prioridad: 1000.
        - Protocolos y puertos: TCP:22 e ICMP.
    - Nombre: `allow-all-internal-all`.
        - Dirección de tráfico: Entrada.
        - Acción en caso de coincidencia: Permitir
        - Destinos: Todas las instancias de la red.
        - Filtro de origen: 10.0.0.0/8
        - Prioridad: 1000.
        - Protocolos y puertos: Todos.
1. Por último, crea las siguientes instancias de VM:
    - Nombre: `vm-cloud`.
        - Región: europe-west1.
        - Tipo de máquina: e2-micro.
        - Red: `vpc-cloud`.
    -  Nombre: `vm-on-prem`.
        - Región: europe-west1.
        - Tipo de máquina: e2-micro.
        - Red: `vpc-on-prem`.

### Tarea 2: Conexión VPN con enrutamiento dinámico
Ahora vamos a establecer la conexión por VPN entre ambos entornos, utilizando la opción de alta disponibilidad (HA) de Cloud VPN con enrutamiento dinámico (Cloud Router):

#### Crear los túneles VPN
Comenzamos por crear las 2 pasarelas de VPN, cada una con 2 túneles (para alta disponibilidad y máximo ancho de banda). Cada pasarela representará el despliegue local en cada entorno, por lo que tendremos 2 en total, o en 2 direcciones.

1. Navega a **Conectividad híbrida > VPN** y pulsa **Crear conexión de VPN**.
1. Selecciona una **VPN con alta disponibilidad (HA)**.
1. Crea una puerta de enlace o pasarela VPN con estas características:
    - Nombre: `gateway-cloud-to-on-prem`.
    - Red: `vpc-cloud`.
    - Región: europe-west1.
1. Pulsa en **Crear y continuar** para crear la puerta de enlace, pero no continúes aún agregando túneles de VPN, ya que antes necesitamos crear la puerta de enlace correspondiente a la otra red para vincularlas, sino pulsa en **Cancelar**.
1. Crea otra puerta de enlace con estas características:
     - Nombre: `gateway-on-prem-to-cloud`.
     - Red: `vpc-on-prem`.
     - Región: europe-west1.
1. Continúa agregando túneles VPN a la pasarela `gateway-on-prem-to-cloud`:
    - Puerta de enlace VPN de par (o remota): Google Cloud.
    - Proyecto: tu ID de proyecto.
    - Nombre de puerta de enlace de VPN: `gateway-cloud-to-on-prem`.
    - Alta disponibilidad: Crear un par de túneles de VPN.
    - Opciones de enrutamiento: Crear nuevo Cloud Router:
        - Nombre: `router-on-prem`.
        - ASN de Google: 64512.
        - Intervalo keepalive de par de BGP: 20.
    - Túneles VPN:
        - Nombre: `tunnel-on-prem-to-cloud-1`.
        - Clave IKE compartida previamente: `hello-vpn-1`.
        - Nombre: `tunnel-on-prem-to-cloud-2`.
        - Clave IKE compartida previamente: `hello-vpn-2`.
1. Pulsa en **Configurar las sesiones de GCP más tarde**, para disponer primero de los 2 túneles en ambas puertas de enlace.
1. De vuelta en la página de **Conectividad híbrida > VPN**, pulsa en la pestaña **Puertas de enlace de Cloud VPN**, donde podrás comprobar que hay 2 puertas de enlace, pero sólo una cuenta ya con los 2 túneles creados.
1. Para la puerta de enlace `gateway-cloud-to-on-prem`, pulsa en **Agregar túnel VPN**.
1. Continúa agregando túneles VPN ahora a la pasarela `gateway-cloud-to-on-prem`:
    - Puerta de enlace VPN de par (o remota): Google Cloud.
    - Proyecto: tu ID de proyecto.
    - Nombre de puerta de enlace de VPN: `gateway-on-prem-to-cloud`.
    - Alta disponibilidad: Crear un par de túneles de VPN.
    - Opciones de enrutamiento: Crear nuevo Cloud Router:
        - Nombre: `router-cloud`.
        - ASN de Google: 64513.
        - Intervalo keepalive de par de BGP: 20.
    - Túneles VPN:
        - Nombre: `tunnel-cloud-to-on-prem-1`.
        - Clave IKE compartida previamente: `hello-vpn-1`.
        - Nombre: `tunnel-cloud-to-on-prem-2`.
        - Clave IKE compartida previamente: `hello-vpn-2`.
1. Pulsa en **Configurar las sesiones de GCP más tarde**.
1. En la página de **Conectividad híbrida > VPN**, en la pestaña **Túneles de Cloud VPN**, comprueba los estados de los 4 túneles de VPN, que deben mostrar "Establecido".

Ahora comienza a configurar las sesiones de BGP:
- Túnel de Cloud VPN: `tunnel-cloud-to-on-prem-1`.
    - Nombre: `session-cloud-to-on-prem-1`.
    - ASN del par: 64512.
    - Prioridad de ruta anunciada (MED): 1.
    - IP de BGP de Cloud Router: 169.254.0.1
    - IP del par de BGP: 169.254.0.2
- Túnel de Cloud VPN: `tunnel-cloud-to-on-prem-2`.
    - Nombre: `session-cloud-to-on-prem-2`.
    - ASN del par: 64512.
    - Prioridad de ruta anunciada (MED): 1.
    - IP de BGP de Cloud Router: 169.254.10.1
    - IP del par de BGP: 169.254.10.2
- Túnel de Cloud VPN: `tunnel-on-prem-to-cloud-1`.
    - Nombre: `session-on-prem-to-cloud-1`.
    - ASN del par: 64513.
    - Prioridad de ruta anunciada (MED): 1.
    - IP de BGP de Cloud Router: 169.254.0.2
    - IP del par de BGP: 169.254.0.1
- Túnel de Cloud VPN: `tunnel-on-prem-to-cloud-2`.
    - Nombre: `session-on-prem-to-cloud-2`.
    - ASN del par: 64513.
    - Prioridad de ruta anunciada (MED): 1.
    - IP de BGP de Cloud Router: 169.254.10.2
    - IP del par de BGP: 169.254.10.1
1. Una vez configuradas, en la pestaña de **Túneles de Cloud VPN** puedes verificar que todas las configuraciones son correctas (*nota:* puedes hacer scroll horizontal para consultar todas las columnas).

Una vez comprobadas las configuraciones, comprueba las rutas creadas automáticamente en **Red de VPC > Rutas**.

Por último, comprueba la conectividad entre las instancias:
1. De `vm-cloud` a `vm-on-prem`: `ping -c 3 IP_INTERNA_DESTINO`.
1. De `vm-on-prem` a `vm-cloud`: `ping -c 3 IP_INTERNA_DESTINO`.

*BONUS:* Como alternativa, puedes crear comprobaciones de conectividad en **Inteligencia de red > Pruebas de conectividad**.

De esta forma podemos establecer una conectividad privada entre 2 redes, simulando una conexión VPN entre el on-premise y la nube.

*ENTREGABLES:*
1. M2U4-5-tarea_2-archivo_1-captura_1.jpg: Captura de pantalla mostrando la pestaña de **Puertas de enlace de Cloud VPN**.
1. M2U4-5-tarea_2-archivo_2-captura_2.jpg: Captura de pantalla mostrando la pestaña de **Túneles de VPN**.

#### Comprobar que el enrutamiento es dinámico
Por último, vamos a comprobar que el enrutamiento es dinámico: cuando creamos una nueva ruta (p. ej. al crear una subred) en un entorno, automáticamente por BGP se publica y actualiza en el otro entorno:

1. Crea una nueva subred en la VPC cloud:
    - Nombre: `vpc-cloud-subnet-euw3`.
    - Región: europe-west3.
    - Rango de direcciones IP: 10.128.0.0/20
1. Crea una nueva instancia de VM en dicha subred:
    - Nombre: `vm-cloud-2`.
    - Región: europe-west3.
    - Tipo de máquina: e2-micro.
    - Red: `vpc-cloud`.
1. Comprueba de nuevo las rutas: *PREGUNTA: ¿Se ha publicado la nueva ruta a la nube subred en `vpc-cloud` a la red `vpc-on-prem`?*
1. Ahora comprueba de nuevo la conectividad entre las instancias:
    1. De `vm-cloud-2` a `vm-on-prem`: `ping -c 3 IP_INTERNA_DESTINO`.
    1. De `vm-on-prem` a `vm-cloud-2`: `ping -c 3 IP_INTERNA_DESTINO`.

Para finalizar, borra los 2 túneles de una pasarela, p. ej. `tunnel-on-prem-to-cloud-1` y `tunnel-on-prem-to-cloud-2` para simular una pérdida de conexión.
1. Vuelve a comprobar las rutas: *PREGUNTA: ¿Siguen manteniéndose las rutas creadas?*
1. Vuelve a comprobar la conectividad entre las instancias: *PREGUNTA: ¿Sigue manteniéndose la conectividad?*

De esta forma hemos podido comprobar que el enrutamiento dinámico con Cloud Router automáticamente publica las rutas entre entornos.

## Resumen de entregas
1. M2U4-5-preguntas.txt: Respuestas a todas las preguntas planteadas en el ejercicio.
1. M2U4-5-tarea_2-archivo_1-captura_1.jpg: Captura de pantalla mostrando la pestaña de **Puertas de enlace de Cloud VPN**.
1. M2U4-5-tarea_2-archivo_2-captura_2.jpg: Captura de pantalla mostrando la pestaña de **Túneles de VPN**.

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. Elimina las instancias de VM creadas.
1. Elimina los túneles de Cloud VPN, las puertas de enlace y los routers creados.
1. Elimina las redes de VPC creadas.
