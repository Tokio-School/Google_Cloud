# Reglas de cortafuegos
Unidad M2U4 - Ejercicio 2

## ¿Qué vamos a hacer?
1. Asignar reglas de cortafuegos con etiquetas de red y según cuentas de servicio.
1. Comprobar el orden de prioridad de aplicación de las reglas de cortafuegos.
1. Ver cómo debuguear problemas de conectividad debidos a reglas de cortafuegos.

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

Para responder a todas las preguntas del ejercicio de forma agrupada, puedes crear antes de empezar un archivo a entregar llamado "M2U4-2-preguntas.txt". Recuerda identificar y ordenar cada pregunta seguida de su respuesta.

Encontrarás las preguntas entre el texto en cursiva: *PREGUNTA: ¿Cómo se llama el servicio de instancias de VMs de Google Cloud?*

### Tarea 1: Creación de la topología de red
En esta primera tarea vamos a crear la topología de red e instancias de VM necesarias para trabajar con las reglas de cortafuegos en las siguientes tareas.

#### Crear la red y subredes
Vamos a crear una red VPC con 2 subredes. Creamos una red personalizada como práctica recomendable en todo proyecto y para permitirnos controlar las reglas de cortafuegos sin tener que eliminar las de la red `default`.

Crea una red VPC con las siguientes características:
- Nombre: `custom-mode-network`.
- Modo de creación de subred: Personalizado.
1. Subred 1:
    - Nombre: `custom-mode-network-subnet-euw1`.
    - Región: europe-west1.
    - Rango de direcciones IP: 10.10.0.0/20
1. Subred 2:
    - Nombre: `custom-mode-network-subnet-euw3`.
    - Región: europe-west3.
    - Rango de direcciones IP: 10.20.0.0/20

#### Crear las instancias de VM
Ahora vamos a crear las instancias de VM en dichas subredes.

Primero crea 2 cuentas de servicio personalizadas:
1. Cuenta de servicio 1: `sa-frontend`.
1. Cuenta de servicio 2: `sa-backend`.
No te preocupes por el acceso o roles asignados a las mismas.

Crea las siguientes instancias de VM:
1. Nombre: `frontend-euw1`.
    - Región: europe-west1.
    - Tipo de máquina: e2-micro.
    - Cuenta de servicio: `sa-frontend-vms`.
    - Script de inicio: `sudo apt update && sudo apt install -y apache2`
1. Nombre: `backend-euw1`.
    - Región: europe-west1.
    - Tipo de máquina: e2-micro.
    - Cuenta de servicio: `sa-backend-vms`.
    - Script de inicio: `sudo apt update && sudo apt install -y apache2`
    - Sin dirección de IP externa.
1. Nombre: `frontend-euw3`.
    - Región: europe-west3.
    - Tipo de máquina: e2-micro.
    - Cuenta de servicio: `sa-frontend-vms`.
    - Script de inicio: `sudo apt update && sudo apt install -y apache2`
1. Nombre: `backend-euw3`.
    - Región: europe-west3.
    - Tipo de máquina: e2-micro.
    - Cuenta de servicio: `sa-backend-vms`.
    - Script de inicio: `sudo apt update && sudo apt install -y apache2`
    - Sin dirección de IP externa.
1. Nombre: `internal-client`.
    - Región: europe-west1.
    - Tipo de máquina: e2-micro.
    - Cuenta de servicio: Cuenta de servicio por defecto de Compute Engine.

Una vez creadas la topología de red y las instancias de VM podemos explorar cómo aplicar las reglas de cortafuegos.

### Tarea 2: Asignación de reglas de cortafuegos
Ahora vamos a asignar distintas reglas de cortafuegos a algunas o todas las instancias y vamos a comprobar el tráfico permitido o bloqueado.

La topología final de conexiones permitidas será la siguiente:
- Las instancias de VM se han desplegado en parejas de frontend-backend en 2 regiones diferentes.
- Sólo se permite la conexión desde el exterior a instancias de frontend

