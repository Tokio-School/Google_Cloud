# Cuentas de servicio
Unidad M2U5 - Ejercicio 1

## ¿Qué vamos a hacer?
1. Utilizar las cuentas de servicio por defecto de Google Cloud.
1. Crear cuentas de servicio personalizadas, administrar sus credenciales y utilizarlas de forma manual y en aplicaciones.
1. Asignar roles a las cuentas de servicios y permisos para usarlas a otras identidades.

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

Para responder a todas las preguntas del ejercicio de forma agrupada, puedes crear antes de empezar un archivo a entregar llamado "M2U0-0-preguntas.txt". Recuerda identificar y ordenar cada pregunta seguida de su respuesta.

Encontrarás las preguntas entre el texto en cursiva: *PREGUNTA: ¿Cómo se llama el servicio de instancias de VMs de Google Cloud?*

### Tarea 1: Cuentas de servicio por defecto

#### Comprobar cuentas de servicio por defecto
- comprobar SA creadas

#### Crear bucket de Cloud Storage
- Crear bucket de GCS - nombre con ID proyecto + m2u5-1

#### Cuenta de servicio por defecto de GCE y sus scopes
- crear VM instance con default SA y scope para GCS
- subir objeto a GCS - leer objeto

### Tarea 2: Cuentas de servicio personalizadas

#### Crear cuentas de servicio
- crear SA - comprobar roles y permisos, no asignar

#### Asignación de roles y acceso a cuenta de servicio
- asignarse SAuser a usuario
- asignarle a SA rol object storage owner

### Tarea 3: Asignación de cuentas de servicio

#### Asignación a instancias de VM
- asignar custom SA a nueva VM
- subir y listar objetos, comprobar acceso

#### Asignación a aplicaciones
- apagar VM y quitar SA
- levantar VM, asignar SA en envvar
- subir y listar objetos, comprobar acceso

#### Impersonar cuentas de servicio
- quitar SA de envvar
- impersonar SA con gcloud
- subir y listar objetos con gsutil, comprobar acceso

## Resumen de entregas
1. M2U0-0-preguntas.txt: Respuestas a todas las preguntas planteadas en el ejercicio.
1. [nombre de archivo]: descripción

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. Eliminar el bucket de Cloud Storage creado.
1. Eliminar las instancias de VM creadas.
1. Eliminar la cuenta de servicio creada.
