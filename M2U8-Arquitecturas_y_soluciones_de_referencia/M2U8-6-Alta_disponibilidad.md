# Arquitectura de alta disponibilidad
Unidad M2U8 - Ejercicio 6

## ¿Qué vamos a hacer?
1. Desplegar un entorno de nube híbrida con alta disponibilidad en patrón frío, con despliegue en la nube ante cualquier desastre.
1. Desplegar un aplicativo simulando una infraestructura "on-premise", configurando sus backups y guardando la información en la nube para prevenir desastres.
1. Ante cualquier problema a nivel regional que afecte a la aplicación, desplegar un entorno de alta disponibilidad o recuperación ante desastres a partir de los backups e información replicada.

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

*Nota: no hay preguntas para este ejercicio.*

### Tarea 1: Despliegue de solución "on-premise"
Desplega la siguiente solución en tu entorno de trabajo:

*Nota:* Si algún requisito técnico no se describe expresamente, utiliza los valores por defecto o escoge la solución que estimes más conveniente.

Utilizaremos una zona interna de Cloud DNS para simular una zona externa que redireccionara nuestro dominio público a la IP del balanceador de carga.

Entorno "on-premise":
- Red:
    - Nombre: `on-prem`
    - Modo de creación de subredes: personalizado
    - Subred en europe-west1
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
    - Cuenta de servicio individual para el MIG
    - Sin IPs externas
    - Snapshots automáticos de los discos de arranque programados cada hora y almacenados en multi-región "europe"
- Balanceador de carga HTTPS balanceando tráfico al MIG:
    - Frontal con IPv4 reservado
    - Protocolo HTTP en TCP:80
    - Afinidad de sesión en L7
- Cloud SQL:
    - MySQL 8.0
    - Región: europe-west1
    - Tipo de máquina: 1 vCPU, 1.7 GB memoria
    - HDD 100 GB
    - Conexión interna, sin IP externa (*nota:* habilitar Private Service Access)
    - Copias de seguridad almacenadas en multi-región europea
- Cloud IAP habilitado para permitir tunelización SSH a las instancias sin IP externa
- Reglas de cortafuegos:
    - Aplicativo: ingress desde LB HTTPS permitido a TCP:80
    - SSH (TCP:22) desde cualquier origen habilitado a todas las instancias
    - ICMP desde origen interno de subred habilitado a todas las instancias
    - Aplicado según cuentas de servicio
- Cloud DNS:
    - Zona privada para red `on-prem`.
    - Nombre de DNS: `www.myhaapp.com`.
    - Reigstro: Tipo A hacia IP externa de LB HTTS

Requisitos técnicos:
- El aplicativo tiene que ser accesible desde el balanceador de carga.
- El aplicativo tiene que poder acceder a Cloud SQL
- Una instancia de VM interna en la VPC `on-prem` tiene que poder resolver la URL `www.myhaapp.com` a la aplicación web del MIG.

Datos:
- Crea algunos archivos en en los discos de todas las instancias para que se incluyan en los backups (p. ej. en la imagen inicial).
- Crea una base de datos y tabla y sube algunos datos de ejemplo a Cloud SQL ([ejemplo](https://cloud.google.com/sql/docs/mysql/quickstart#create_a_database_and_upload_data)).

Backups:
- Asegúrate de disponer de al menos un backup de los discos y Cloud SQL antes de continuar.
- Puedes forzarlos o crearlos de forma manual si es necesario.

### Tarea 2: Despliegue de entorno de HA/DR en frío en la nube
Desplega la siguiente solución en tu entorno de trabajo:

*Nota:* Si algún requisito técnico no se describe expresamente, utiliza los valores por defecto o escoge la solución que estimes más conveniente.

La VPN simulará la conexión entre el "on-premise" y la nube para sincronizar los backups e información.

Entorno "cloud":
- Red:
    - Nombre: `cloud`
    - Modo de creación de subredes: personalizado
    - Subred en europe-west3
    - Enrutamiento global
    - Private Google Access o "Acceso Privado a Google" habilitado
- VPN:
    - Túneles de conexión conectando ambas VPCs entre sí, `on-prem` y `cloud`
    - VPN de HA
    - Enrutamiento basado en Cloud Router
- Cloud IAP habilitado para permitir tunelización SSH a las instancias sin IP externa
- Reglas de cortafuegos:
    - Aplicativo: ingress desde LB HTTPS permitido a TCP:80
    - SSH (TCP:22) desde cualquier origen habilitado a todas las instancias
    - ICMP desde origen interno de subred habilitado a todas las instancias
    - Aplicado según cuentas de servicio
- Cloud DNS:
    - Actualiza la zona interna para que también tenga visibilidad desde la VPC `cloud`.

Requisitos técnicos:
- Crea una instancia de VM interna en la red `cloud` y verifica la conectividad desde y hacia ambos entornos.
- Una instancia de VM interna en la VPC `on-prem` tiene que poder resolver la URL `www.myhaapp.com` a la aplicación web del MIG "on-prem" a través de la IP pública del LB.

### Tarea 3: Recuperación de alta disponibilidad
Simula la recreación del entorno en la nube en la VPC `cloud`:

1. Despliega otro MIG en europe-west3 creando una plantilla de instancia a partir del último snapshot disponible.
1. Recrea la instancia de Cloud SQL en europe-west3 a partir del backup de la instancia inicial.
1. Crea un nuevo balanceador de carga para el MIG "cloud".
1. Sustituye la resolución DNS a la IP externa del nuevo LB "cloud".
1. Comprueba que el nuevo entorno funciona correctamente, con el LB apuntando a las instancias, las instancias y BBDD con la información correspondiente y acceso a Cloud SQL.

## Resumen de entregas
Capturas de pantalla mostrando el listado o los detalles de cada elemento creado y las conexiones previstas:
- Detalles de la red `on-prem`
- Listado de las reglas de cortafuegos
- Detalle del grupo de instancias `on-prem`
- Detalle del balanceador de carga `on-prem`
- Resolución desde la instancia interna a `www.myhaapp.com`, mostrando la IP del LB `on-prem`
- Acceso desde las instancias a Cloud SQL
- Detalles de la red `cloud`
- Detalle del grupo de instancias `cloud`
- Detalle del balanceador de carga `cloud`
- Resolución desde la instancia interna a `www.myhaapp.com`, mostrando la IP del LB `cloud`

*Nota:* Sigue la misma numeración propuesta hasta ahora: "M2U[nº de unidad]-[nº de ejercicio]-tarea_[nº de tarea]-archivo_[nº de archivo en la tarea]-[tipo de archivo: captura, exportado, script]_[nº de archivo si hay más de 1 del mismo tipo]", p. ej. "M2U8-1-tarea_1-archivo_1-captura_1.jpg".

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. Elimina todos los recursos creados, asegurándote de no dejar ninguno que pueda seguir incurriendo en costes o de luegar a un error en otro ejercicio.
