# Grupos de instancias
Unidad M2U2 - Ejercicio 4

## ¿Qué vamos a hacer?
1. Crear un grupo de instancias no administrados.
1. Trabajar con plantillas de instancias para los grupos de instancias administrados.
1. Crear un grupo de instancias administrado para aplicaciones serverless.
1. Crear un grupo de instancias administrado para aplicaciones stateful.
1. Actualizar aplicaciones en los diferentes grupos de instancias.

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

Para responder a todas las preguntas del ejercicio de forma agrupada, puedes crear antes de empezar un archivo a entregar llamado "M2U2-4-preguntas.txt". Recuerda identificar y ordenar cada pregunta seguida de su respuesta.

Encontrarás las preguntas entre el texto en cursiva: *PREGUNTA: ¿Cómo se llama el servicio de instancias de VMs de Google Cloud?*

### Tarea 1: Grupos de instancias no administrados
Vamos a crear un grupo de instancias no administrado, compuesto por varias instancias creadas manualmente y no administradas por el servicio.

#### Crear la imagen de disco base
Antes que nada, necesitamos crear una imagen de disco base común:
1. En el menú de navegación, navega a **Compute Engine > Instancias de VM**.
1. Pulsa sobre el botón **Crear instancia** para abrir el asistente.
1. Crea una instancia con las siguientes características (*Nota:* siempre que no se indiquen valores para alguna característica significa que uses el valor por defecto):
    - Zona: europe-west1-b.
    - Tipo de máquina: e2-micro.
    - Disco de arranque: Debian GNU/Linux 11.
    - Firewall: permitir tráfico HTTP y HTTPS.
    - Como script de inicio, introduce el siguiente:
    ```
    #! /bin/bash
    apt-get update
    apt-get install apache2 -y
    a2ensite default-ssl
    a2enmod ssl
    vm_hostname="$(curl -H "Metadata-Flavor:Google" http://169.254.169.254/computeMetadata/v1/instance/name)"
    echo "Page served from: $vm_hostname" | tee /var/www/html/index.html
    systemctl restart apache2
    ```
1. Una vez creada la instancia de VM, conéctate a ella por SSH con el botón **SSH** de la consola o cualquier otra de las vías que hemos visto anteriormente.
1. Comprueba la web en local: `curl localhost:80`.
    1. *Nota:* Si la web no funciona correctamente, prueba a ejecutar el script de inicio en la terminal SSH de la instancia.
1. Comprueba el acceso a la instancia pulsando sobre su IP externa en la página de **Compute Engine > Instancias de VM** (*nota:* asegúrate de que te conectas por HTTP y no HTTPS).
1. Una vez comprobado todo, puedes cerrar la conexión SSH con `exit`.
1. Navega a **Compute Engine > Imágenes** y crea una imagen de disco:
    - Nombre: `webserver-v1`.
    - Origen: disco de arranque de la instancia.
    - Familia: `webserver`.
    - Ubicación: multi-regional "eu".
    - Seguir ejecutando instancia.
1. Una vez creada la imagen de disco, puedes eliminar la instancia.

#### Crear un grupo de instancias no administrado
Ahora vamos a crear un grupo de 3 instancias heterogéneas no administrado. Para ello debemos crear dichas instancias manualmente:

1. Navega a **Compute Engine > Instancias de VM**.
1. Crea 3 instancias de VM diferentes con estas características comunes:
    - Zona: europe-west1-b
    - Tipo de máquina: e2-micro, e2-small y e2-medium, respectivamente.
    - Disco: imagen de disco personalizada `webserver-v1`.
    - Etiquetas de red: `network-lb`

Ahora crea un grupo de instancias no administrado y añade las 3 instancias:
1. Navega a **Compute Engine > Grupos de instancias**.
1. Crea un grupo de instancias no administrado con las siguientes características:
    - nombre uig-europe-west1
    - Localización: europe-west1-b.
    - Subred: `default`.
    - Instancias: añade las 3 instancias creadas previamente.

