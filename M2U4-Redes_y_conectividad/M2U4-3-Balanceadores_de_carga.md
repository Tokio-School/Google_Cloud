# Balanceadores de carga
Unidad M2U4 - Ejercicio 3

## ¿Qué vamos a hacer?
1. Crear 2 clústeres de frontend regionales y otro de backend regional con una aplicación web.
1. Desplegar un balanceador de carga interno regional para el clúster de backend y otro externo global para los clústeres de frontend.
1. Añadir un bucket de Cloud Storage al balanceador de carga externo y redirigir tráfico según destino.
1. Comprobar la alta escalabilidad y disponibilidad de los clústeres.

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

*NOTA: No hay preguntas para este ejercicio.*

### Tarea 1: Creación del clúster de backend y el balanceador de carga interno
Vamos a comenzar con el despliegue de la red, clúster de backend y balanceador de carga interno para el mismo. En este caso, por simplificar, vamos a desplegar sólo un clúster de backend, aunque en muchos aplicativos se pueden desplegar duplicados en varias regiones.

#### Crear la red VPC
Comenzamos por crear la red VPC, subredes y reglas de cortafuegos:

1. Crea una red VPC con las siguientes características:
    - Nombre: `custom-mode-net`.
    - Modo de creación de subred: Personalizados.
1. Crea 2 subredes con las siguientes características:
    - Nombre: `custom-mode-net-subnet-euw1`.
        - Región: europe-west1.
        - Rango de direcciones IP: 10.10.0.0/20.
    - Nombre: `custom-mode-net-subnet-euw3`.
        - Región: europe-west3.
        - Rango de direcciones IP: 10.20.0.0/20.
1. Crea las siguientes reglas de cortafuegos:
    - Nombre: `allow-ssh-ping-internet-all`.
        - Red: `custom-mode-net`.
        - Dirección de tráfico: Entrada.
        - Acción en caso de coincidencia: Permitir.
        - Destinos: Todas las instancias de la red.
        - Protocolos y puertos: TCP:22 e ICMP
    - Nombre: `allow-http-frontend-backend`.
        - Red: `custom-mode-net`.
        - Dirección de tráfico: Entrada.
        - Acción en caso de coincidencia: Permitir
        - Destinos: Etiquetas de destino especificadas, `webapp-apache2-backend`.
        - Filtro de origen: Etiquetas de origen, `webapp-apache2-frontend`.
        - Protocolos y puertos: TCP:80

#### Crear el clúster de backend
Ahora vamos a comenzar el proceso de despliegue de una aplicación en un clúster de instancias regional. Para ello primero tenemos que crear una imagen de disco, luego una plantilla de instancia que la incluya y finalmente el clúster.

Comienza por crear una imagen de disco con la aplicación desplegada:
1. Crea una instancia de VM con las siguientes características:
    - Tipo de instancia: e2-micro.
    - Disco de arranque: Disco persistente equilibrado nuevo, 10 GB, Debian 11.
    - Red: `custom-mode-net`.
1. Conéctate por SSH a la instancia e instala el servidor web Apache2: `sudo apt update && sudo apt install -y apache2`.
1. Comprueba que el servidor web está activo: `curl localhost:80`.
1. Apaga la instancia de VM, navega a **Compute Engine > Imágenes** y pulsa en **Crear imagen**.
1. Explora las opciones disponibles y crea una imagen de disco con las siguientes características:
    - Nombre: `webapp-apache2-v1`.
    - Origen: disco, disco de origen: disco de arranque de la instancia creada anteriormente.
    - Ubicación: Multiregional.
    - Familia: `webapp-apache2`.
1. A continuación, navega a **Compute Engine > Plantillas de instancias**, pulsa en **Crear plantilla de instancias** y crea una con las siguientes características:
    - Nombre: `webapp-apache2-backend-template`.
    - Tipo de máquina: e2-small.
    - Disco de arranque: Imagen personalizada, `webapp-apache2-v1`, disco persistente equilibrado, 10 GB.
    - Etiquetas de red: `webapp-apache2-backend`.
    - Red: `custom-mode-net`.
    - Sin IP externa.

