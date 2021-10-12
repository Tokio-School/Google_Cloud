# Cloud Memorystore
Unidad M2U3 - Ejercicio 6

## ¿Qué vamos a hacer?
1. Crear una instancia de Cloud Memorystore.
1. Conectarnos desde una instancia de VM cliente.
1. Desplegar una aplicación web de ejemplo.

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

*Nota: No hay preguntas para este ejercicio.*

### Tarea 1: Conectarse a una instancia de Cloud Memorystore
En esta tarea vamos a crear una instancia de Cloud Memorystore y vamos a conectarnos desde una instancia de VM cliente.

Primero habilita la API de Cloud Memorystore for Redis.

#### Crear una instancia de Cloud Memorystore
Vamos a comenzar creando la instancia de Cloud Memorystore:
1. En la consola, navega a **Memorystore > Redis** y pulsa **Crear instancia**.
1. Explora las diferentes opciones y crea una instancia con las siguientes características:
    1. ID de instancia: `redis-instance`.
    1. Nivel: básico.
    1. Zona: europe-west1-b.
    1. Capacidad: 5 GB.
    1. Versión: 6.x
    1. Red de VPC autorizada: `projects/ID_PROYECTO/global/networks/default`.
1. Copia la dirección de IP de la instancia

*Nota:*
1. Si tienes algún problema con los rangos de IPs al crear la instancia, navega a **Red de VPC > Redes de VCP > red default**, pulsa sobre **Conexión privada a servicios** y asigna un rango de IP automático con longitud de 20.
1. Si sigues teniendo problemas, crea una nueva VPC de modo personalizado y una subnet en `europe-west1` ([docs](https://cloud.google.com/vpc/docs/using-vpc#create-custom-network)) y habilita Private Services Accesss en la misma ([docs](https://cloud.google.com/vpc/docs/configure-private-services-access)).

Espera a que la instancia de Cloud Memorystore para Redis se haya creado y copia la IP de la instancia, que llamaremos `IP_INSTANCIA_REDIS`.

De esta forma podemos desplegar una instancia de Cloud Memorystore compatible con Redis para utilizarla como memoria caché, BBDD en memoria, etc.

#### Crear una instancia de VM cliente
Ahora vamos a crear una instancia de VM para actuar como cliente y conectarnos a la instancia de Cloud Memorystore.

Crea una instancia de VM con las siguientes características:
- Nombre: `client-vm`.
- Zona: `europe-west1-b`.
- Tipo de máquina: `e2-micro`.
- Disco de arranque: Debian 11, 10 GB estándar.
- Redes: Deshabilita la IP externa, para que sólo disponga de IP interna.
- *Nota:* Si has utilizado una nueva VPC para la instancia de Cloud Memorystore, selecciona dicha red en **Redes**.

#### Conexión desde el cliente
Ahora configura y comprueba el acceso desde la instancia de VM:
1. Instala la herramienta `telnet`: `sudo apt-get install telnet`.
1. Conéctate a la instancia de Redis usando `telnet`: `telnet IP_INSTANCIA_REDIS 6379`.
1. Comprueba la conexión en la terminal a Redis: `PING`. Debes recibir `PONG` como respuesta.
1. Comprueba la creación de claves: `SET HELLO WORLD` y `GET HELLO`. Debe responder `WORLD`.

De esta forma nos hemos conectado a la instancia de Redis desde una instancia de VM cliente.

*ENTREGABLES:* M2U3-6-tarea_1-archivo_1-captura_1.jpg: Captura de pantalla de la conexión a la terminal de Redis mostrando la respuesta de los comandos `PING`, `SET HELLO WORLD` y `GET HELLO`.

### Tarea 2: Desplegar una aplicación web de ejemplo
Ahora vamos a desplegar una aplicación web de ejemplo que utiliza una instancia de Redis de Cloud Memorystore.

La aplicación mantiene un conteo de las visitas a su endpoint `/` en Redis. Comienza por explorar los archivos de su [repositorio](https://github.com/GoogleCloudPlatform/python-docs-samples/tree/master/memorystore/redis).

#### Desplegar la aplicación web
La aplicación necesita una instancia de Cloud Memorystore ya desplegada, con su IP interna localizada y copiada.

También necesita un bucket de GCS, por lo que crea un bucket regional en europe-west1 y anota su nombre, `NOMBRE_BUCKET`.

Una vez desplegado el bucket, despliega la aplicación desde Cloud Shell o tu instalación de Cloud SDK local:
1. Clona el repositorio: `git clone https://github.com/GoogleCloudPlatform/python-docs-samples`.
1. Abre el directorio correspondiente: `cd python-docs-samples/memorystore/redis/gce_deployment`.
1. El script `deploy.sh` despliega el artefacto de la aplicación a GCS, despliega una instancia de GCE, ejecuta la aplicación con un script de inicio de la instancia y crea una regla de firewall para habilitar tráfico a la misma.
1. Establece las variables de entorno locales para configurar el acceso a la instancia de Redis: `export REDISHOST=IP_INSTANCIA_REDIS` y `export REDISPORT=6379`.
1. Establece la variable de entorno local para configurar el acceso al bucket de GCS: `export GCS_BUCKET_NAME=NOMBRE_BUCKET`.
1. Habilita el script como ejecutable y ejecútalo: `chmod +x deploy.sh` y `./deploy.sh`.

#### Comprobar la aplicación web
Una vez que la instancia de VM está lista, copia su IP externa y accede desde tu navegador: `http://IP_EXTERNA:8080`.

Puedes usar el script `teardown.sh` en local para eliminar la instancia de VM y la regla de firewall: `chmod +x teardown.sh` y `./teardown.sh`.

*ENTREGABLES:* M2U3-6-tarea_2-archivo_1-captura_1.jpg: Captura de pantalla de la página web mostrando su URL.

## Resumen de entregas
1. M2U3-6-tarea_1-archivo_1-captura_1.jpg: Captura de pantalla de la conexión a la terminal de Redis mostrando la respuesta de los comandos `PING`, `SET HELLO WORLD` y `GET HELLO`.
1. M2U3-6-tarea_2-archivo_1-captura_1.jpg: Captura de pantalla de la página web mostrando su URL.

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. Elimina las instancias de VM.
1. Elimina la instancia de Cloud Memorystore.
1. Elimina el bucket de GCS.
1. Si has creado una VPC adicional, elimínala.