Creamos una regla de firewall para permitir el tráfico entre el balanceador de carga y las instancias de VM:
1. Activa tu entorno de Cloud Shell o una instalación de Cloud SDK local.
1. Crea la regla de firewall con el siguiente comando: `gcloud compute firewall-rules create allow-network-lb --target-tags network-lb --allow tcp:80`.
    1. Dicha regla de firewall llamada `allow-network-lb` se aplicará a las instancias con la etiqueta de red `network-lb` permitiendo el tráfico al puerto TCP:80.
    1. La creamos con `gcloud` directamente, ya que veremos todos los detalles de las reglas de firewall en un módulo posterior.

A continuación creamos el balanceador de carga, con su IP externa y comprobación de salud o "health-check":
1. Reserva una IP externa llamada `network-lb-ip-uig` con el comando `gcloud compute addresses create network-lb-ip-uig --region europe-west1`.
1. Crea el health-check `tcp-health-check` con el comando `gcloud compute health-checks create tcp tcp-health-check --region europe-west1 --port 80`.
1. Crea un servicio de backend `network-lb-backend-service-uig` con el comando:
    ```
    gcloud compute backend-services create network-lb-backend-service-uig \
    --protocol TCP \
    --health-checks tcp-health-check \
    --health-checks-region europe-west1 \
    --region europe-west1
    ```
1. Añade el grupo de instancias al servicio de backend como backend:
    ```
    gcloud compute backend-services add-backend network-lb-backend-service-uig \
    --instance-group uig-europe-west1 \
    --instance-group-zone europe-west1-b \
    --region europe-west1
    ```
1. Crea el balanceador de carga TCP externo regional creando una regla de reenvío al servicio de backend:
    ```
    gcloud compute forwarding-rules create network-lb-forwarding-rule-uig \
    --load-balancing-scheme external \
    --region europe-west1 \
    --ports 80 \
    --address network-lb-ip-uig \
    --backend-service network-lb-backend-service-uig
    ```
1. El balanceador de carga tardará un par de minutos en estar disponible. Puedes comprobar el estado y, una vez disponible, la IP externa del mismo o `IP_EXTERNA`, con el comando `gcloud compute forwarding-rules describe network-lb-forwarding-rule-uig --region europe-west1`.
1. Una vez disponible, comprueba que el balanceador de carga y el grupo de instancias no administrado con la aplicación funcionan correctamente realizando peticiones en bucle a su IP externa: `while true; do curl -m1 IP_EXTERNA; done` (*nota:* puedes cancelar el comando con **CTRL + X**).

De esta forma hemos creado un grupo de instancias no administrado a partir de 3 instancias creadas manualmente, y hemos desplegado un balanceador de carga para permitir tráfico externo y balancearlo entre las mismas.

*ENTREGABLES:*
1. M2U2-3-tarea_1-archivo_1-captura_1.jpg: Captura de la página de detalles del grupo de instancias no administrado.
1. M2U2-3-tarea_1-archivo_2-captura_2.jpg: Captura de la página web de la aplicación mostrando la IP del balanceador de carga y la respuesta de la misma.

### Tarea 2: Grupos de instancias administrados
Además de los grupos de instancias no administrados, podemos utilizar grupos de instancias administrados para aplicaciones serverless y stateful. La principal diferencia entre ellos es que los stateless disponen de autoescalado, mientras que lo stateful no, y las políticas de actualización disponibles para ambos, dadas las características de sus aplicaciones.

#### Crear una plantilla de instancia
Dado que en un grupo de instancias administrado todas las réplicas de instancias son homogéneas entre sí, las instancias se definen a partir de una plantilla de instancia.

Para crear una plantilla de instancia de una forma similar a la creación de una instancia regular, sigue las siguientes instrucciones:
1. En la consola, navega a **Compute Engine > Plantillas de instancias** y pulsa **Crear plantilla de instancias**.
1. Crea una plantilla con la siguiente configuración:
    - Tipo de máquina: e2-micro.
    - Disco de arranque: imagen de disco personalizada `webserver-v1`.
    - Etiquetas de red: `network-lb`
    - *PREGUNTA: ¿Es posible seleccionar la localización de las instancias a crear en una plantilla de instancia?*

*Nota:* Con la etiqueta de red incluida en la plantilla de instancia, la regla de cortafuegos correspondiente se aplicará automáticamente a dichas instancias.

#### Crear un grupo de instancias administrado serverless
Ahora vamos a crear un grupo de instancias administrado serverless siguiendo nuestra plantilla de instancia:

