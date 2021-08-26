# Cloud Pub/Sub
Unidad M2U6

## ¿Qué vamos a hacer?
1. Crear una aplicación que envía y recibe mensajes.
1. Crear un esquema para un tema.
1. Utilizar suscripciones de tipo pull y push.
1. Desplegar arquitecturas de 1 a 1 y 1 a varios.
1. Explorar las opciones de filtrado y ordenado de mensajes.
1. Utilizar temas de "dead letter".

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

Para responder a todas las preguntas del ejercicio de forma agrupada, puedes crear antes de empezar un archivo a entregar llamado "M2U6-1-preguntas.txt". Recuerda identificar y ordenar cada pregunta seguida de su respuesta.

Encontrarás las preguntas entre el texto en cursiva: *PREGUNTA: ¿Cómo se llama el servicio de instancias de VMs de Google Cloud?*

### Tarea 1: Desplegar esquema "publisher"/"suscriber"
xxx

#### Crear un tema y suscripción pull
- crear un tema
- crear una suscripción pull al tema

#### Comprobar mensajes con la consola
- enviar mensaje con consola
- comprobar mensaje con consola
- confirmar mensaje con consola

#### Comprobar mensajes con Cloud SDK
- enviar mensaje con gcloud
- comprobar mensaje con gcloud
- confirmar mensaje con gcloud

#### Deplegar emisor y receptor
- desplegar emisor en CF
- desplegar receptor en CF
- enviar mensajes y comprobar recepción

### Tarea 2: Esquemas, filtrado y ordenado de mensajes y temas "dead letter"
xxx

#### Crear un tema con esquema
- crear esquema
- crear tema y asignarle esquema
- crear suscripción
- enviar mensaje erróneo
- comprobar que no va
- enviar mensaje correcto
- comprobar envío

#### Ordenado de mensajes
- crear tema con ordenado
- enviar mensaje - id 1
- confirmar recepción
- enviar mensaje - id 3
- confirmar recepción
- enviar mensaje desordenado - id 2
- confirmar recepción de id 2 e id 3

#### Filtrado de mensajes
- crear suscripción con filtrado (tema anterior)
- enviar mensaje normal - no supera filtro
- confirmar recepción en 1ª susc
- enviar mensaje supera filtro
- confirmar recepción en ambas susc

#### Temas "dead letter"
- añadir tema dead letter a tema
- configurar reintento rápido
- crear suscripción para tema dead letter
- desplegar que devuelve error directamente
- enviar mensaje
- confirmar recepción en suscripción dead letter con consola/gcloud

### Tarea 3: Suscripciones de tipo push
xxx

#### Enviar mensajes en paralelo
- crear tema
- crear 2 suscripciones push
- crear 2 CF
- enviar mensajes con gcloud
- confirmar mensajes en ambas CF

## Resumen de entregas
1. M2U6-1-preguntas.txt: Respuestas a todas las preguntas planteadas en el ejercicio.
1. [nombre de archivo]: descripción

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. paso1
1. paso2
