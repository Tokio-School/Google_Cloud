# Cloud Monitoring
Unidad M2U7 - Ejercicio 1

## ¿Qué vamos a hacer?
1. Crear un Workspace de Cloud Monitoring.
1. Monitorizar un grupo de instancias.
1. Crear un dashboard personalizado.
1. Crear una política de alertas.

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

Para responder a todas las preguntas del ejercicio de forma agrupada, puedes crear antes de empezar un archivo a entregar llamado "M2U7-1-preguntas.txt". Recuerda identificar y ordenar cada pregunta seguida de su respuesta.

Encontrarás las preguntas entre el texto en cursiva: *PREGUNTA: ¿Cómo se llama el servicio de instancias de VMs de Google Cloud?*

### Tarea 1: Monitorizar un grupo de instancias
En esta tarea vamos a crear un dashboard de monitorización personalizado para el clúster de instancias de VM de una aplicación web.

Para ello crearemos un workspace, un grupo de recursos a monitorizar, su dashboard personalizado, una política de alertas y una monitorización de estado activo.

#### Desplegar un grupo de instancias
Antes de nada, tenemos que desplegar nuestros recursos a monitorizar.

Básate en ejercicios anteriores para seguir los siguientes pasos:
1. Crea una instancia de VM de núcleo compartido con Debian e instala un servidor web Apache
1. Crea una plantilla de instancias y Grupo de Instancias Administrado llamado `webapp` de 3 instancias mínimo con acceso HTTP a partir de una imagen de disco de la misma
1. Comprueba que puedes acceder a la web de muestra de las instancias desde tu navegador

Una vez desplegado nuestro clúster con la aplicación web, podemos crear el dashboard de monitorización.

#### Crear un Workspace
Primero tenemos que crear un workspace de Cloud Monitoring o "espacio de trabajo" para monitorizar nuestro proyecto, y opcionalmente otros a la vez.

Para ello, en la consola web navega a **Monitorización** y espera que el workspace se cree por defecto.

#### Grupo de recursos
Un grupo de recursos agrupará a las instancias de VM para monitorizarlas en conjunto.

Para crear el grupo, sigue los siguientes pasos:
1. En **Monitorización**, en el panel a la izquierda, pulsa en **Grupos** y en **Crear grupo**
1. Nombre del grupo: `webapp`
1. Criterio: contiene `webapp` en el nombre de la instancia
1. Pulsa **Crear**

Automáticamente el servicio crea un dashboard por defecto con información agragada de tu conjunto de instancias.

#### Crear un dashboard personalizado
Para crear un dashboard personalizado con diversas métricas por defecto y personalizadas, sigue los siguientes pasos:
1. En **Monitorización**, en el panel a la izquierda, pulsa **Panel de control** y **Crear panel**
1. Crea un nuevo dashboard con un nombre a tu elección
1. Añade una gráfica de la **Librería de gráficas**, como la de **línea**
1. Dale un título a tu elección
1. En el campo **Recursos y métricas**, busca `Utilización de CPU` y pulsa **Añadir**

Una vez creado tu dashboard, también puedes explorar las diferentes métricas sin tener que crear una gráfica previamente desde la pestaña **Explorador de métricas**.

*ENTREGABLE: M2U7-1-tarea_1-archivo_1-captura_1.jgp: Captura de pantalla mostrando el dashboard personalizado.*

#### Crear una política de alertas
Vamos a crear una política para recibir alertas automatizadas por diversos canales, como email, si una métrica supera o es inferior a un valor límite.

Para ello, sigue los siguientes pasos:
1. En **Monitorización > Alertas**, pulsa **Crear política**
1. Pulsa en **Seleccionar una métrica**, busca **Instancia de VM > Instancia** y selecciona `Utilización de CPU`, pulsa **Aplicar**
1. Como **Ventana rodante**, selecciona 1 minuto
1. Como posición de alerta, selecciona **Sobre límite** y un valor de 20
1. Pulsa **Siguiente**
1. En **Canales de notificación**, pulsa en **Administrar canales**
1. Añade un canal de email nuevo con tu dirección de correo electrónico
1. En la pestaña anterior, pulsa en el canal creado
1. Dale un nombre a la alerta y pulsa en **Crear política**

#### Monitorización de estado
Vamos a crear una monitorización de estado activo o "uptime".

Para ello, sigue los siguientes pasos:
1. En **Monitorización**, pulsa **Comprobaciones de estado**
1. Pulsa en **Crear comprobación de estado**
1. Crea una comprobación de estado de protocolo HTTP para tu grupo de instancais con una frecuencia de 1 minuto
1. Selecciona tu email como canal de notificación
1. Testea la monitorización de estado

*ENTREGABLE: M2U7-1-tarea_1-archivo_2-captura_2.jgp: Captura de pantalla mostrando el resultado de la comprobación de la monitorización de estado.*

## Resumen de entregas
1. M2U7-1-tarea_1-archivo_1-captura_1.jgp: Captura de pantalla mostrando el dashboard personalizado.
1. M2U7-1-tarea_1-archivo_2-captura_2.jgp: Captura de pantalla mostrando el resultado de la comprobación de la monitorización de estado.

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. Elimina la monitorización de estado
1. Elimina la política de alerta
1. Elimina el clúster de instancias de VM creado, junto con la plantilla de instancia, la imagen de disco y la instancia de VM original
