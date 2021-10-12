# Bigtable
Unidad M2U3 - Ejercicio 7

## ¿Qué vamos a hacer?
1. Crear una instancia de Cloud Bigtable.
1. Gestionar datos con la herramienta cbt.
1. Consultar datos desde una aplicación.

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

*NOTA: No hay preguntas para este ejercicio.*

### Tarea 1: Crear una instancia de Cloud Bigtable
Vamos a crear una instancia de Cloud Bigtable para utilizar en el ejercicio.

Primero habilita las APIs de Bigtable y Bigtable Admin en **APIs y Servicios > Biblioteca**.

Luego crea una instancia de Bigtable:
1. En la consola, navega a **Bigtable** y pulsa **Crear instancia**.
1. Explora todas las opciones y crea una instancia con estas características:
    - Nombre e ID de instancia: `test-instance`.
    - Tipo de almacenamiento: HDD.
    - ID de clúster: `test-intance-c1`.
    - Región y zona: europe-west1-b.

De esta forma podemos crear una instancia de Cloud Bigtable con un clúster zonal.

### Tarea 2: Gestionar datos con cbt
En esta tarea vamos a conectarnos y gestionar datos con la herramienta `cbt` utilizando Cloud Shell o tu instalación de Cloud SDK local.

*Nota:* Si en tu instalación de Cloud SDK local no tienes instalado el componente de `cbt`, instálalo siguiendo las [instrucciones](https://cloud.google.com/sdk/docs/components#managing_components).

#### Conectarse a la instancia
1. Configura `cbt` para utilizar tu proyecto e instancia: `echo "project = ID_PROYECTO" > ~/.cbtrc` y `echo "instance = test-instance" >> ~/.cbtrc`.
1. Verific tu configuración: `cat ~/.cbtrc`.

#### Crear una tabla
1. Crea una tabla `my-table`: `cbt createtable my-table`.
1. Lista tus tablas: `cbt ls`.
1. Crea una familia de columnas `cf1`: `cbt createfamily my-table cf1`.
1. Lista tus familias de columnas: `cbt ls my-table`.

#### Crear y leer filas
1. Crea una fila con la clave `r1` y el valor `test-value` para la columna `c1` de la familia de columnas `cf1`: `cbt set my-table r1 cf1:c1=test-value`.
1. Lista todas las filas de la tabla: `cbt read my-table`.
1. Inserta 3 nuevas filas con las claves `r2, r3, r4`.
1. Lista todas las filas: `cbt read my-table`.

Por último, elimina la tabla e instancia creadas: `cbt deletetable my-table` y `cbt deleteinstance test-instance`.

*ENTREGABLES:* M2U3-7-tarea_2-archivo_1-captura_1.jpg: Captura de pantalla mostrando el restulado del último comando para listar todas las filas.

### Tarea 3: Consultar datos desde una aplicación
Como última tarea, vamos a ejecutar un codelab con una aplicación de ejemplo.

La aplicación creará una instancia y tabla en Bigtable, importará un gran dataset desde Cloud Storage utilizando Cloud Dataflow como pipeline de datos y ejecutará queries para visualizar datos sobre la posición de autobuses en Manhattan.

Para ello, sigue los pasos del codelab público: [Introduction to Cloud Bigtable](https://codelabs.developers.google.com/codelabs/cloud-bigtable-intro-java).

*ENTREGABLES:* M2U3-7-tarea_3-archivo_1-captura_1.jpg: Captura de pantalla mostrando el restulado del último comando del paso 10.

## Resumen de entregas
1. M2U3-7-tarea_2-archivo_1-captura_1.jpg: Captura de pantalla mostrando el restulado del último comando para listar todas las filas.

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. En la consola, elimina la instancia de Cloud Bigtable.