#### Asignar reglas de cortafuegos a todas las instancias
Vamos a asignar unas reglas de cortafuegos mínimas a todas las instancias que permitan las conexiones SSH y ping:

1. En la consola, navega a **Red de VPC > Firewall** y comprueba que no existe ninguna regla de cortafuegos para la red `custom-mode-network`.
1. Pulsa **Crear regla de firewall**, explora todas las opciones y crea las siguientes reglas de cortafuegos:
    1. Nombre: `allow-ssh-internet-all`:
        - Red: `custom-mode-network`.
        - Prioridad: 50000.
        - Dirección del tráfico: Entrada.
        - Acción en caso de coincidencia: Permitir.
        - Destinos: Todas las instancias de la red.
        - Filtro de origen: Rangos de IPv4, rangos de IPv4 de origen: 0.0.0.0/0
        - Protocolos y puertos: TCP:22.
1. Una vez creada, navega a **Compute Engine > Instancias de VM** y comprueba que puedes abrir una sesión SSH a las instancias de VM.

#### Comprobar las reglas de cortafuegos implícitas
Vamos a comprobar las 2 reglas de cortafuegos implícitas: todo tráfico entrante ("ingress") es bloqueado, todo tráfico saliente ("egress") es permitido:

1. Conéctate por SSH a la instancia cliente de `internal-client`.
1. Comprueba que puedes hacer ping a destinos externos: `ping -c 3 www.google.com` y `ping -c 3 8.8.8.8`.
    - Los ping externos son posibles porque todo tráfico saliente es permitido.
1. Comprueba que no puedes hacer ping a las otras instancias de la misma red: `ping -c 3 NOMBRE_INSTANCIA` o `ping -c 3 IP_INTERNA_INSTANCIA`.
    - Los ping a dichas instancias no son posibles porque todo el tráfico entrante a las mismas es bloqueado.
1. Ahora conéctate por SSH a cualquier otra instancia de VM, a la que hayas comprobado que no se permitía establecer un ping.
1. Comprueba que el ping al exterior desde la misma sí sigue siendo posible: `ping -c 3 www.google.com` y `ping -c 3 8.8.8.8`.
    - De nuevo los ping externos son posibles porque todo tráfico saliente es permitido.
1. Ahora vuelve a **Red de VPC > Firewall** y crea otra regla de cortafuegos:
    - Nombre: `allow-ping-internet-all`:
    - Red: `custom-mode-network`.
    - Prioridad: 50000.
    - Dirección del tráfico: Entrada.
    - Acción en caso de coincidencia: Permitir.
    - Destinos: Todas las instancias de la red.
    - Filtro de origen: Rangos de IPv4, rangos de IPv4 de origen: 0.0.0.0/0
    - Protocolos y puertos: ICMP.
1. Vuelve a conectarte por SSH a la instancia cliente de `internal-client` y comprueba que ahora sí puedes hacer ping al resto de instancias.

De esta forma podemos comprobar la aplicación de las 2 reglas de cortafuegos implícitas a toda red, que tienen la mínima prioridad.

*ENTREGABLE:* M2U4-2-tarea_2-archivo_1-captura_1.jpg: Captura de pantalla mostrando el comando ping y su resultado desde la instancia cliente.

#### Asignar reglas de cortafuegos con etiquetas de red
Ahora vamos a asignar reglas de cortafuegos a las instancias para permitir el tráfico a sus webs, según la topología de conexiones prevista.

Primero comprueba que no es posible la conexión desde la instancia de cliente `internal-client` a las diferentes webs con `curl http://IP_INSTANCIA:80`.

