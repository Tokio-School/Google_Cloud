# Cloud Scheduler
Unidad M2U6 - Ejercicio 2

## ¿Qué vamos a hacer?
1. Configurar trabajos cron de mensajes a Cloud Pub/Sub y endpoints HTTPS.
1. Realizar llamadas a APIs de Google Cloud directamente.

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

Para responder a todas las preguntas del ejercicio de forma agrupada, puedes crear antes de empezar un archivo a entregar llamado "M2U6-2-preguntas.txt". Recuerda identificar y ordenar cada pregunta seguida de su respuesta.

Encontrarás las preguntas entre el texto en cursiva: *PREGUNTA: ¿Cómo se llama el servicio de instancias de VMs de Google Cloud?*

### Tarea 1: Configurar trabajos cron
En esta primera tarea vamos a ver cómo crear un trabajo cron que con una frecuencia automatizada envíe un mensaje a Pub/Sub que será consumido por otro servicio.

#### Recepción de mensajes en endpoint serverless
Primero vamos a desplegar una función de Cloud Functions que reciba el mensaje de Pub/Sub enviado automáticamente por Cloud Scheduler.

Para ello, sigue los siguientes pasos:
1. Navega a **Cloud Functions**.
1. Crea una función con las siguientes características:
    - Nombre y descripción: Dale un nombre y descripción según tu criterio
    - Activador: Cloud Pub/Sub
    - Tema de Cloud Pub/Sub: Crea un nuevo tema de Pub/Sub para este ejercicio
    - Fuente: editor en línea
    - Tiempo de ejecución: Node.js 8
    - Archivos `index.js` y `package.json`: contenido por defecto
    - Deja el resto de opciones con sus valores por defecto

De esta forma hemos comenzado creando la aplicación o endpoint con una función serverless de Cloud Function que recibirá y procesará automáticamente el mensaje de Pub/Sub, en este caso simplemente leyéndolo y registrándolo en sus logs de Cloud Logging

#### Creación del trabajo cron
Ahora vamos a crear el trabajo de cron automatizado en Cloud Scheduler que enviará el mensaje de Pub/Sub.

