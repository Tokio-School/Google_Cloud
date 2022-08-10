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
Vamos a comenzar por explorar las cuentas de servicio disponibles por defecto en Google Cloud. Estas cuentas se crean cuando se habilitan diferentes servicios o APIs, como los de Compute Engine, Kubernetes Engine, App Engine, Cloud Run, etc.

#### Comprobar cuentas de servicio por defecto
1. Para comprobar las cuentas de servicio creadas por defecto, navega a **IAM y administración > Cuentas de servicio**.
1. Pulsa sobre la cuenta de servicio por defecto de Compute Engine.
1. Explora todos los detalles de las diferentes pestañas: **Detalles, permisos, claves, métricas, registros**.
1. En la pestaña **Permisos**, comprueba qué roles/permisos tiene asignados por defecto dicha cuenta de servicio, a través del analizador de políticas de Cloud IAM.
    1. *PREGUNTA: ¿Qué rol tiene asignado la cuenta de servicio por defecto de Compute Engine?*

#### Crear bucket de Cloud Storage
Crea un bucket de Cloud Storage para utilizarlo en siguientes pasos:
- Nombre: `ID_PROYECTO-m2u5-1`.
- Control de acceso: Uniforme.

Por último, sube un objeto a Cloud Storage desde Cloud Shell o una instalación de Cloud SDK local:
1. `echo "hola mundo!" > sample_file.txt`.
1. `gsutil cp sample_file.txt gs://ID_PROYECTO-m2u5-1/`

#### Cuenta de servicio por defecto de GCE y sus scopes
Vamos a utilizar dicha cuenta de servicio por defecto para asignársela a una instancia y comprobar el uso de sus scopes:
1. Navega a **Compute Engine > Instancias de VM** y pulsa **Crear instancia**.
1. En **Identidad y acceso a la API**, comprueba que sólo tienes acceso a modificar los **Permisos de acceso** o *scopes* para la cuenta de servicio por defecto de Compute Engine.
1. *PREGUNTA: Escogiendo "Configurar acceso para cada API", ¿cuál es el acceso por defecto asignado para Google Storage? ¿Coincide con el acceso asignado por el rol que tiene asignada la cuenta de servicio?*
1. Crea una instancia con las siguientes características:
    - Tipo de máquina: e2-micro.
    - Cuenta de servicio: Cuenta de servicio por defecto de Compute Engine.
    - Permisos de acceso: Permitir el acceso predeterminado.
1. Por último, conéctate por SSH a la instancia y comprueba el acceso a Cloud Storage. *PREGUNTAS:*
    1. *¿Puedes descargar el objeto anteriormente subido?* `gsutil cp gs://ID_PROYECTO-m2u5-1/sample_file.txt .`
    1. *Repite los pasos anteriores para crear y subir un objeto, ¿puedes subirlo?*

De esta forma podemos utilizar la cuenta de servicio por defecto de Compute Engine, modificando sus permisos de acceso o *scopes* si fuera necesario.

*Nota:* Recordamos que utilizar esta cuenta de servicio en lugar de crear una personalizada para cada aplicación/instancia no es la práctica recomendada, pero para instancias temporales, auxiliares, etc., puede ser una alternativa.

### Tarea 2: Cuentas de servicio personalizadas
En esta tarea vamos a ver cómo crear cuentas de servicio personalizadas para cada script, aplicación o instancia de VM:

#### Crear cuentas de servicio
Para crear una cuenta de servicio personalizada:
1. Navega a **IAM y administración > Cuentas de servicio** y pulsa **Crear cuenta de servicio**.
1. El asistente tiene 3 pasos:
    1. Detalles de la cuenta de servicio, que la creal al pulsar **Crear y continuar**.
    1. Asignar roles/funciones a la cuenta de servicio (opcional).
    1. Asignar acceso a dicha cuenta de servicio con los roles/funciones de usuario o administrador de cuenta de servicio a otra identidad (opcional).
1. Explora las distintas opciones y crea una cuenta de servicio de nombre `sa-m2u5-1`, sin asignarle roles o asignarle acceso a dicha cuenta a otra identidad.

De esta forma podemos crear una cuenta de servicio y, opcionalmente, el asistente nos permite asignarle un rol y acceso a otras identidades en el momento de creación.

#### Asignación de roles 
Para asignar o modificar los roles/funciones asignados a una cuenta de servicio posteriormente, podemos hacerlo al igual que para cualquier otra identidad:
1. Navega a **IAM y administración > IAM**.
1. Busca la cuenta de servicio y pulsa en su botón de lápiz.
    1. *Nota:* Si no la encuentras, es porque no tiene ningún rol asignado aún. Vuelve a **IAM y administración > Cuentas de servicio** y copia su dirección de correo electrónico.
    1. Vuelve a **IAM y administración > IAM**, pulsa **Agregar** y añade la dirección de la cuenta de servicio como principal.
1. Asígnale el rol/función de `Cloud Storage > Visualizador de objetos de Storage`.