Asígnale a cada instancia de VM las etiquetas de red ("network tag" o "tag") correspondientes:
1. Navega a **Compute Engine > Instancias de VM**.
1. Para modificar las etiquetas de red de una instancia de VM, pulsa sobre la instancia para acceder a su página de detalles, luego sobre **Editar** y, en **Herramientas de redes**, añade el listado de etiquetas de red separándolas con un espacio o coma.
1. Añade las siguientes etiquetas de red a cada instancia:
    - Instancia: `frontend-euw1`, etiquetas de red: `frontend`.
    - Instancia: `backend-euw1`, etiquetas de red: `backend`.
    - Instancia: `frontend-euw3`, etiquetas de red: `frontend`.
    - Instancia: `backend-euw3`, etiquetas de red: `backend`.

Ahora crea las siguientes reglas de cortafuegos asignándolas con etiquetas de red:
1. Nombre: `allow-http-internet-frontend`:
    - Red: `custom-mode-network`.
    - Prioridad: 1000.
    - Dirección del tráfico: Entrada.
    - Acción en caso de coincidencia: Permitir.
    - Destinos: Etiquetas de destino especificadas.
    - Etiquetas de destino: `frontend`.
    - Filtro de origen: Rangos de IPv4, rangos de IPv4 de origen: 0.0.0.0/0
    - Protocolos y puertos: TCP:80.
1. Nombre: `allow-http-frontend-backend`:
    - Red: `custom-mode-network`.
    - Prioridad: 1000.
    - Dirección del tráfico: Entrada.
    - Acción en caso de coincidencia: Permitir.
    - Destinos: Etiquetas de destino especificadas.
    - Etiquetas de destino: `backend`.
    - Filtro de origen: Etiquetas de origen, etiquetas de origen: `frontend`.
    - Protocolos y puertos: TCP:80.

Ahora realiza las siguientes comprobaciones de conectividad a la web HTTP con `curl http://IP_INSTANCIA:80` (*nota:* utiliza la IP externa de los `frontend` y la interna de los `backend`):
1. Comprueba que la instancia cliente `internal-client` puede conectarse a las instancias `frontend`.
1. Comprueba que la instancia cliente `internal-client` no puede conectarse a las instancias `backend`.
1. Comprueba que las instancias `frontend` pueden conectarse a las `backend`.

De esta forma sencilla podemos asignar diferentes reglas de cortafuegos utilizando una etiqueta de red coincidente entre la regla de cortafuegos y la instancia de VM.

*ENTREGABLE:* M2U4-2-tarea_2-archivo_2-captura_2.jpg: Captura de pantalla mostrando el listado de las reglas de cortafuegos.

Sin embargo, como comentábamos esta no es la práctica más recomendada, ya que cualquiera que pueda administrar una instancia de VM puede modificar sus etiquetas de red, o producirse un error al introducir las etiquetas.

Para comprobarlo:
1. Navega a una de las instancias de `backend` y edítala.
1. Introduce un error en su etiqueta de red, p. ej. cambiando `backend` por `backends`.
1. Comprueba cómo ahora los `frontend` no se pueden conectar a dicha instancia.

Por estas razones, la práctica más recomendable es asignar las reglas de cortafuegos utilizando cuentas de servicio.

*PREGUNTA: ¿Cuál sería la forma más sencilla y segura de asignar una regla de cortafuegos a una instancia para sólo permitir tráfico desde su subred?*

#### Asignar reglas de cortafuegos con cuentas de servicio
A continuación vamos a asignar las reglas de cortafuegos utilizando las cuentas de servicio de las instancias de VM.

1. Elimina todas las reglas de cortafuegos creadas para la red `custom-mode-net`.
1. Crea las siguientes reglas de cortafuegos:
1. Nombre: `allow-http-internet-frontend`:
    - Red: `custom-mode-network`.
    - Prioridad: 1000.
    - Dirección del tráfico: Entrada.
    - Acción en caso de coincidencia: Permitir.
    - Destinos: Cuenta de servicio especificada.
    - Cuenta de servicio objetivo: `sa-frontend`.
    - Filtro de origen: Rangos de IPv4, rangos de IPv4 de origen: 0.0.0.0/0
    - Protocolos y puertos: TCP:80.