Para ello, sigue los siguientes pasos:
1. En la consola, navega a **APIs y servicios > Biblioteca** y habilita la API de Cloud Scheduler
1. Navega a **Cloud Scheduler**.
1. Crea un trabajo con las siguientes características:
    - Nombre y descripción: Dale un nombre y descripción según tu criterio
    - Frecuencia: Escoge una frecuencia según el formato [unix-cron](https://man7.org/linux/man-pages/man5/crontab.5.html), p. ej. `* * * * *` para cada minuto
    - Zon horaria: Madrid
    - Tipo de destino: Pub/Sub
    - Tema: Escoge el tema creado en el paso anterior
    - Mensaje: Añade un mensaje. p. ej. `hola mundo!`
    - Deja el resto de opciones con sus valores por defecto

De esta forma hemos creado un trabjao cron automatizado que enviará un mensaje de Pub/Sub a la función suscrita con la frecuencia indicada.

#### Verificar
Por último, vamos a verificar nuestro despliegue:

1. Puedes esperar a la primera ejecución del trabajo cron, o ejecutarla manualmente
1. En la página de **Cloud Scheduler**, pulsa **Ejecutar ahora** para tu trabajo
    1. La primera vez que se ejecuta un trabajo puede tardar unos pocos minutos más debido a unas configuraciones internas necesarias
1. Espera al resultado en la columna **Resultado**
1. Comprueba los logs del trabajo con el botón **Ver** de la columna **Registros**
1. Del mismo modo, comprueba la función navegando a **Cloud Functions**
1. Pulsa en el nombre de la función antes creada
1. En la página de detalles de la función, en la pestaña **General**, comprueba el nº de invocaciones para ver si se ha invocado la función
1. Comprueba también la invocación de la función y que ha recogido el mensaje de Pub/Sub correctamente en el botón **Ver registros**

De esta forma hemos podido verificar nuestro despliegue por completo, verificando cada una de sus partes individualmente.

*ENTREGABLES:*
1. M2U6-2-tarea_1-archivo_1-captura_1.jpg: Captura de pantalla mostrando los logs de invocación del trabajo de Cloud Scheduler.
1. M2U6-2-tarea_1-archivo_2-captura_2.jpg: Captura de pantalla mostrando los logs de invocación de la función con su mensaje.

### Tarea 2: Realizar llamadas a APIs de Google Cloud
En esta otra tarea, vamos a utilizar Cloud Scheduler para invocar directamente una operación de Google Clouda través de sus APIs.

Esta operación se puede realizar llamando a las APIs directamente, o utilizando las librerías de cliente de Cloud Client Libraries para implementarlo utilizando código, utilizando p. ej. una función de Cloud Functions llamada directamente o a través de Pub/Sub, como en la tarea anterior.

Habitualmente es más sencillo implementarlo y testearlo utilizando código y Cloud Functions, pero si fuera una API de un servicio externo, este trabajo cron que envíe directamente peticiones HTTP sería la única opción. Del mismo modo, lo implementaremos de esta forma para mostrar otro tipo de destino del trabajo cron disponible.

Un ejemplo muy habitual es utilizar un trabajo cron serverless para encender y apagar entorno de desarrollo/pruebas fuera de horario de trabajo.

En este caso, por tanto, vamos a crear 2 trabajos cron que van a encender y apagar automáticamente una instancia de VM a las 9:00 y 17:00 a través de la API de Compute Engine.

Para ello, sigue los siguientes pasos:
1. Crea una instancia de VM (preferiblemente de nucleo compartido)
1. Crea una cuenta de servicio para el trabajo de cron de Cloud Scheduler, y dale el rol de `Administrador de instancias de Compute`
1. Crea 2 trabajos de Cloud Scheduler con las siguientes características:
    - Nombres: `inicio-instancias-desarrollo` y `apagado-instancias-desarrollo`
    - Cuenta de servicio
    - Frecuencia: `0 9 * * 1-5`
    - Zona horaria: Madrid
    - Tipo de destino: HTTP
    - URL: Para obtener la URL correcta, busca en la documentación de Compute Engine la referencia de la API para los métodos `start` y `stop`. Utiliza el explorador de API para comprobar la URL (puedes maximizarlo para ver más detalles del explorador)
    - Método HTTP: POST
    - Encabezado de la autenticación: Agregar token de OAuth
    - Cuenta de servicio: La cuenta de servicio creada en el paso anterior
    - Deja el resto de opciones con sus valores por defecto

Para verificar tu despliegue, utiliza el botón **Ejecutar ahora** de los trabajos de Cloud Scheduler y verifica que la instancia de VM se enciende/apaga a través de la consola y de los logs de ejecución de los trabajos.

De esta forma hemos desplegado 2 trabajos de Cloud Scheduler que automatizan el encendido y apagado de una instancia de VM con un entorno de desarrollo.

*ENTREGABLES:*
1. M2U6-2-tarea_2-archivo_1-captura_1.jpg: Captura de pantalla mostrando los logs de invocación del trabajo de Cloud Scheduler de encendido de la instancia de VM.
1. M2U6-2-tarea_2-archivo_2-captura_2.jpg: Captura de pantalla mostrando los logs de invocación del trabajo de Cloud Scheduler de encendido de la instancia de VM.

## Resumen de entregas
1. M2U6-2-tarea_1-archivo_1-captura_1.jpg: Captura de pantalla mostrando los logs de invocación del trabajo de Cloud Scheduler.
1. M2U6-2-tarea_1-archivo_2-captura_2.jpg: Captura de pantalla mostrando los logs de invocación de la función con su mensaje.
1. M2U6-2-tarea_2-archivo_1-captura_1.jpg: Captura de pantalla mostrando los logs de invocación del trabajo de Cloud Scheduler de encendido de la instancia de VM.
1. M2U6-2-tarea_2-archivo_2-captura_2.jpg: Captura de pantalla mostrando los logs de invocación del trabajo de Cloud Scheduler de encendido de la instancia de VM.

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. Elimina la instancia de VM
1. Elimina la cuenta de servicio
1. Elimina el tema de Pub/Sub
1. Elimina la función de Cloud Functions
1. Elimina los trabajos de Cloud Scheduler
