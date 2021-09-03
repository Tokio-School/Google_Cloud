# Presupuestos y alertas
Unidad M2U1

## ¿Qué vamos a hacer?
1. Crear presupuestos y alertas.
1. Recibir notificaciones programáticas de alertas.

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

*NOTA: No hay preguntas para este ejercicio.*

### Tarea 1: Crear presupuestos y alertas
Vamos a crear un presupuesto y alertas para evitar problemas con gastos excesivos durante el curso.

#### Crear un presupuesto y alertas
1. En la consola web, accede a **Facturación > Presupuestos y alertas**.
1. Explora todas las opciones para analizar sus posibilidades.
1. Crea un presupuesto con estas características, dejando el resto con los valores por defecto:
    - Intervalo de tiempo: por mes.
    - Proyectos: todos.
    - Servicios: todos.
    - Créditos: descuentos y promociones y otros créditos.
    - Tipo de presupuesto e importe: importe de 100 €.
    - Acciones: límites del 50%, 90% y 100% del presupuesto real mensual.
    - Notificaciones: alertas a los administradores y usuarios de la cuenta de facturación.

### Tarea 2: Recibir notificaciones programáticas de alertas
Ahora vamos a establecer unas notificaciones programáticas de prueba para controlar el gasto de forma selectiva, apagando unas instancias de VM pero no eliminándolas ni eliminando información de Cloud Storage, usando una función de Cloud Functions ejecutada automáticamente.

#### Crear entorno
Antes de continuar, habilita la API de Cloud Functions desde la consola web, navegando a **APIs y servicios > Biblioteca**.

1. Crea 2 instancias de VM con la siguiente configuración:
    - Zona: europe-west1-b
    - Imagen y familia de imagen: Debian 10 Buster
    - Tipo de máquina: e2-micro (*Nota: puedes comprobar los tipos disponibles con `gcloud compute machine-types list --zones europe-west1-b`*)
    - Utiliza el comando `gcloud compute instances create NOMBRE_VM --image=debian-10-buster-v20200309 --image-project=ID_PROYECTO --machine-type="e2-micro"` ([docs](https://cloud.google.com/compute/docs/instances/create-start-instance#publicimage))
1. Crea un bucket de Cloud Storage con el comando `gsutil mb -p ID_PROYECTO -c STANDARD -l EUROPE-WEST1 -b on gs://NOMBRE_BUCKET` (*Nota: no modifiques los argumentos de `STANDARD` y `EUROPE-WEST1` del comando*).
1. En la consola web, navega a **Compute Engine > Instancias de VM** y comprueba que se han creado correctamente.
1. Navega a **Storage > Navegador** y comprueba que el bucket se ha creado correctamente.
1. Accede al bucket de Cloud Storage y, utilizando la consola web, sube cualquier archivo desde local.

#### Notificaciones programáticas
Ahora vamos a desplegar una función de Cloud Functions que recibirá una alerta automática enviada a través de Cloud Pub/Sub cada vez que se alcance un límite del presupuesto, y llegado al 100% del mismo apagará las instancias de VM para controlar de forma selectiva el gasto.

Para ello, sigue las instrucciones disponibles públicamente en la [documentación](https://cloud.google.com/billing/docs/how-to/notify#selectively_control_usage):
- Crea un tema de Cloud Pub/Sub siguiendo la [documentación](https://cloud.google.com/billing/docs/how-to/budgets-programmatic-notifications#manage-notifications).
- Edita el código a desplegar en la función y añade la ID de tu proyecto y `europe-west1-b` como zona.
- Las últimas versiones de runtime de Node.js y Python tienen disponible la variable de entorno de `GCP_PROJECT`.
- Sigue las instrucciones para añadir el rol de "Compute Instance Admin (v1)" a la cuenta de servicio que utiliza Cloud Functions por defecto.
- Para testear el proceso, sigue las instrucciones de la [documentación](https://cloud.google.com/billing/docs/how-to/notify#test-your-cloud-function).
- Para comprobar que las instancias se han apagado, en la consola navega a **Compute Engine > Instancias de VM**.
- Si ocurre cualquier error, analiza los logs de la función de Cloud Functions navegando a **Cloud Functions**, accediendo a tu función y a la opción de acceder a los logs de la misma.

*ENTREGABLE:* M2U1-5-tarea_2-archivo_1-captura.jpg: captura de los logs de la función de Cloud Functions mostrando los nombres de las instancias de VM apagadas.

## Resumen de entregas
1. M2U1-5-tarea_2-archivo_1-captura.jpg: captura de los logs de la función de Cloud Functions mostrando los nombres de las instancias de VM apagadas.

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. Elimina las instancias de VM creadas, en **Compute Engine > Instancias de VM**.
1. Elimina el bucket de Cloud Storage, en **Storage > Navegador**.
1. Desactiva la opción de notificaciones programáticas de las alertas del presupuesto.
1. Elimina la función de Cloud Functions, en **Cloud Functions**.
1. Elimina el tema de Cloud Pub/Sub, en **Cloud Pub/Sub > Temas**.
