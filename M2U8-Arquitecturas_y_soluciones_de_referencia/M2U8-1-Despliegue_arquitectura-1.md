# Arquitectura de referencia 1
Unidad M2U0 - Ejercicio 1

## ¿Qué vamos a hacer?
1. Desplegar una aplicación web de 3 capas.

Este ejercicio es un ejercicio de reto, donde tendrás que desplegar la arquitectura descrita sin instrucciones paso a paso asociadas. Para completarlo, te recomendamos basarte en lo visto en otros ejercicios, la documentación oficial o cualquier otro recurso online (tutoriales, StackOverflow, videos, etc.).

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

Para responder a todas las preguntas del ejercicio de forma agrupada, puedes crear antes de empezar un archivo a entregar llamado "M2U0-0-preguntas.txt". Recuerda identificar y ordenar cada pregunta seguida de su respuesta.

*Nota: no hay preguntas para este ejercicio.*

### Tarea única

Desplega la siguiente solución en tu entorno de trabajo:

*Nota:* Si algún requisito técnico no se describe expresamente, utiliza los valores por defecto o escoge la solución que estimes más conveniente.

- Red:
    - Modo de creación de subredes: personalizado
    - 1 subred en europe-west1
    - Enrutamiento global
    - Private Google Access o "Acceso Privado a Google" habilitado
- Frontend:
    - Grupo de instancias administrado de GCE
    - Regional: europe-west1 (distribuido por todas las zonas)
    - Tipo de instancias: e2-micro
    - Disco de arranque: imagen personalizada, disco persistente balanceado 20 GB
    - Autoescalable: 3-10 instancias, escalado basado en CPU (70%)
    - Health-check: conexión TCP puerto 80
    - Imagen personalizada: Debian 11 ejecutando el servidor web Apache2 en TCP:80
    - Cuenta de servicio individual para el MIG
    - Sin IPs externas
- Balanceador de carga HTTPS balanceando tráfico al MIG de frontend:
    - Frontal con IPv4 e IPv6 reservadas
    - Protocolo HTTP en TCP:80
    - Afinidad de sesión en L7
- Backend:
    - Grupo de instancias administrado de GCE
    - Regional: europe-west1 (distribuido por todas las zonas)
    - Tipo de instancias: e2-small
    - Disco de arranque: imagen personalizada, disco persistente balanceado 20 GB
    - Autoescalable: 3-10 instancias, escalado basado en CPU (70%)
    - Health-check: conexión TCP puerto 80
    - Imagen personalizada: Debian 11 ejecutando el servidor web Apache2 en TCP:80, cliente mysql y Cloud SQL Auth proxy
    - Cuenta de servicio individual para el MIG
    - Sin IPs externas
- Balanceador de carga TCP interno regional balanceando tráfico al MIG de backend:
    - Afinidad de sesión (tupla de 5 elementos)
    - Acceso global
- Cloud IAP habilitado para permitir tunelización SSH a las instancias sin IP externa
- Reglas de cortafuegos:
    - Frontend: ingress desde LB HTTPS permitido a TCP:80
    - Backend: ingress desde frontends permitido a TCP:80
    - Tráfico egress a la BBDD bloqueado excepto desde backend (TCP:3306) (*nota: usa las prioridades de diferentes reglas de cortafuegos*)
    - SSH (TCP:22) desde cualquier origen habilitado a todas las instancias
    - ICMP desde origen interno de subred habilitado a todas las instancias
    - Aplicado según cuentas de servicio
    - Deshabilitar el resto de tráfico
- Cloud SQL:
    - MySQL 8.0
    - Localización: europe-west1-b
    - Tipo de máquina: 1 vCPU, 1.7 GB memoria
    - HDD 100 GB
    - Conexión interna, sin IP externa (*nota:* habilitar Private Service Access)
    - Configuración de alta disponibilidad
    - Copias de seguridad y ventanas de mantenimiento habilitadas

Requisitos técnicos:
- Los frontend tienen que ser accesibles desde el balanceador de carga
- Los backend tienen que ser accesibles desde los frontend a través de su balanceador de carga
- La BBDD tiene que ser accesible sólo desde los backend

## Resumen de entregas
Capturas de pantalla mostrando el listado o los detalles de cada elemento creado y las conexiones previstas:
- Detalles de la red y listado de subredes
- Listado de las reglas de cortafuegos
- Detalle de cada grupo de instancias
- Detalle de cada balanceador de carga
- Detalle de la instancia de Cloud SQL
- Conexión externa a LB HTTPS
- Conexión desde frontend a LB interno/backend
- Conexión desde backend a BBDD (terminal MySQL)

*Nota:* Sigue la misma numeración propuesta hasta ahora: "M2U[nº de unidad]-[nº de ejercicio]-tarea_[nº de tarea]-archivo_[nº de archivo en la tarea]-[tipo de archivo: captura, exportado, script]_[nº de archivo si hay más de 1 del mismo tipo]", p. ej. "M2U8-1-tarea_1-archivo_1-captura_1.jpg".

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. Elimina todos los recursos creados, asegurándote de no dejar ninguno que pueda seguir incurriendo en costes o de luegar a un error en otro ejercicio.