1. En la consola, navega a **Compute Engine > Grupos de instancias** y pulsa **Crear grupo de instancias**.
1. En la columna de la izquierda, selecciona **Nuevo grupo de instancias administrado (sin estado)**.
1. Explora todas las opciones, y crea un grupo de instancias con las siguientes características:
    - Nombre: mig-serverless-europe-west1.
    - Ubicación: varias zonas, región: europe-west1.
    - Plantilla de instancias: la recién creada.
    - Nº mínimo de instancias: 3.
    - Reparación automática: `tcp-health-check`.

Ahora repite los pasos para crear otro balanceador de carga TCP externo regional para el grupo de instancias administrado serverless:
1. Reserva una IP externa llamada `network-lb-ip-serverless` con el comando `gcloud compute addresses create network-lb-ip-serverless --region europe-west1`.
1. Crea un servicio de backend `network-lb-backend-service-serverless` con el comando:
    ```
    gcloud compute backend-services create network-lb-backend-service-serverless \
    --protocol TCP \
    --health-checks tcp-health-check \
    --health-checks-region europe-west1 \
    --region europe-west1
    ```
1. Añade el grupo de instancias al servicio de backend como backend:
    ```
    gcloud compute backend-services add-backend network-lb-backend-service-serverless \
    --instance-group mig-serverless-europe-west1 \
    --instance-group-zone europe-west1-b \
    --region europe-west1
    ```
1. Crea el balanceador de carga TCP externo regional creando una regla de reenvío al servicio de backend:
    ```
    gcloud compute forwarding-rules create network-lb-forwarding-rule-serverless \
    --load-balancing-scheme external \
    --region europe-west1 \
    --ports 80 \
    --address network-lb-ip-serverless \
    --backend-service network-lb-backend-service-serverless
    ```
1. El balanceador de carga tardará un par de minutos en estar disponible. Puedes comprobar el estado y, una vez disponible, la IP externa del mismo o `IP_EXTERNA`, con el comando `gcloud compute forwarding-rules describe network-lb-forwarding-rule-serverless --region europe-west1`.
1. Una vez disponible, comprueba que el balanceador de carga y el grupo de instancias administrado con la aplicación funcionan correctamente realizando peticiones en bucle a su IP externa: `while true; do curl -m1 IP_EXTERNA; done` (*nota:* puedes cancelar el comando con **CTRL + X**).

De esta forma hemos creado un grupo de instancias administrado con un balanceador de carga externo.

*ENTREGABLES:*
1. M2U2-3-tarea_2-archivo_1-captura_1.jpg: Captura de la página de detalles del grupo de instancias administrado serverless.
1. M2U2-3-tarea_2-archivo_2-captura_2.jpg: Captura de la página web de la aplicación mostrando la IP del balanceador de carga y la respuesta de la misma.

#### Crear un grupo de instancias administrado stateful
Los grupos de instancias adminsitrados stateful son muy similares a los serverless, con diferencias mínimas de comportamiento para preservar el estado de la instancia o no disponer de autoescalado.

Para crear un grupo de instancias administrado stateful, sigue los pasos anteriores para crear un grupo de instancias administrado, excepto:
1. Selecciona **Nuevo grupo de instancias administrado (con estado)**.
1. En **Configuración con estado**, marca el disco de inicio como **Stateful**.
1. Como número de instancias, indica 3.

Ahora sigue los mismos pasos para crear un tercer balanceador de carga:
1. Reserva una IP externa llamada `network-lb-ip-stateful` con el comando `gcloud compute addresses create network-lb-ip-stateful --region europe-west1`.
1. Crea un servicio de backend `network-lb-backend-service-stateful` con el comando:
    ```
    gcloud compute backend-services create network-lb-backend-service-stateful \
    --protocol TCP \
    --health-checks tcp-health-check \
    --health-checks-region europe-west1 \
    --region europe-west1
    ```
1. Añade el grupo de instancias al servicio de backend como backend:
    ```
    gcloud compute backend-services add-backend network-lb-backend-service-stateful \
    --instance-group mig-stateful-europe-west1 \
    --instance-group-zone europe-west1-b \
    --region europe-west1
    ```
