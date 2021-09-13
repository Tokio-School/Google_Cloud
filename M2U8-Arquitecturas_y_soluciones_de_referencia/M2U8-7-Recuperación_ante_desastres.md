# NOMBRE_DEL_EJERCICIO
Unidad M2U0

## ¿Qué vamos a hacer?
1. Desplegar un entorno de recuperación ante desastres en patrón templado.
1. Desplegar un entorno principal en una región y recuperarlo ante cualquier desastre en otra región.
1. Automatizar la recuperación con un script.

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

*Nota: no hay preguntas para este ejercicio.*

### Tarea 1: Despliegue de solución
Desplega la siguiente solución en tu entorno de trabajo:

*Nota:* Si algún requisito técnico no se describe expresamente, utiliza los valores por defecto o escoge la solución que estimes más conveniente.

Nuestra región principal será `europe-west1` y la de backup `europe-west3`.

- Red:
    - Modo de creación de subredes: personalizado
    - Subredes en europe-west1 y europe-west3
    - Enrutamiento global
    - Private Google Access o "Acceso Privado a Google" habilitado
- Aplicativo:
    - Grupo de instancias administrado de GCE
    - Regional: europe-west1 (distribuido por todas las zonas)
    - Tipo de instancias: e2-micro
    - Disco de arranque: imagen personalizada, disco persistente balanceado 20 GB
    - Autoescalable: 3-10 instancias, escalado basado en CPU (70%)
    - Health-check: conexión TCP puerto 80
    - Imagen personalizada: Debian 11 ejecutando el servidor web Apache2 en TCP:80
    - Cuenta de servicio individual para el MIG, con acceso de lectura y escritura a Cloud Storage
    - Sin IPs externas
    - Snapshots automáticos de los discos de arranque programados cada hora y almacenados en multi-región "europe"
- Balanceador de carga HTTPS balanceando tráfico al MIG:
    - Frontal con IPv4 e IPv6 reservados
    - Protocolo HTTP en TCP:80
    - Afinidad de sesión en L7
- Cloud SQL:
    - MySQL 8.0
    - Región: europe-west1
    - Tipo de máquina: 1 vCPU, 1.7 GB memoria
    - HDD 100 GB
    - Conexión interna, sin IP externa (*nota:* habilitar Private Service Access)
    - Configuración de alta disponibilidad
    - Copias de seguridad almacenadas en multi-región europea
    - Réplica de lectura en europe-west3
- Cloud Storage:
    - Bucket multi-regional en Europa
    - Clase de almacenamiento estándar
- Cloud IAP habilitado para permitir tunelización SSH a las instancias sin IP externa
- Reglas de cortafuegos:
    - Aplicativo: ingress desde LB HTTPS permitido a TCP:80
    - SSH (TCP:22) desde cualquier origen habilitado a todas las instancias
    - ICMP desde origen interno de subred habilitado a todas las instancias
    - Aplicado según cuentas de servicio

Requisitos técnicos:
- El aplicativo tiene que ser accesible desde el balanceador de carga.
- El aplicativo tiene que poder acceder a Cloud SQL y Cloud Storage.

Datos:
- Crea algunos archivos en en los discos de todas las instancias para que se incluyan en los backups (p. ej. en la imagen inicial).
- Crea una base de datos y tabla y sube algunos datos de ejemplo a Cloud SQL ([ejemplo](https://cloud.google.com/sql/docs/mysql/quickstart#create_a_database_and_upload_data)).

Backups:
- Asegúrate de disponer de al menos un backup de los discos y Cloud SQL antes de continuar.
- Puedes forzarlos o crearlos de forma manual si es necesario.

### Tarea 2: Recuperación de desastres
Simula la recreación del entorno en otra región a partir de los snapshots de los discos y la réplica de lectura de la BBDD:

1. Crea una nueva plantilla de instancia a partir del último snapshot de disco disponible.
1. Recrea el MIG en `europe-west3` usando la nueva plantilla de instancia.
1. Asciende la réplica de lectura de Cloud SQL a máster, configura su alta disponibilidad y crea una réplica de lectura en `europe-west1`.
1. Sustituye el backend en `europe-west1` del LB por un nuevo backend con el MIG de `europe-west3`.
1. Comprueba que el nuevo entorno funciona correctamente, con el LB apuntando a las instancias, las instancias y BBDD con la información correspondiente y acceso a Cloud SQL y Cloud Storage.

## Resumen de entregas
Capturas de pantalla mostrando el listado o los detalles de cada elemento creado y las conexiones previstas:
- Detalles de la red y subredes.
- Listado de las reglas de cortafuegos.
- Detalle del grupo de instancias de `europe-west1`.
- Detalle de la instancia de Cloud SQL en `europe-west1`
- Detalle del balanceador de carga con la configuración inicial.
- Acceso a la aplicación web a través de la IP externa del LB.
- Acceso desde las instancias a Cloud SQL.
- Acceso desde las instancias a Cloud Storage.
- Misma información tras la recuperación a la región de backup.

*Nota:* Sigue la misma numeración propuesta hasta ahora: "M2U[nº de unidad]-[nº de ejercicio]-tarea_[nº de tarea]-archivo_[nº de archivo en la tarea]-[tipo de archivo: captura, exportado, script]_[nº de archivo si hay más de 1 del mismo tipo]", p. ej. "M2U8-1-tarea_1-archivo_1-captura_1.jpg".

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. Elimina todos los recursos creados, asegurándote de no dejar ninguno que pueda seguir incurriendo en costes o de luegar a un error en otro ejercicio.
