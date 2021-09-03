# Planificación de costes
Unidad M2U1

## ¿Qué vamos a hacer?
1. Planificar costes de Google Cloud y realizar presupuestos de ejemplo.

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

*NOTA: No hay preguntas para este ejercicio.*

### Tarea 1: Calculadora de costes y listado de precios
Para la primera tarea, vamos a explorar algunos listados de precios de los servicios de Google Cloud y la calculadora de costes.

#### Listado de precios
En la documentación de cada servicio podemos encontrar la información sobre los costes del mismo, junto con todos los detalles necesarios y varios ejemplos de casos de uso con el detalle y total de los costes asociados.

Puedes encontrar los costes al final de la página de documentación de cada servicio (puedes buscarla en un buscador web) o en el [listado de precios](https://cloud.google.com/pricing/list) general.

Busca y analiza los costes detallados y ejemplos para los sigiuientes servicios:
- Compute Engine: instancias de VM
- Cloud Storage
- Cloud Run
- Cloud Firestore

#### Calculadora de costes de Google Cloud
La calculadora de costes de Google Cloud es la herramienta idónea para explorar de forma general los costes de una solución o hacer presupuestos detallados para una propuesta.

Accede a ella: [Calculadora de costes](https://cloud.google.com/products/calculator).

1. Analiza la calculadora de costes, y familiarízate con las distintas secciones.
1. Explora los distintos servicios, y prueba a añadir algunas instancias de VM (y discos) p. ej.
1. Cambia los resultados para mostrarlos en €.
1. Modifica y elimina recursos para comprobar cómo modificar un presupuesto.
1. Al acabar, guarda el presupuesto y/o envíatelo por email. Al crear el presupuesto, genera una URL con una ID única del mismo.

### Tarea 2: Presupuestos de ejemplo
En esta tarea vas a crear 3 presupuestos de ejemplo utilizando la calculadora de costes. Recuerda crear un presupuesto diferente para cada uno de ellos (eliminando la ID del mismo de la URL para crear uno nuevo si es necesario).

#### Presupuesto 1 - entorno simple
Crea el siguiente presupuesto y guarda su URL:

1. Región: europe-west1.
1. 3 instancias de VM e2-standard-8 Ubuntu.
    1. Sin IPs externas.
    1. En ejecución 24/7.
    1. Disco balanceado zonal de 200 GB por instancia.
    1. 200 GB de snapshot multi-regional por instancia.
1. 1 TB de tráfico de descarga/egress de internet a Europa mensual.
1. Balanceador de carga HTTPS:
    1. 1 regla de reenvío.
    1. 200 GB procesados.
1. Cloud Storage:
    1. 1 TB alojado en estándar regionalmente.
    1. 10M de operaciones Clase A mensuales.
    1. 50M de operaciones Clase B mensuales.
    1. Sin tráfico fuera de la región.
1. Cloud NAT:
    1. 1 pasarela NAT.
    1. 10 GB procesados.

*ENTREGABLES*: M2U1-7-presupuestos.txt: Añadir URL de presupuesto y coste total.

#### Presupuesto 2 - entorno complejo
Crea el siguiente presupuesto y guarda su URL:

1. 12 instancias de VM e2-standard-4 Ubuntu Pro:
    1. Sin IPs externas.
    1. En ejecución 24/7.
    1. Disco balanceado zonal de 200 GB por instancia.
    1. 200 GB de snapshot multi-regional por instancia.
    1. 6 instancias en europe-west1 y 6 en europe-west3.
1. 6 instancias de VM e2-standard-8 Red Hat Enterprise Linux:
    1. Sin IPs externas.
    1. En ejecución 24/7.
    1. Disco SSD zonal de 300 GB por instancia.
    1. 300 GB de snapshot multi-regional por instancia.
    1. 3 instancias en europe-west1 y 3 en europe-west3.
1. 4 instancias de VM e2-standard-16 Windows Server:
    1. Sin IPs externas.
    1. En ejecución 24/7.
    1. Disco SSD zonal de 500 GB por instancia.
    1. 400 GB de snapshot multi-regional por instancia.
    1. Instancias en europe-west3.
1. Clúster de GKE estándar:
    1. Región: europe-west1 (clúster regional).
    1. 6 instancias e2-standard-8.
    1. En ejecución 24/7.
    1. 200 GB de disco balanceado por instancia.
    1. 100 GB de snapshot multi-regional por instancia.
1. Tráfico de descarga/egress recurso-a-recurso:
    1. 24 TB de tráfico entre regiones en Europa.
1. Tráfico de descarga/egress de internet:
    1. 25 TB de tráfico a Europa.
    1. 12 TB de tráfico a norteamérica.
    1. 24 TB de tráfico a sudamérica.
1. Balanceador de carga HTTPS:
    1. 6 reglas de reenvío.
    1. 20 TB procesados.
1. Cloud Storage:
    1. 10 TB alojado en estándar multi-regionalmente en Europa.
    1. 20 TB alojado en coldline multi-regionalmente en Europa.
    1. 100M de operaciones Clase A mensuales.
    1. 500M de operaciones Clase B mensuales.
    1. 10 TB de tráfico a las instancias de VM.
1. Cloud NAT:
    1. 3 pasarelas NAT.
    1. 50 GB procesados.
1. Cloud Functions:
    1. Memoria: 256 MB.
    1. Tiempo de ejecución: 250 ms.
    1. Ancho de banda: 100 MB.
    1. Nº invocaciones: 500.000.
1. Cloud SQL:
    1. 1 instancia MySQL db-standard-4.
    1. 500 GB de almacenamiento SSD.
    1. 1 TB de backup.
    1. En ejecución 24/7.
    1. Región: europe-west1.
1. BigQuery (a demanda):
    1. Región: multi-regional, Unión Europea.
    1. 500 GB de almacenamiento activo.
    1. 2 TB almacenamiento a largo plazo.
    1. Datos procesados: 15 TB.

*ENTREGABLES*: M2U1-7-presupuestos.txt: Añadir la URL del presupuesto y coste total.

#### Presupuesto 3 - entorno enterprise
Crea el siguiente presupuesto y guarda su URL:

1. 32 instancias de VM e2-standard-8 Ubuntu Pro:
    1. Sin IPs externas.
    1. En ejecución 24/7.
    1. Disco balanceado zonal de 250 GB por instancia.
    1. 250 GB de snapshot multi-regional por instancia.
    1. 6 instancias en europe-west1, 5 en europe-west3, 6 en us-east2, 4 en us-east1, 4 en us-west1, 7 en us-west2.
1. 20 instancias de VM e2-standard-8 Red Hat Enterprise Linux:
    1. Sin IPs externas.
    1. En ejecución 24/7.
    1. Disco SSD zonal de 300 GB por instancia.
    1. 300 GB de snapshot multi-regional por instancia.
    1. 4 instancias en europe-west1, 4 en europe-west3, 4 en us-east2, 4 en us-west1, 4 en asia-east1.
1. 15 instancias de VM e2-standard-16 Windows Server:
    1. Sin IPs externas.
    1. En ejecución 24/7.
    1. Disco SSD zonal de 500 GB por instancia.
    1. 500 GB de snapshot multi-regional por instancia.
    1. 3 instancias en europe-west1, 3 en europe-west3, 3 en us-east2, 3 en us-west1, 3 en asia-east1.
1. Clústeres de GKE estándar:
    1. Región: europe-west1 (clúster regional):
        1. 6 instancias e2-standard-8.
        1. En ejecución 24/7.
        1. 250 GB de disco balanceado por instancia.
        1. 150 GB de snapshot multi-regional por instancia.
    1. Región: us-central1 (clúster regional):
        1. 5 instancias e2-standard-8.
        1. En ejecución 24/7.
        1. 250 GB de disco balanceado por instancia.
        1. 150 GB de snapshot multi-regional por instancia.
    1. Región: asia-east1 (clúster regional):
        1. 4 instancias e2-standard-8.
        1. En ejecución 24/7.
        1. 250 GB de disco balanceado por instancia.
        1. 150 GB de snapshot multi-regional por instancia.
1. Tráfico de descarga/egress recurso-a-recurso:
    1. 50 TB de tráfico entre regiones en Europa.
    1. 24 TB de tráfico entre regiones en EE.UU.
    1. 20 TB de tráfico entre regiones en Asia.
    1. 15 TB de tráfico entre Europa y EE.UU
    1. 12 TB de tráfico entre Europa y Asia.
    1. 10 TB de tráfico entre EE.UU y Asia.
1. Tráfico de descarga/egress de internet:
    1. 40 TB de tráfico a Europa.
    1. 21 TB de tráfico a norteamérica.
    1. 16 TB de tráfico a Asia.
1. Balanceador de carga HTTPS:
    1. 12 reglas de reenvío.
    1. 60 TB procesados.
1. Cloud Storage:
    1. 50 TB alojado en estándar multi-regionalmente en Europa.
    1. 100 TB alojado en nearline multi-regionalmente en Europa.
    1. 300 TB alojado en coldline multi-regionalmente en Europa.
    1. 500 TB alojado en archive multi-regionalmente en Europa.
    1. 500M de operaciones Clase A mensuales.
    1. 1000M de operaciones Clase B mensuales.
    1. 25 TB de tráfico a las instancias de VM en Europa.
    1. 20 TB de tráfico a las instancias de VM en EE.UU.
    1. 12 TB de tráfico a las instancias de VM en Asia.
1. Cloud NAT:
    1. 5 pasarelas NAT.
    1. 2 TB procesados.
1. Cloud Functions:
    1. Memoria: 512 MB.
    1. Tiempo de ejecución: 300 ms.
    1. Ancho de banda: 150 MB.
    1. Nº invocaciones: 10M.
1. Cloud SQL:
    1. Región: europe-west1:
        1. 1 instancia MySQL db-highmem-8.
        1. 2 TB de almacenamiento SSD.
        1. 3 TB de backup.
        1. En ejecución 24/7.
    1. Región: us-central1:
        1. 1 instancia MySQL db-highmem-4.
        1. 1 TB de almacenamiento SSD.
        1. 2 TB de backup.
        1. En ejecución 24/7.
    1. Región: asia-east1:
        1. 1 instancia MySQL db-standard-4.
        1. 500 GB de almacenamiento SSD.
        1. 1 TB de backup.
        1. En ejecución 24/7.
1. BigQuery (a demanda):
    1. Región: multi-regional, Unión Europea:
        1. 1 TB de almacenamiento activo.
        1. 6 TB almacenamiento a largo plazo.
        1. Datos procesados: 24 TB.
    1. Región: multi-regional, EE.UU.:
        1. 500 GB de almacenamiento activo.
        1. 3 TB almacenamiento a largo plazo.
        1. Datos procesados: 12 TB.
1. Cloud Pub/Sub:
    1. Volumen de mensajes mensual: 10 GB.
    1. Backlog: 50 GB.
1. BigTable:
    1. Región: europe-west1.
    1. 4 nodos.
    1. 400 GB de almacenamiento SSD.
    1. En ejecución 24/7.
1. Cloud Memorystore para Redis:
    1. Región: europe-west1.
    1. Tier: Estándar.
    1. 100 GB de almacenamiento.
1. Cloud Filestore:
    1. Región: europe-west1.
    1. 250 GB de almacenamiento en tier premium.

*ENTREGABLES*: M2U1-7-presupuestos.txt: Añadir la URL del presupuesto y coste total.

## Resumen de entregas
1. *ENTREGABLES*: M2U1-7-presupuestos.txt: Añadir las URLs y coste total de los 3 presupuestos.

## Limpiar recursos
*No hay recursos que eliminar tras este ejercicio.*