1. Crea el balanceador de carga TCP externo regional creando una regla de reenvío al servicio de backend:
    ```
    gcloud compute forwarding-rules create network-lb-forwarding-rule-stateful \
    --load-balancing-scheme external \
    --region europe-west1 \
    --ports 80 \
    --address network-lb-ip-stateful \
    --backend-service network-lb-backend-service-stateful
    ```
1. El balanceador de carga tardará un par de minutos en estar disponible. Puedes comprobar el estado y, una vez disponible, la IP externa del mismo o `IP_EXTERNA`, con el comando `gcloud compute forwarding-rules describe network-lb-forwarding-rule-stateful --region europe-west1`.
1. Una vez disponible, comprueba que el balanceador de carga y el grupo de instancias administrado con la aplicación funcionan correctamente realizando peticiones en bucle a su IP externa: `while true; do curl -m1 IP_EXTERNA; done` (*nota:* puedes cancelar el comando con **CTRL + X**).

De esta forma hemos creado un grupo de instancias administrado con un balanceador de carga externo.

*ENTREGABLES:*
1. M2U2-3-tarea_2-archivo_3-captura_3.jpg: Captura de la página de detalles del grupo de instancias administrado stateful.
1. M2U2-3-tarea_2-archivo_4-captura_4.jpg: Captura de la página web de la aplicación mostrando la IP del balanceador de carga y la respuesta de la misma.

### Tarea 3: Actualización de aplicaciones en grupos de instancias
Vamos a comprobar cómo podemos actualizar un grupo de isntancias administrado serverless y stateful.

#### Actualizar la imagen de disco base
Antes de nada, necesitamos actualizar nuestra aplicación y generar una nueva imagen de disco con la nueva versión:
1. En el menú de navegación, navega a **Compute Engine > Instancias de VM**.
1. Pulsa sobre el botón **Crear instancia** para abrir el asistente.
1. Crea una instancia con las siguientes características, cambiando el script de inicio:
    - Zona: europe-west1-b.
    - Tipo de máquina: e2-micro.
    - Disco de arranque: Debian GNU/Linux 11.
    - Firewall: permitir tráfico HTTP y HTTPS.
    - Como script de inicio, introduce el siguiente:
    ```
    #! /bin/bash
    apt-get update
    apt-get install apache2 -y
    a2ensite default-ssl
    a2enmod ssl
    vm_hostname="$(curl -H "Metadata-Flavor:Google" http://169.254.169.254/computeMetadata/v1/instance/name)"
    echo "Version 2 - Page served from: $vm_hostname" | tee /var/www/html/index.html
    systemctl restart apache2
    ```
1. Una vez creada la instancia de VM, conéctate a ella por SSH con el botón **SSH** de la consola o cualquier otra de las vías que hemos visto anteriormente.
1. Comprueba la web en local: `curl localhost:80`.
    1. *Nota:* Si la web no funciona correctamente, prueba a ejecutar el script de inicio en la terminal SSH de la instancia.
1. Comprueba el acceso a la instancia pulsando sobre su IP externa en la página de **Compute Engine > Instancias de VM** (*nota:* asegúrate de que te conectas por HTTP y no HTTPS).
1. Una vez comprobado todo, puedes cerrar la conexión SSH con `exit`.
1. Navega a **Compute Engine > Imágenes** y crea una nueva imagen de disco:
    - Nombre: `webserver-v2`.
    - Origen: disco de arranque de la instancia.
    - Familia: `webserver` (igual que la anterior).
    - Ubicación: multi-regional "eu".
    - Seguir ejecutando instancia.
1. Una vez creada la imagen de disco, puedes eliminar la instancia.

De esta forma, hemos sacado una nueva imagen de disco con la nueva versión de nuestra aplicación, usando la misma familia de imágenes.

#### Actualizar la plantilla de instancia
Para ello, necesitamos una nueva plantilla de instancia con la nueva versión de nuestra aplicación:
1. Navega a **Compute Engine > Plantillas de instancias**.
1. Selecciona la plantilla de instancias previa y pulsa **Copiar**.
1. Las opciones que aparecerán indicadas deben ser las mismas que en la plantilla anterior, comprueba si es así.
1. Actualiza la imagen del disco de arranque a la nueva imagen `webserver-v2`.
1. Guarda la nueva plantilla de instancias con un nombre descriptivo, p. ej. añadiendo `v2` al anterior.

