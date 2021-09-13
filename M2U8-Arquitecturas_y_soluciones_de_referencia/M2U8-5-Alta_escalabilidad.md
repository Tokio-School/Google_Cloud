# Alta escalabilidad
Unidad M2U8 - Ejercicio 5

## ¿Qué vamos a hacer?
1. Desplegar una arquitectura basada en eventos serverless y de alta escalabilidad

Este ejercicio es un ejercicio de reto, donde tendrás que desplegar la arquitectura descrita sin instrucciones paso a paso asociadas. Para completarlo, te recomendamos basarte en lo visto en otros ejercicios, la documentación oficial o cualquier otro recurso online (tutoriales, StackOverflow, videos, etc.).

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

*Nota: no hay preguntas para este ejercicio.*

### Tarea única

Para este ejercicio nos vamos a basar en un workshop de acceso público mantenido por el equipo de devRel de Google Cloud:
- [Pic-a-Daily Serverless Workshop](g.co/codelabs/serverless-workshop)
- [Repositorio GitHub](https://github.com/GoogleCloudPlatform/serverless-photosharing-workshop)
- Licencia: [Apache 2.0](https://github.com/GoogleCloudPlatform/serverless-photosharing-workshop/blob/master/LICENSE)

El objetido del workshop es desplegar una arquitectura basada en eventos completa utilizando servicios serverless:
![Arquitectura basada en eventos](https://raw.githubusercontent.com/GoogleCloudPlatform/serverless-photosharing-workshop/master/pic-a-daily-architecture-events.png "Arquitectura basada en eventos")

En el último lab la convertiremos en una arquitectura basada en eventos orquestada en lugar de corografiada:
![Arquitectura basada en eventos orquestada](https://raw.githubusercontent.com/GoogleCloudPlatform/serverless-photosharing-workshop/master/pic-a-daily-architecture-workflows.png "Arquitectura basada en eventos orquestada")

Por favor, completa los 6 labs por orden en tu entorno/proyecto de GCP, siguiendo las instrucciones de cada uno paso a paso y completando los entregables indicados tras cada lab individualmente.

*Nota:* No limpies tu entorno con los pasos de "clean up" entre labs, excepto entre el lab 5º y 6º, donde debes eliminar todos los recursos creados utilizando los pasos de "clean-up" de los labs 1-5, ya que el 6º lab comienza desde un entorno vacío. También sigue los pasos de "clean up" del lab 6º tras acabarlo.

## Resumen de entregas
Capturas de pantalla de páginas de logs, resultados o aplicación web similares a las indicadas en cada último apartado de test de cada lab:
1. Lab 1 (paso 13): captura de pantalla de los logs de las invocaciones de la función de Cloud Functions y los datos en Cloud Firestore.
1. Lab 2 (paso 11): captura de pantalla de los logs de la app en Cloud Run.
1. Lab 3 (paso 10): captura de pantalla de los logs de la app en Cloud Run.
1. Lab 4 (paso 7): captura de pantalla de la aplicación de App Engine.
1. Lab 5 (paso 11): captura de pantalla de los logs de la app en Cloud Run.
1. Lab 6 (paso 15): captura de pantalla de la aplicación de App Engine.

*Nota:* Sigue la misma numeración propuesta hasta ahora: "M2U[nº de unidad]-[nº de ejercicio]-tarea_[nº de tarea]-archivo_[nº de archivo en la tarea]-[tipo de archivo: captura, exportado, script]_[nº de archivo si hay más de 1 del mismo tipo]", p. ej. "M2U8-1-tarea_1-archivo_1-captura_1.jpg".

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. Elimina todos los recursos creados, asegurándote de no dejar ninguno que pueda seguir incurriendo en costes o de luegar a un error en otro ejercicio.
