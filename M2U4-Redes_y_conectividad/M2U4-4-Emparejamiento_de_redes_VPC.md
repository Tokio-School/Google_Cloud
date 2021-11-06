# Emparejamiento de redes VPC
Unidad M2U4 - Ejercicio 4

## ¿Qué vamos a hacer?
1. Establecer conectividad interna entre redes con emparejamiento de redes VPC.
1. Comprobar la descentralización de la gestión de redes a través de reglas de cortafuegos.
1. Comprobar cómo el emparejamiento entre redes no es transitivo.

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

Para responder a todas las preguntas del ejercicio de forma agrupada, puedes crear antes de empezar un archivo a entregar llamado "M2U4-4-preguntas.txt". Recuerda identificar y ordenar cada pregunta seguida de su respuesta.

Encontrarás las preguntas entre el texto en cursiva: *PREGUNTA: ¿Cómo se llama el servicio de instancias de VMs de Google Cloud?*

### Tarea 1: Desplegar la topología de red
Una vez más vamos a comenzar desplegando la topología de red y las instancias de VM a utilizar durante el ejercicio.

#### Crear las redes y subredes
Crea las siguientes redes VPC:
1. Crea una red VPC con las siguientes características:
    - Nombre: `vpc-a`.
        - Modo de creación de subred: Personalizados.
        - Subred: `vpc-a-subnet`.
            - Región: europe-west1.
            - Rango de direcciones IP: 10.10.0.0/20.
    - Nombre: `vpc-b`.
        - Modo de creación de subred: Personalizados.
        - Subred: `vpc-b-subnet`.
            - Región: europe-west1.
            - Rango de direcciones IP: 10.20.0.0/20.
    - Nombre: `vpc-c`.
        - Modo de creación de subred: Personalizados.
        - Subred: `vpc-c-subnet`.
            - Región: europe-west1.
            - Rango de direcciones IP: 10.30.0.0/20.
1. Crea las siguientes reglas de cortafuegos en cada una de las redes:
    - Nombre: `allow-ssh-ping-internet-all`.
        - Dirección de tráfico: Entrada.
        - Acción en caso de coincidencia: Permitir.
        - Destinos: Todas las instancias de la red.
        - Prioridad: 1000.
        - Filtro de origen: 0.0.0.0/0
        - Protocolos y puertos: TCP:22 e ICMP
    - Nombre: `allow-all-internal-all`.
        - Dirección de tráfico: Entrada.
        - Acción en caso de coincidencia: Permitir
        - Destinos: Todas las instancias de la red.
        - Filtro de origen: 10.0.0.0/8
        - Prioridad: 1000.
        - Protocolos y puertos: Todos

#### Crear las instancias de VM
Por último, crea las siguientes instancias de VM:
1. Nombre: `vm-a1` y `vm-a2`.
    - Región: europe-west1.
    - Tipo de máquina: e2-micro.
    - Red: `vpc-a`.
1. Nombre: `vm-b1` y `vm-b2`.
    - Región: europe-west1.
    - Tipo de máquina: e2-micro.
    - Red: `vpc-b`.
1. Nombre: `vm-c1` y `vm-c2`.
    - Región: europe-west1.
    - Tipo de máquina: e2-micro.
    - Red: `vpc-c`.

### Tarea 2: Emparejar las redes VPC
En esta tarea vamos a comenzar con los emparejamientos o "peering" de redes VPC y comprobar que permiten la conectividad entre redes.

#### Emparejar VPC A y B
Primero vamos a comenzar por comprobar que no hay conectividad posible entre instancias:
1. Conéctate por SSH a cada una de las instancias `vm-a1`, `vm-b1` y `vm-c1`.
1. Comprueba la conectividad a la IP interna de las otras 2 instancias, p. ej. `vm-a1` a `vm-b1` y `vm-c1` con su IP interna: `ping -c 3 IP_INTERNA_DESTINO`.
1. Como verás, no hay conectividad posible entre dichas instancias al estar en VPCs diferentes.

Ahora vamos a emparejar las VPCs A y B:
1. Navega a **Red de VPC > Intercambio de tráfico entre redes de VPC** y pulsa sobre **Crear conexión de intercambio de tráfico**.
1. Explora las opciones disponibles y crea un emparejamiento con las siguientes características:
    - Nombre: `peering-vpc-a-to-b`.
    - Tu red de VPC: `vpc-a`.
    - Red de VPC con intercambio de tráfico: En el mismo proyecto.
    - Nombre de la red de VPC: `vpc-b`.
1. Repite la operación para crear un emparejamiento en sentido inverso, `peering-vpc-b-to-a`.

*PREGUNTA: Nos advierte que no se puede crear un emparejamiento de redes si hay solapamiento de direcciones IP, ¿lo hay entre las 3 VPCs?*

Una vez creado, navega a **Red de VPC > Rutas** y comprueba que se han creado rutas en cada red para conectarse a la red destino.

Para terminar, comprueba de nuevo la conectividad en ambos sentidos, esta vez entre las instancias `vm-a1` y `vm-b1`.

*ENTREGABLE:* M2U4-4-tarea_2-archivo_1-captura_1.jpg: Captura de pantalla comprobando la conectividad entre las 2 instancias de VM en uno de los sentidos.

De esta forma tan sencilla podemos establecer un emparejamiento de redes VPC entre 2 redes, en el mismo proyecto o en proyectos diferentes.

#### Restringir tráfico
El emparejamiento de redes VPC nos permite mantener una administración de red distribuida e independiente, por lo que siempre podremos controlar la conectividad a nivel de cada red con reglas de cortafuegos.