De esta forma hemos generado una plantilla de instancias con la nueva versión de la aplicación incluida. Ahora vamos a utilizar la nueva plantilla de instancias para actualiar los grupos de instancias.

#### Actualizar el grupo de instancias no administrado
En un grupo de instancias no administrado no contamos con ninguna opción administrada por el servicio, por lo que no contamos con auto-actualización.

Para actualizar la aplicación de un grupo de instancias no administrado, lo mejor es crear las nuevas instancias manualmente, crear un nuevo servicio de backend con las mismas y dirigir el balanceador de carga al mismo:

1. Navega a **Compute Engine > Instancias de VM**.
1. Crea 3 nuevas instancias de VM diferentes de forma manual a partir de la nueva plantilla de instancia en europe-west1-b.

Ahora crea un nuevo grupo de instancias no administrado y añade las 3 nuevas instancias:
1. Navega a **Compute Engine > Grupos de instancias**.
1. Crea un grupo de instancias no administrado con las siguientes características:
    - nombre uig-europe-west1-v2
    - Localización: europe-west1-b.
    - Subred: `default`.
    - Instancias: añade las 3 nuevas instancias creadas.

Creamos el nuevo servicio de backend y redirigimos el balanceador de carga del anterior grupo de instancias no administrado al nuevo:
1. Crea un servicio de backend `network-lb-backend-service-uig-v2` con el comando:
    ```
    gcloud compute backend-services create network-lb-backend-service-uig-v2 \
    --protocol TCP \
    --health-checks tcp-health-check \
    --health-checks-region europe-west1 \
    --region europe-west1
    ```
1. Añade el grupo de instancias al servicio de backend como backend:
    ```
    gcloud compute backend-services add-backend network-lb-backend-service-uig-v2 \
    --instance-group uig-europe-west1-v2 \
    --instance-group-zone europe-west1-b \
    --region europe-west1
    ```
1. Actualiza el balanceador de carga TCP externo regional para que apunte al nuevo servicio de backend:
    ```
    gcloud compute forwarding-rules update network-lb-forwarding-rule-uig \
    --region europe-west1 \
    --backend-service network-lb-backend-service-uig-v2
    ```
1. El balanceador de carga tardará un par de minutos en Actualizarse. Puedes comprobar el estado y, una vez disponible, la IP externa del mismo o `IP_EXTERNA` (igual que la anterior), con el comando `gcloud compute forwarding-rules describe network-lb-forwarding-rule-uig --region europe-west1`.
1. Una vez disponible, comprueba que el balanceador de carga y el grupo de instancias no administrado con la aplicación actualizada funcionan correctamente realizando peticiones en bucle a su IP externa: `while true; do curl -m1 IP_EXTERNA; done` (*nota:* puedes cancelar el comando con **CTRL + X**).

*ENTREGABLES:*
1. M2U2-3-tarea_3-archivo_1-captura_1.jpg: Captura de la página de detalles del grupo de instancias no administrado.
1. M2U2-3-tarea_3-archivo_2-captura_2.jpg: Captura de la página web de la aplicación mostrando la IP del balanceador de carga y la respuesta de la misma.

#### Actualizar el grupo de instancias administrado serverless
En un grupo de instancias administrado serverless, sólo debemos indicar que siga la nueva plantilla de instancias para que la política de auto-actualización comience a recrearlas siguiendo la nueva plantilla:
1. Navega a **Compute Engine > Grupos de instancias**, pulsa sobre el grupo `mig-serverless-europe-west1` y pulsa sobre el botón **Actualizar VMs**.
1. Selecciona la nueva plantilla de instancias.
1. En **Configuración de la actualización**, explora las opciones y selecciona como **Tipo de actualización** el tipo **Automático**.
1. Pulsa **Actualizar las VMs** y vuelve a la página de detalles del grupo mientras revisas el estado de la actualización.