1. Nombre: `allow-http-frontend-backend`:
    - Red: `custom-mode-network`.
    - Prioridad: 1000.
    - Dirección del tráfico: Entrada.
    - Acción en caso de coincidencia: Permitir.
    - Destinos: Cuenta de servicio especificada.
    - Cuenta de servicio objetivo: `sa-backend`.
    - Filtro de origen: Cuenta de servicio, cuenta de servicio fuente: `sa-frontend`.
    - Protocolos y puertos: TCP:80.

Realiza de nuevo las comprobaciones de conectividad a la web HTTP con `curl http://IP_INSTANCIA:80` (*nota:* utiliza la IP externa de los `frontend` y la interna de los `backend`):
1. Comprueba que la instancia cliente `internal-client` puede conectarse a las instancias `frontend`.
1. Comprueba que la instancia cliente `internal-client` no puede conectarse a las instancias `backend`.
1. Comprueba que las instancias `frontend` pueden conectarse a las `backend`.

*ENTREGABLE:* M2U4-2-tarea_2-archivo_3-captura_3.jpg: Captura de pantalla mostrando el listado de las reglas de cortafuegos.

Por último, intenta modificar la cuenta de servicio de una instancia de VM.

*PREGUNTA: De cara a posibles errores, ¿por qué 3 razones es más seguro asignar las reglas de cortafuegos utilizando cuentas de servicio?*
*NOTA:* Si tienes dudas, puedes consultar la [documentación](https://cloud.google.com/vpc/docs/firewalls#service-accounts-vs-tags) al respecto.

### Tarea 3: Resolución de problemas de conectividad con las reglas de cortafuegos
En esta última tarea vamos a explorar diferentes formas de resolver problemas de conectividad relacionados con las reglas de cortafuegos.

Comienza por crear una nueva regla de cortafuegos que bloquea de nuevo todo el tráfico externo a todas las instancias:
- Nombre: `block-all-all-all`:
    - Red: `custom-mode-network`.
    - Prioridad: 1.
    - Dirección del tráfico: Entrada.
    - Acción en caso de coincidencia: Bloquear.
    - Destinos: Todas las instancias de la red.
    - Filtro de origen: Rangos de IPv4, rangos de IPv4 de origen: 0.0.0.0/0
    - Protocolos y puertos: Todos.

Desde `internal-client`, comprueba que no puedes hacer ping a ninguna de las instancias.

#### Ver qué reglas se aplican a la instancia
Ahora vamos a comprobar por qué no se puede realizar dicha conectividad.

1. En **Compute Engine > Instancias de VM**, pulsa sobre una de las instancias `frontend`.
1. En su página de detalles, busca su interfaz de red `nic0` y pulsa sobre la misma.
1. En la siguiente página de detalles de conectividad, explora toda la información correspondiente.

*PREGUNTAS:*
1. *¿Qué reglas de cortafuegos se aplican a dicha instancia de VM?*
1. *En el análisis de conectividad entrante/"ingress", ¿qué puertos y protocolos están bloqueados y por qué regla de cortafuegos?*

#### Habilitar registros para las reglas de cortafuegos
Ahora vamos a habilitar los registros para las reglas de cortafuegos para comprobar si una regla está permitiendo o bloqueando un tipo de tráfico:

1. En **Red de VPC > Firewall**, pulsa sobre la regla de cortafuegos `block-all-all-all` y sobre **Editar**.
1. Pulsa en **Verificar la prioridad de otras reglas de firewall** y comprueba si tiene mayor o menor prioridad que la regla que permiten el tráfico ICMP.
1. Habilita sus registros.

Ahora vuelve a la sesión SSH de `internal-client` y comprueba varias veces que no puedes hacer ping ni acceder a la web de las instancias `frontend`.

1. Navega a **Cloud Logging**.
1. En la sección superior (con un botón azul a la derecha de **Ejecutar consulta**), escribe una búsqueda pulsando sobre los menús desplegables de **Recurso** y **Nombre del registro**:
    - Recurso: Subred > Subnetwork_ID: `custom-mode-net-subnet-euw1` > Subnetwork_name: `custom-mode-net-subnet-euw1`.
    - Nombre del registro: Compute Engine > `compute.googleapis.com/firewall`.
1. Ejecuta la consulta y revisa los logs.

De esta manera podemos comprobar cómo se aplica una regla de cortafuegos.

*ENTREGABLE:* M2U4-2-tarea_3-archivo_1-captura_1.jpg: Captura de pantalla mostrando los logs en Cloud Logging.

Por último, elimina la regla de cortafuegos `block-all-all-all`.

#### Pruebas de conectividad
Las pruebas de conectividad están disponibles en **Inteligencia de red > Pruebas de conectividad**.

Crea una prueba por cada una de las siguientes conectividades:
1. Desde `internal-client` a uno de los `frontend` para el protocolo ICMP.
1. Desde `internal-client` a uno de los `frontend` para el puerto TCP:80.
1. Desde uno de los `frontend` a uno de los `backend` para el puerto TCP:80.
1. Desde uno de los `backend` a uno de los `frontend` para el protocolo ICMP.
1. Desde uno de los `backend` a uno de los `frontend` para el puerto TCP:80.
1. Desde `internal-client` a uno de los `frontend` para el puerto TCP:22.
1. Desde `internal-client` a uno de los `backend` para el puerto TCP:22.

De esta forma podemos utilizar las pruebas de conectividad del centro de inteligencia de red para comprobar en todo momento que la conectividad se mantiene como la esperada.

*ENTREGABLE:* M2U4-2-tarea_3-archivo_2-captura_2.jpg: Captura de pantalla mostrando las pruebas de conectividad creadas.

#### Deshabilitar reglas de cortafuegos
Por último, comprueba cómo deshabilitando las reglas de cortafuegos podemos debuguear nuestra conectividad o deshabilitar la aplicación de una regla durante un momento para cualquier operación de administración, sin llegar a eliminar la regla y tener que recrearla después:

1. En la consola, navega a **Red de VPC > Firewall**.
1. Pulsa sobre la regla de cortafuegos `allow-http-frontend-backend` y luego sobre **Editar**.
1. Deshabilita la regla de cortafuegos.
1. Re-ejecuta el test de conectividad desde uno de los `frontend` a uno de los `backend` para el puerto TCP:80 y comprueba que ya no se puede establecer dicha conexión.

De estas formas podemos resolver distintos problemas de conectividad entre nuestras instancias.

## Resumen de entregas
1. M2U4-2-preguntas.txt: Respuestas a todas las preguntas planteadas en el ejercicio.
1. M2U4-2-tarea_2-archivo_1-captura_1.jpg: Captura de pantalla mostrando el comando ping y su resultado desde la instancia cliente.
1. M2U4-2-tarea_2-archivo_2-captura_2.jpg: Captura de pantalla mostrando el listado de las reglas de cortafuegos.
1. M2U4-2-tarea_2-archivo_3-captura_3.jpg: Captura de pantalla mostrando el listado de las reglas de cortafuegos.
1. M2U4-2-tarea_3-archivo_1-captura_1.jpg: Captura de pantalla mostrando los logs en Cloud Logging.
1. M2U4-2-tarea_3-archivo_2-captura_2.jpg: Captura de pantalla mostrando las pruebas de conectividad creadas.

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. Eliminar las instancias de VM creadas.
1. Eliminar las cuentas de servicio creadas.
1. Eliminar las redes, lo que también eliminará las subredes y las reglas de cortafuegos creadas.