Por último, vamos a crear el grupo de instancias administrado con la aplicación:
1. Navega a **Compute Engine > Grupos de instancias** y pulsa en **Crear grupo de instancias**.
1. Selecciona **Nuevo grupo de instancias administrado (sin estado)** en la columna de la izquierda.
1. Explora las opciones disponibles y crea un MIG con las siguientes características:
    - Nombre: `webapp-apache2-backend`.
    - Ubicación: varias zonas (todas), región: europe-west1.
    - Plantilla de instancias: `webapp-apache2-backend-template`.
    - Ajuste de escala automático: Ajustar escala automáticamente.
    - Nº mínimo de instancias: 3, nº máximo: 10.
    - Reparación automática: crear una verificación de estado
    - Verificación de estado: `hc-http`, TCP:80

Una vez creado el clúster, abre una sesión SSH a cada una de las instancias y comprueba que el servidor web está activo: `curl localhost:80`.

De esta forma hemos seguido todos los pasos para desplegar una aplicación web en un grupo de instancias administrado.

*ENTREGABLE:* M2U4-tarea_1-archivo_1-captura_1.jpg: Captura de pantalla mostrando la página de detalles del MIG de backend.

#### Crear el balanceador de carga interno
Por último, vamos a crear un balanceador de carga de red interno para balancear el tráfico desde los clústeres de frontend al de backend:

1. Navega a **Servicios de red > Balanceo de carga** y pulsa **Crear balanceador de cargas**.
1. Selecciona **Balanceo de cargas de TCP**, **Solo entre mis VM** y **Solo en una región**.
1. Explora las opciones disponibles y crea un balanceador de carga con las siguientes características:
    - Nombre: `webapp-apache2-backend-lb`.
    - Región: europe-west1.
    - Red: `custom-mode-net`.
    - Configuración de backend:
        - Nuevo backend con grupo de instancias `webapp-apache2-backend`.
        - Verificación de estado: crear nueva, `hc-http` alcance global, TCP:80
    - Configuración de frontend:
        - Subred: `custom-mode-net-subnet-euw1`.
        - Puertos: Única, 80
        - Acceso global: Habilitar
        - Etiqueta de servicio: `webapp-apache2-backend`.
    - Usa la opción de **Revisión y finalización** para comprobar tu configuración y crea el balanceador.

Una vez creado, necesitamos crear una instancia interna para comprobar el acceso al mismo.

1. Crea una instancia de VM con las siguientes características:
    - Nombre: `internal-client`.
    - Región: europe-west1.
    - Tipo de máquina: e2-standard-4.
    - Red: `custom-mode-net`.
    - Etiquetas de red: `webapp-apache2-frontend`.
1. Conéctate por SSH a la misma.
1. Comprueba que puedes hacer ping a las instancias del MIG backend: `ping -c 3 IP_INTERNA_BACKEND`.
1. Comprueba que puedes acceder a su aplicación web: `curl http://IP_LB_INTERNO:80` y `curl webapp-apache2-frontend`.

De esta forma podemos desplegar un balanceador de carga para permitir tráfico interno entre los frontend y el grupo de instancias de backend de una aplicación.

*ENTREGABLE:* M2U4-tarea_1-archivo_2-captura_2.jpg: Captura de pantalla mostrando la página de detalles del balanceador de carga interno.

### Tarea 2: Creación de los clústeres de frontend y el balanceador de carga externo
En esta tarea vamos a crear el grupo de instancias administrado de frontend y el balanceador de carga externo para los mismos.

#### Crear los clústeres de frontend
Del mismo modo que hemos hecho antes, vamos a desplegar un grupo de instancias administrado de frontend.

Como en este caso utilizarán la misma aplicación, puedes duplicar la plantilla de instancia:
1. Navega a **Compute Engine > Plantillas de instancias**, selecciona la plantilla de instancia creada y pulsa **Copiar**.
1. Comprueba que tiene la misma configuración que la plantilla anterior, y simplemente cambia su etiqueta de red: `webapp-apache2-backend-template` a `webapp-apache2-frontend-template`.

