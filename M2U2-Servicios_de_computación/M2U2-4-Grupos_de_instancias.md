# Grupos de instancias
Unidad M2U2 - Ejercicio 4

## ¿Qué vamos a hacer?
1. Crear un grupo de instancias no administrados.
1. Trabajar con plantillas de instancias para los grupos de instancias administrados.
1. Crear un grupo de instancias administrado para aplicaciones serverless.
1. Crear un grupo de instancias administrado para aplicaciones stateful.
1. Actualizar aplicaciones en los diferentes grupos de instancias.

Para responder a todas las preguntas del ejercicio de forma agrupada, puedes crear antes de empezar un archivo a entregar llamado "M2U2-4-preguntas.txt". Recuerda identificar y ordenar cada pregunta seguida de su respuesta.

Encontrarás las preguntas entre el texto en cursiva: *PREGUNTA: ¿Cómo se llama el servicio de instancias de VMs de Google Cloud?*

### Tarea 1: Grupos de instancias no administrados
xxx

#### Crear la imagen de disco base
- Crear la instancia
- instalar servidor web, ver si script lo personaliza
- sacar imagen de disco y crear familia de imágenes, con tag v1
- eliminar instancia

#### Crear un grupo de instancias no administrado
- crear 3 instancias heterogéneas en tamaño a partir de la imagen
- crear FW
- crear health check?
- crear LB
- comprobar acceso a la app

### Tarea 2: Grupos de instancias administrados
xxx

#### Crear una plantilla de instancia
- crear a partir de la imagen
- aplicar fw

#### Crear un grupo de instancias administrado serverless
- Crear el grupo a partir de la plantilla
- Crear LB
- comprobar webapp

#### Crear un grupo de instancias administrado stateful
- Crear el grupo a partir de la plantilla
- Crear LB
- comprobar webapp
- modificar estado en las instancias
- comprobar webapp

### Tarea 3: Actualización de aplicaciones en grupos de instancias
xxx

#### Actualizar la imagen de disco base
- crear instancia
- personalizarla con nueva versión
- sacar nueva imagen de disco
- eliminar imagen

#### Actualizar la plantilla de instancia
- crear nueva plantilla de instancia con nueva imagen

#### Actualizar el grupo de instancias no administrado
- crear nuevas instancias
- añadir instancias al grupo
- comprobar webapp
- desvincular viejas instancias del grupo y eliminarlas
- comprobar webapp
- explicar que se puede hacer como mismo servicio de backend de LB u otro

#### Actualizar el grupo de instancias administrado serverless
- revisar política
- actualizar
- comprobar webapp

#### Actualizar el grupo de instancias administrado stateful
- revisar política
- actualizar
- comprobar webapp

## Resumen de entregas
1. M2U2-4-preguntas.txt: Respuestas a todas las preguntas planteadas en el ejercicio.
1. [nombre de archivo]: descripción

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. paso1
1. paso2
