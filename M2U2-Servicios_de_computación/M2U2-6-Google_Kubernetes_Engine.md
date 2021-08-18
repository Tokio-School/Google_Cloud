# Google Kubernetes Engine
Unidad M2U2 - Ejercicio 6

## ¿Qué vamos a hacer?
1. Crear un clúster de Kubernetes con Google Kubernetes Engine.
1. Desplegar una aplicación sobre el clúster de GKE.
1. Comprobar cómo podemos desplegar recursos de Google Cloud como objetos de Kubernetes.
1. Comprobar cómo escala el clúster de GKE en función de la carga.
1. Crear un clúster de GKE en modo autopilot y comprobar las diferencias.

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

Para responder a todas las preguntas del ejercicio de forma agrupada, puedes crear antes de empezar un archivo a entregar llamado "M2U2-6-preguntas.txt". Recuerda identificar y ordenar cada pregunta seguida de su respuesta.

Encontrarás las preguntas entre el texto en cursiva: *PREGUNTA: ¿Cómo se llama el servicio de instancias de VMs de Google Cloud?*

### Tarea 1: Crear un clúster de Kubernetes y desplegar una aplicación web
xxx

#### Crear un clúster de GKE
- modo estándar
- políticas de autoescalado

#### Desplegar una aplicación web
- crear manifiesto deployment
- desplegar deployment
- comprobar pods y deployment
- desplegar servicio/lb
- crear fr?
- comprobar acceso a app
- comprobar lb desplegado

#### Comprobar el autoescalado
- comprobar nº de pods y nodos
- sobrecargar clúster
- comprobar app
- comprobar nº de pods y nodos

#### Desplegar una aplicación stateful
- crear manifiesto statefulset
- comprobar storageclass
- desplegar statefulset con PVC
- comprobar cómo se ha creado el disco automáticamente

- comprobar que nº de pods y nodos ha escalado hacia abajo

### Tarea 2: Crear un clúster autopilot de GKE
- crear un clúster
- comprobar diferencias
- cambiar credenciales clúster de kubectl
- desplegar deployment
- desplegar servicio
- comprobar app
- sobrecargar pods
- comprobar nº de pods

## Resumen de entregas
1. M2U2-6-preguntas.txt: Respuestas a todas las preguntas planteadas en el ejercicio.
1. [nombre de archivo]: descripción

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. paso1
1. paso2