Una vez emparejadas las VPC A y B, las instancias `vm-a1` y `vm-a2` podían conectarse a las instancias `vm-b1` y `vm-b2`, y viceversa. Vamos a crear una regla de cortafuegos para que sólo podamos conectarnos desde la otra VPC a las instancias `vm-a1` y `vm-b1`.

1. Edita las instancias `vm-a2` y `vm-b2` añadiéndoles la etiqueta de red `no-peering`.
1. Crea una regla de cortafuegos con la siguientes características:
    - Nombre: `block-all-peering-all`.
        - Red: `vpc-a`.
        - Dirección de tráfico: Entrada.
        - Acción en caso de coincidencia: Permitir.
        - Destinos: Etiquetas de destino específicas, `no-peering`.
        - Filtro de origen: rangos de IPv4, 10.20.0.0/20
        - Prioridad: 100.
        - Protocolos y puertos: Todos
    - Nombre: `block-all-peering-all`.
        - Red: `vpc-b`.
        - Dirección de tráfico: Entrada.
        - Acción en caso de coincidencia: Permitir.
        - Destinos: Etiquetas de destino específicas, `no-peering`.
        - Filtro de origen: rangos de IPv4, 10.10.0.0/20
        - Prioridad: 100.
        - Protocolos y puertos: Todos

Haz las siguientes comprobaciones de conectividad de nuevo:
1. Hay conectividad entre `vm-a1` y `vm-a2`.
1. Hay conectividad entre `vm-b1` y `vm-b2`.
1. No hay conectividad entre `vm-a1` y `vm-b2`.
1. No hay conectividad entre `vm-b1` y `vm-a2`.
1. Hay conectividad entre `vm-a2` y `vm-b1`.

*ENTREGABLE:* M2U4-4-tarea_2-archivo_2-captura_2.jpg: Captura de pantalla comprobando la conectividad entre las instancias de VM `vm-a1` y `vm-b2`.

De esta forma podemos mantener el control de la conectividad de forma distribuida o independiente en cada una de las redes.

### Tarea 3: Comprobar que el emparejamiento no es transitivo
En esta última tarea vamos a realizar otro emparejamiento entre las redes VPC B y C para demostrar que el peering no es transitivo, que dicho emparejamiento no permite que haya conexión entre la VPC A y C.

#### Emparejar VPC B y C
1. Sigue las instrucciones anteriores para crear un emparejamiento entre las redes `vpc-b` y `vpc-c` en ambos sentidos.
1. Una vez creado, comprueba que existen rutas administradas automáticamente para habilitar dicha conexión.
1. Comprueba la conectividad entre las instancias `vm-b1` y `vm-c1`, que ahora sí podrán conectarse entre sí.
1. Comprueba la conectividad entre las instancias `vm-a1` y `vm-c1`, que no podrán conectarse.

*PREGUNTA: Vuelve al listado de rutas, ¿a qué VPCs hay rutas hay disponibles en la VPC B? ¿Y en la A, hay rutas hacia la VPC C y viceversa?*

#### Emparejar VPC A y C
Sin embargo, siempre podemos solventar esta conectividad creando un emparejamiento directo entre las VPCs A y C en ambos sentidos.

1. De nuevo, crea otro par de emparejamientos entre las redes `vpc-a` y `vpc-c` en ambos sentidos.
1. Una vez creado, comprueba que ahora sí existen rutas entre ambas redes VPC.
1. Comprueba la conectividad entre las instancias `vm-a1` y `vm-c1`, que ahora sí podrán conectarse.

*ENTREGABLE:* M2U4-4-tarea_3-archivo_1-captura_1.jpg: Captura de pantalla comprobando la conectividad entre las instancias de VM `vm-a1` y `vm-c1`.

De esta forma podemos comprobar que el peering no es transitivo, que sólo existe conectividad si hay un emparejamiento directo entre redes VPC ya que estas rutas no se transfieren a través de otros emparejamientos.

#### Detener el emparejamiento
Del mismo modo que podemos administrar nuestras redes de forma distribuida e individual, podemos detener el emparejamiento si lo borramos:
1. Navega a **Red de VPC > Intercambio de tráfico entre redes de VPC**.
1. Elimina el emparejamiento de la red A a la red C, manteniendo el de la red C a la red A.
1. Comprueba la conectividad entre las instancias `vm-a1` y `vm-c1` en ambos sentidos.
1. Comprueba si las rutas entre ambas redes se mantienen.

Así podemos comprobar que, una vez eliminado un emparejamiento de red, la conectividad se interrumpe sin depender de si se elimina también el emparejamiento en la otra red en sentido inverso.

## Resumen de entregas
1. M2U4-4-preguntas.txt: Respuestas a todas las preguntas planteadas en el ejercicio.
1. M2U4-4-tarea_2-archivo_1-captura_1.jpg: Captura de pantalla comprobando la conectividad entre las 2 instancias de VM en uno de los sentidos.
1. M2U4-4-tarea_2-archivo_2-captura_2.jpg: Captura de pantalla comprobando la conectividad entre las instancias de VM `vm-a1` y `vm-b2`.
1. M2U4-4-tarea_3-archivo_1-captura_1.jpg: Captura de pantalla comprobando la conectividad entre las instancias de VM `vm-a1` y `vm-c1`.

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. Elimina las instancias de VM creadas.
1. Elimina las redes de VPC creadas, eliminando las subredes, reglas de cortafuegos y emparejamientos utilizados.