Ahora crea los 2 grupos de instancias para los frontend:
1. Navega a **Compute Engine > Grupos de instancias** y pulsa en **Crear grupo de instancias**.
1. Selecciona **Nuevo grupo de instancias administrado (sin estado)** en la columna de la izquierda.
1. Crea un MIG con las siguientes características:
    - Nombre: `webapp-apache2-frontend-usc1`.
    - Ubicación: varias zonas (todas), región: us-central1.
    - Plantilla de instancias: `webapp-apache2-frontend-template`.
    - Ajuste de escala automático: Ajustar escala automáticamente.
    - Nº mínimo de instancias: 3, nº máximo: 10.
    - Reparación automática: crear una verificación de estado
    - Verificación de estado: `hc-http`, TCP:80
1. Crea un segundo MIG con las siguientes características:
    - Nombre: `webapp-apache2-frontend-euw1`.
    - Ubicación: varias zonas (todas), región: europe-west1.
    - Plantilla de instancias: `webapp-apache2-frontend-template`.
    - Ajuste de escala automático: Sin ajuste de escala automático.
    - Nº mínimo de instancias: 1.
    - Reparación automática: verificación de estado `hc-http`.

Así hemos podido crear 2 clústeres de instancias, el más cercano a nosotros con baja capacidad y sin autoescalado (para comprobaciones posteriores), y un clúster lejano con autoescalado.

#### Subir archivos estáticos a Cloud Storage
También vamos a ver cómo podemos utilizar un bucket de Cloud Storage como servicio de backend de un balanceador de carga HTTPS.

Para ello, crea un bucket de Cloud Storage regional en europe-west1 y sube una imagen cualquiera en JPG llamada `image.jpg`, p. ej. descargada de internet.
Por último, edita los permisos de acceso del bucket para permitir el acceso público a todos los objetos.

#### Crear el balanceador de carga global externo
Como último paso de la tarea, vamos a crear el balanceador de carga externo global HTTP(S) para los 2 clústeres de frontend regionales:

1. Navega a **^Servicios de red > Balanceo de cargas** y pulsa sobre **Crear balanceador de cargas**.
1. Selecciona **Balanceo de cargas HTTP(S)** y **De internet a mis VM**.
1. Explora las diferentes opciones y crea un balanceador de carga con las siguientes características:
    - Nombre: `web-apache2-frontend-lb`.
    - Configuración de backend:
        - Servicios de backend:
            - Nombre: `web-apache2-frontend-lb-webserver`.
            - Tipo de backend: Grupo de instancias
            - Protocolo: HTTP
            - Puerto con nombre: `http`.
            - Backends:
                - Grupo de instancias:
                    - Grupo: `webapp-apache2-frontend-euw1`.
                        - Capacidad: 50%.
                - Grupo de instancias:
                    - Grupo: `webapp-apache2-frontend-usc1`.
                        - Capacidad: 100%.
        - Bucket de backend:
            - Nombre: `web-apache2-frontend-lb-bucket`.
            - Bucket: el bucket de GCS creado previamente.
    - Reglas de host y ruta:
        - Host y rutas de acceso: cualquiera que no coincida (predeterminado), backends: `web-apache2-frontend-lb-webserver`.
        - Host: `*`, Rutas de acceso: `/static/*`, backends: `web-apache2-frontend-lb-bucket`.
    - Configuración de frontend:
        - Nombre: `web-apache2-frontend-lb-fe`.
        - Protocolo: HTTP.
        - Versión de IP: IPv4.
        - Dirección IP: crear dirección IP estática, `web-apache2-frontend-lb-fe-ipv4`.

Una vez completado el proceso, tardará un par de minutos en crearse.

Mientras tanto, puedes crear la regla de cortafuegos que permitirá a las instancias de los MIG de frontend recibir tráfico desde el proxy del balanceador de carga:
- Nombre: `allow-http-lbproxy-frontend`.
    - Red: `custom-mode-net`.
    - Dirección de tráfico: Entrada.
    - Acción en caso de coincidencia: Permitir
    - Destinos: Etiquetas de destino especificadas, `webapp-apache2-frontend`.
    - Filtro de origen: Rangos de IP, 130.211.0.0/22 y 35.191.0.0/16.
    - Protocolos y puertos: TCP:80