Una vez actualizado el grupo, puedes comprobar la aplicación con la IP de su balanceador de carga:
1. Recupera la IP externa del mismo o `IP_EXTERNA` con el comando `gcloud compute forwarding-rules describe network-lb-forwarding-rule-stateless --region europe-west1`.
1. Una vez disponible, comprueba el grupo de instancias administrado realizando peticiones en bucle a su IP externa: `while true; do curl -m1 IP_EXTERNA; done` (*nota:* puedes cancelar el comando con **CTRL + X**).

De esta forma, hemos actualizado un grupo de instancias administrado serverless a una nueva versión con una actualización rodante o "rolling update".

*ENTREGABLES:*
1. M2U2-3-tarea_3-archivo_3-captura_3.jpg: Captura de la página de detalles del grupo de instancias administrado serverless.
1. M2U2-3-tarea_3-archivo_4-captura_4.jpg: Captura de la página web de la aplicación mostrando la IP del balanceador de carga y la respuesta de la misma.

#### Actualizar el grupo de instancias administrado stateful
Las actualizaciones en un grupo de instancias administrado stateful se realizan de la misma forma que para los serverless, con la única diferencia de que se mantiene el estado de las instancias.

Sigue las instrucciones del punto anterior para actualizar tu grupo de instancias administrado stateful y finalmente comprobarlo cuando se hayan actualizado todas las instancias.

*ENTREGABLES:*
1. M2U2-3-tarea_3-archivo_5-captura_5.jpg: Captura de la página de detalles del grupo de instancias administrado serverless.
1. M2U2-3-tarea_3-archivo_6-captura_6.jpg: Captura de la página web de la aplicación mostrando la IP del balanceador de carga y la respuesta de la misma.

## Resumen de entregas
1. M2U2-4-preguntas.txt: Respuestas a todas las preguntas planteadas en el ejercicio.
1. M2U2-3-tarea_1-archivo_1-captura_1.jpg: Captura de la página de detalles del grupo de instancias no administrado.
1. M2U2-3-tarea_1-archivo_2-captura_2.jpg: Captura de la página web de la aplicación mostrando la IP del balanceador de carga y la respuesta de la misma.
1. M2U2-3-tarea_2-archivo_1-captura_1.jpg: Captura de la página de detalles del grupo de instancias administrado serverless.
1. M2U2-3-tarea_2-archivo_2-captura_2.jpg: Captura de la página web de la aplicación mostrando la IP del balanceador de carga y la respuesta de la misma.
1. M2U2-3-tarea_2-archivo_3-captura_3.jpg: Captura de la página de detalles del grupo de instancias administrado stateful.
1. M2U2-3-tarea_2-archivo_4-captura_4.jpg: Captura de la página web de la aplicación mostrando la IP del balanceador de carga y la respuesta de la misma.
1. M2U2-3-tarea_3-archivo_1-captura_1.jpg: Captura de la página de detalles del grupo de instancias no administrado.
1. M2U2-3-tarea_3-archivo_2-captura_2.jpg: Captura de la página web de la aplicación mostrando la IP del balanceador de carga y la respuesta de la misma.
1. M2U2-3-tarea_3-archivo_3-captura_3.jpg: Captura de la página de detalles del grupo de instancias administrado serverless.
1. M2U2-3-tarea_3-archivo_4-captura_4.jpg: Captura de la página web de la aplicación mostrando la IP del balanceador de carga y la respuesta de la misma.
1. M2U2-3-tarea_3-archivo_5-captura_5.jpg: Captura de la página de detalles del grupo de instancias administrado serverless.
1. M2U2-3-tarea_3-archivo_6-captura_6.jpg: Captura de la página web de la aplicación mostrando la IP del balanceador de carga y la respuesta de la misma.

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. En **Servicios de red > Balanceo de carga**, elimina los balanceadores de carga.
1. En **Compute Engine > Grupos de instancias**, elimina los grupos de instancias creados.
1. En **Compure Engine > Instancias de VM**, elimina las instancias que no se hayan eliminado previamente.
1. En **Compure Engine > Discos**, elimina los discos si no se han eliminado automáticamente.
1. En **Compute Engine > Plantillas de instancias**, elimina las plantillas de instancias.
1. En **Compute Engine > Imágenes**, elimina las imágenes creadas.
1. En **Red de VPC > Direcciones IP externas**, libera las direcciones de IP externas reservadas.