#### Acceso a cuenta de servicio
Las cuentas de servicio son a la vez identidades y recursos. Como recursos, también podemos administrarlas, asignarlas a instancias de VM, crear credenciales de acceso, etc. Dicho acceso se gestiona por parte de otras identidades, que para realizar dichas acciones necesitan el rol de usuario o administrador de dicha cuenta de servicio (rol a nivel de cuenta de servicio individual o de proyecto).

Para asignarle a un usuario el acceso a una cuenta de servicio, sigue estos pasos:
1. Navega a **IAM y administración > Cuentas de servicio**.
1. Selecciona la cuenta de servicio y pulsa **Administrar acceso**.
1. Comprueba las identidades que tienen acceso a la cuenta de servicio, agrupadas por rol que les proporciona dicho acceso, entre otros permisos.
    1. *Nota:* Debes encontrar tu cuenta de usuario bajo el rol de **Propietario**, ya que los propietarios de proyecto tienen asignados también permisos sobre todas las cuentas de servicio.
1. Para agregar acceso a un nuevo usuario, pulsa **Agregar principal** y asígnate el rol (también) de `Cuentas de servicio > Usuario de cuentas de servicio` para dicha cuenta de servicio.

De esta forma podemos crear cuentas de servicio, asignarle roles y acceso a la misma a otras identidades, tanto en el momento de creación de la cuenta de servicio como posteriormente.

### Tarea 3: Asignación de cuentas de servicio
En esta última tarea vamos a ver cómo utilizar una cuenta de servicio para nuestras operaciones y aplicaciones.

#### Asignación a instancias de VM
Para asignar una cuenta de servicio a una instancia de VM, permitiendo que toda operación utilizando Cloud SDK, librerías de cliente o las APIs la utilicen para autenticarse, sigue estos pasos:
1. Crea una instancia de VM y, en **Identidad y acceso a la API**, selecciona la cuenta de servicio creada anteriormente.
1. Una vez creada la instancia de VM, conéctate por SSH.
1. Comprueba si tienes acceso para descargar el archivo anteriormente subido a Cloud Storage: `gsutil cp gs://ID_PROYECTO-m2u5-1/sample_file.txt .`

#### Asignación a aplicaciones
Para asignar la cuenta de servicio a aplicaciones que utilizan las librerías de cliente de Google Cloud, podríamos seguir los mismos pasos que seguimos en uno de los primeros ejercicios, sobre **Librerías de cliente**. Al ser los mismos pasos, no los indicaremos de nuevo aquí.

Como resumen, los pasos serían:
1. Crear unas credenciales de la cuenta de servicio y descargarlas en la instancia de VM.
1. Incluirlas en la creación de los clientes de la librería de cliente, para cada aplicación y librería utilizada, o
1. Utilizar las "Application Default Credentials", simplemente añadiendo el path del archivo de credenciales a la variable de entorno `GOOGLE_APPLICATION_CREDENTIALS`.

#### Impersonar cuentas de servicio para Cloud SDK
Para usar las herramientas de CLI de Cloud SDK en la instancia, podemos impersonar la cuenta de servicio para que las acciones se realicen con esa autenticación en lugar de con nuestra cuenta de usuario.

Para ello:
1. Crea otra instancia de VM, esta vez sin ninguna cuenta de servicio activada.
1. Navega a **IAM y administración > Cuentas de servicio** y pulsa en la cuenta de servicio.
1. En la pestaña **Claves**, pulsa **Agregar clave > Crear clave nueva** y crea una clave/credencial de tipo JSON.
1. Copia el archivo de credenciales a la instancia de VM, p. ej. utilizando la opción de subir archivo de la ventana de navegador de la sesión SSH.
1. Activa la autenticación de Cloud SDK con dicha cuenta de servicio: `gcloud auth activate-service-account DIRECCION_EMAIL_CUENTA_SERVICIO --key-file=/PATH/A/ARCHIVO/CREDENCIALES/ARCHIVO.json --project=ID_PROYECTO`.
1. De nuevo, comprueba si tienes acceso para descargar el archivo anteriormente subido a Cloud Storage: `gsutil cp gs://ID_PROYECTO-m2u5-1/sample_file.txt .`

*ENTREGABLE:* M2U5-1-tarea_3-archivo_1-captura_1.jpg: Captura de pantalla mostrando el resultado del comando `gcloud auth activate-service-account ETC`.

De esta forma podemos usar una cuenta de servicio a una instancia de VM, tanto para cualquier operación en la instancia, para aplicaciones que utilicen las librerías de cliente o para operaciones utilizando el Cloud SDK desde dicha instancia.

## Resumen de entregas
1. M2U5-1-preguntas.txt: Respuestas a todas las preguntas planteadas en el ejercicio.
M2U5-1-tarea_3-archivo_1-captura_1.jpg: Captura de pantalla mostrando el resultado del comando `gcloud auth activate-service-account ETC`.

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. Eliminar el bucket de Cloud Storage creado.
1. Eliminar las instancias de VM creadas.
1. Eliminar la cuenta de servicio creada.
