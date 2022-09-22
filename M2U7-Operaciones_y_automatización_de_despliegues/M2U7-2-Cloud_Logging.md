# Cloud Logging
Unidad M2U7 - Ejercicio 2

## ¿Qué vamos a hacer?
1. Recibir logs de una instancia de VM con una aplicación web.
1. Instalar el agente de operaciones.
1. Visualizar logs por varios métodos.
1. Analizar logs de auditoría.

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

Para responder a todas las preguntas del ejercicio de forma agrupada, puedes crear antes de empezar un archivo a entregar llamado "M2U7-2-preguntas.txt". Recuerda identificar y ordenar cada pregunta seguida de su respuesta.

Encontrarás las preguntas entre el texto en cursiva: *PREGUNTA: ¿Cómo se llama el servicio de instancias de VMs de Google Cloud?*

### Tarea 1: Logs de una instancia de VM
En esta primera tarea, vamos a trabajar con los logs creados por una instancia de VM con un servidor web.

Estos logs recogerán todo lo enviado a syslog, stdout y stderr en la instancia, y serán enviados a Cloud Logging, desde donde podremos revisarlos junto al resto de logs.

#### Desplegar instancia de VM
Primero comienza por desplegar la instancia de VM y un servidor web.

Para ello, sigue los siguientes pasos:
1. Crea una instancia de VM de Debian con acceso HTTP
1. Conéctate por SSH, instala un servidor web Apache y comprueba su acceso
1. Instala el agente de Ops: [Instalar la última versión del agente (Linux)](https://cloud.google.com/stackdriver/docs/solutions/agents/ops-agent/installation#agent-install-latest-linux)
1. Haz múltiples peticiones a la aplicación web accediendo a la IP de la instancia en tu navegador

De esta forma hemos desplegado una instancia de VM con el agente de Ops instalado y hemos generado logs con el tráfico a la aplicación web.

#### Comprobar logs de la instancia y aplicación
Para comprobar los logs de la instancia, podemos hacerlo desde la página de la instancia o desde Cloud Logging:
1. En la consola web, navega a **Compute Engine > Instancias de VM** y pulsa sobre el nombre de tu instancia
1. Pulsa sobre el botón **Registros**
1. Este botón te llevará a la interfaz de Cloud Logging de **Registros** con los logs de tu instancia ya filtrados

Explora la interfaz de Cloud Logging y familiarízate con sus opciones:
1. Filtrar logs de un servicio o recurso en concreto
1. Modificar el período de tiempo
1. Modificar el orden de registros, por orden de tiempo ascendiente o descendiente
1. Filtrar según nivel de importancia

*ENTREGABLE: M2U7-2-tarea_1-archivo_1-captura_1.jpg: Captura de pantalla de Cloud Logging mostrando los logs de la instancia de VM.*

### Tarea 2: Logs de auditoría
Ahora vamos a trabajar con los logs de auditoría. Estos logs nos permitirán ver quién ha hecho qué operación, cuándo y en qué recurso, para mantener una auditoría de operaciones y seguridad.

#### Logs de auditoría de administrador
Primero realiza varias operaciones que generen logs de auditoría de administrador:
1. Crea un bucket de Cloud Storage, reserva una dirección de IP estática externa y crea una regla de cortafuegos cualquiera

Ahora comprueba dichos logs de auditoría:
1. En la consola web, navega a la página de inicio y pulsa en la pestaña **Actividad**
1. Revisa dichos logs. Puedes utilizar los filtros en la columna de la derecha para filtrar entre ellos
1. Ahora navega a **Registros > Explorador de registros**
1. En el menú desplegable **Nombre de registro**, busca **CLOUD AUDIT > actividad**
1. Revisa los logs en los resultados de la búsqueda

*ENTREGABLE: M2U7-2-tarea_2-archivo_1-captura_1.jpg: Captura de pantalla de Cloud Logging mostrando los logs de auditoría de administrador.*

## Resumen de entregas
1. M2U7-2-tarea_1-archivo_1-captura_1.jpg: Captura de pantalla de Cloud Logging mostrando los logs de la instancia de VM.
1. M2U7-2-tarea_2-archivo_1-captura_1.jpg: Captura de pantalla de Cloud Logging mostrando los logs de auditoría de administrador.

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. Elimina la instancia de VM creada, el bucket de Cloud Storage y la regla de cortafuegos
1. Libera la dirección de IP estática externa