Una vez desplegado, haz las siguientes comprobaciones de conectividad:
1. Conéctate a una de las instancias de frontend e intenta conectarte con `curl` a las web de las instancias de backend y a `curl http://IP_LB_INTERNO:80` y `curl webapp-apache2-frontend`.
1. Conéctate a la IP pública del balanceador de carga HTTP(S) desde tu navegador (*nota:* asegúrate de que usas HTTP en lugar de HTTPS).
1. Comprueba que tras el balanceador de carga puedes acceder a la imagen de Cloud Storage: `http://IP_LB_EXTERNO/static/image.jpg`.

Así hemos podido desplegar 2 grupos de instancias de frontend en regiones diferentes y un balanceador de carga global que de acceso a los mismos.

*ENTREGABLE:* M2U4-tarea_2-archivo_1-captura_1.jpg: Captura de pantalla mostrando la página de detalles del balanceador de carga externo.

### Tarea 3: Comprobar la alta escalabilidad y disponibilidad
En esta última tarea vamos a comprobar la alta escalabilidad y disponibilidad de los grupos de instancias.

#### Alta disponibilidad
Para comprobar la alta disponibilidad, vamos a apagar algunas instancias de VM y ver cómo el grupo de instancias administrado las recrea de nuevo:

1. Navega a **Compute Engine > Instancias**.
1. Localiza alguna de las instancias de cada grupo de instancias, selecciónalas y elimínalas.
1. Navega **Compute Engine > Grupos de instancias** y comprueba cómo el nº de instancias actuales y objetivo de los grupos no coincide (p. ej. "2/3").
1. Al lado de cada grupo, un icono mostrará que el estado del grupo está en renovación, y tras unos segundos volverá a mostrar otra vez cómo el nº de instancias actuales y objetivo coincide de nuevo (p. ej. "3/3").

#### Alta escalabilidad
Para comprobar la alta escalabilidad de los clústeres, vamos a comprobar cómo el tráfico se redirige a la siguiente región más cercana si el clúster más cercano no puede sostener tanta carga, o un clúster objetivo autoescalará en nº de instancias:

1. Conéctate por SSH a la instancia `internal-client`.
1. Instala el servidor web Apache2, que nos dará acceso a la herramienta `ab`: `sudo apt update && sudo apt install -y apache2`.
1. Localiza y copia la dirección IP externa del balanceador de carga HTTP(S), `LB_IP_EXTERNA`.
1. Ejecuta una prueba de carga con `ab`: `ab -n 500000 -c 1000 http://LB_IP_EXTERNA`, deja el comando y la sesión de SSH corriendo en segundo plano.
1. Observa el tráfico de los backends navegando a **Servicios de red > Balanceo de cargas**, pulsando en la pestaña **Backends** y sobre `web-apache2-frontend-lb-webserver`.

Comprobarás cómo el tráfico se dirige originalmente al MIG de frontend más cercano, en europe-west1, y cuando no puede copar con más carga se redirige parte del mismo al otro MIG de frontend en us-central1.

También puedes comprobar el estado y nº de instancias en el MIG de europe-west1 en **Compute Engine > Grupos de instancias**, pulsando sobre dicho MIG.

*Nota:* Si lo necesitas para forzar más carga, puedes re-ejecutar el comando `ab` ampliando el nº de conexiones concurrentes con `-c N_CONEXIONES`.

*ENTREGABLE:* M2U4-tarea_3-archivo_1-captura_1.jpg: Captura de pantalla mostrando la página de direccionamiento de tráfico del servicio de backend del balanceador de carga.

## Resumen de entregas
1. M2U4-tarea_1-archivo_1-captura_1.jpg: Captura de pantalla mostrando la página de detalles del MIG de backend.
1. M2U4-tarea_1-archivo_2-captura_2.jpg: Captura de pantalla mostrando la página de detalles del balanceador de carga interno.
1. M2U4-tarea_2-archivo_1-captura_1.jpg: Captura de pantalla mostrando la página de detalles del balanceador de carga externo.
1. M2U4-tarea_3-archivo_1-captura_1.jpg: Captura de pantalla mostrando la página de direccionamiento de tráfico del servicio de backend del balanceador de carga.

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. Elimina la imagen de disco creada.
1. Elimina los grupos de instancias creados.
1. Elimina la instancia de cliente.
1. Elimina la plantilla de instancias.
1. Elimina la red creada, los balanceadores de carga y la IP externa reservada.
