# Cloud Spanner
Unidad M2U3 - Ejercicio 4

## ¿Qué vamos a hacer?
1. Crear una instancia y base de datos en Cloud Spanner.
1. Gestionar tablas y esquemas.
1. Administrar y consultar datos en la base de datos.

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

*Nota: no hay preguntas para este ejercicio.*

### Tarea 1: Crear una instancia de Cloud Spanner
En esta tarea vamos a empezar creando una instancia de Cloud Spanner para trabajar con bases de datos en la misma en posteriores puntos.

Para ello, comienza habilitando la API de Cloud Spanner en **APIs y Servicios > Biblioteca".

#### Crear la instancia de Cloud Spanner
Vamos a crear una instancia de Cloud Spanner utilizando la consola web:

1. En la consola, navega a **Spanner > Crear instancia**.
1. Explora todas las opciones y crea una instancia con las siguientes características:
    - Nombre e ID de instancia: `test-instance`, lo referenciaremos como `ID_INSTANCIA`.
    - Configuración: regional en europe-west1.
    - Capacidad de procesamiento: 1 nodo o 1000 unidades de procesamiento.

Ahora simplemente espera que la instancia de Cloud Spanner esté lista para utilizar.

#### Crear la base de datos
Una vez creada la instancia, crearemos una base de datos en la misma:

1. En la página de **Cloud Spanner**, pulsa sobre el nombre de la instancia creada.
1. Pulsa sobre **Crear base de datos**.
1. Crea una base de datos con el nombre `example-db`, sin incluir un esquema en este punto.

De esta forma podemos crear una base de datos en Cloud SQL.

#### Crear una tabla
Una vez creada una base de datos, vamos a crear una tabla y su esquema en la misma:

1. En la página de **Vista general** de la base de datos `example-db`, pulsa en **Crear tabla**.
1. En el recuadro de **Escribir sentencias DDL**, introduce la siguiente sentencia `CREATE TABLE` de SQL:
```SQL
CREATE TABLE Singers (
  SingerId   INT64 NOT NULL,
  FirstName  STRING(1024),
  LastName   STRING(1024),
  SingerInfo BYTES(MAX),
  BirthDate  DATE,
) PRIMARY KEY(SingerId);
```

Una vez actualizado el esquema, se mostrará la tabla en la consola.

Ahora vamos a actualizar la base de datos creando la tabla `Albums` dependiente de la tabla `Singers` utilizando la línea de comandos:
1. En Cloud Shell, actualiza la base de datos: `gcloud spanner databases ddl update example-db --ddl='CREATE TABLE Albums ( SingerId INT64 NOT NULL, AlbumId INT64 NOT NULL, AlbumTitle STRING(MAX)) PRIMARY KEY (SingerId, AlbumId), INTERLEAVE IN PARENT Singers ON DELETE CASCADE'`.

De esta forma podemos crear tablas y actualizar sus esquemas con la consola y Cloud SDK.

*ENTREGABLE:* M2U3-4-tarea_1-archivo_1-captura_1.jpg: Captura de pantalla mostrando las tablas en la base de datos.

### Tarea 2: Administrar datos en Cloud Spanner
En esta tarea vamos a administrar datos utilizando la consola, la línea de comandos y librerías de cliente.

#### Utilizando la consola web

Vamos a insertar unas cuantas filas en la tabla:
1. En la página de la BBDD `example-db`, pulsa sobre la tabla `Singers`.
1. En el menú lateral, pulsa sobre **Datos** y luego sobre **Insertar**.
1. Utilizando la plantilla de SQL, sustituye los valores en la misma e introduce 3 filas con `SingerID` de 1, 2 y 3.
1. Finalmente, en la página de la tabla, comprueba los registros insertados.

Ahora vamos a editar una de las filas:
1. En la página de la tabla, selecciona alguna de las filas y pulsa **Editar**.
1. Utilizando la plantilla de SQL de nuevo, modifica algunos valores y actualiza dicha fila.
1. Finalmente, en la página de la tabla, comprueba el registro actualizado.

Vamos a eliminar alguna de las filas:
1. En la página de la tabla, selecciona alguna de las filas y pulsa **Eliminar**.

Por último, vamos a realizar una búsqueda SQL:
1. En la página de la BBDDs, pulsa sobre **Búsqueda** en el menú lateral para mostrar la sección de búsquedas.
1. Pulsa en **Nueva pestaña**.
1. Ejecuta la siguiente búsqueda SQL: `SELECT * FROM Singers;`

*ENTREGABLE:* M2U3-4-tarea_2-archivo_1-captura_1.jpg: Captura de pantalla mostrando el resultado de la búsqueda SQL.

#### Utilizando Cloud SDK
Vamos a utilizar la línea de comandos de Cloud SDK local o Cloud Shell para escribir datos:
1. Inserta una nueva fila, sustituyendo los valores correspondientes para `SingerID` (diferentes a los existentes, 1, 2, y 3) y el resto: `gcloud spanner rows insert --database=example-db --table=Singers --data=SingerId=ID_FILA,FirstName=NOMBRE,LastName=APELLIDO`.
1. Inserta al menos 3 filas.
1. Ahora inserta también al menos 3 filas en la tabla `Albums`, relacionada con las filas de `Singers` que acabas de añadir: `gcloud spanner rows insert --database=example-db --table=Albums --data=SingerId=ID_FILA_SINGERS,AlbumId=ID_FILA_ALBUMS,AlbumTitle="NOMBRE_ALBUM_ENTRE_COMILLAS"`

Ahora vamos a lanzar una query SQL desde Cloud SDK:
1. Selecciona todos los álbumes: `gcloud spanner databases execute-sql example-db --sql='SELECT SingerId, AlbumId, AlbumTitle FROM Albums'`

Por último, vamos a actualizar datos. Para ello, vuelve a la consola web para encontrar la plantilla de sentencia `UPDATE` de SQL para actualizar datos y utiliza el comando anterior de Cloud SDK para lanzar una sentencia SQL que actualice alguna de las filas que has insertado en cualquiera de las tablas.

*ENTREGABLE:* M2U3-4-tarea_2-archivo_2-captura_2.jpg: Captura de pantalla mostrando el resultado de la última sentencia SQL.

#### Utilizando la librería de cliente para Python
Por último vamos a realizar varias acciones de administración de datos utilizando ejemplos con la librería de cliente de Python.

Para ello, primero inicia un entorno de trabajo en Cloud Shell o tu instalación de Cloud SDK local:
1. Clona el repositorio de ejemplo: `git clone https://github.com/googleapis/python-spanner`.
1. Accede al directorio de trabajo: `cd python-spanner/samples/samples`.
1. Crea un entorno virtual aislado de Python e instala las dependencias:
```Python
virtualenv env
source env/bin/activate
pip install -r requirements.txt
```

Revisa el código a ejecutar está disponible en GitHub: [snippets.py](https://github.com/googleapis/python-spanner/blob/HEAD/samples/samples/snippets.py).

Vamos a ejecutar algunos snippets de funciones del archivo `snippets.py` con el comando `python snippets.py test-instance --database-id example-db NOMBRE_FUNCION`.

A continuación tienes un listado de funciones a ejecutar con un enlace de referencia en la documentación y el nombre de la función:
1. [Escribir datos con DML](https://cloud.google.com/spanner/docs/getting-started/python#write-data): insert_with_dml
1. [Escribir datos con mutaciones](https://cloud.google.com/spanner/docs/getting-started/python#write-data-with-mutations): insert_data
1. [Búsquedas SQL](https://cloud.google.com/spanner/docs/getting-started/python#using_the_client_library_for): query_data
1. [Búsquedas SQL con parámetros](https://cloud.google.com/spanner/docs/getting-started/python#query_using_a_sql_parameter): query_data_with_parameter
1. [Leer datos con la API de lectura](https://cloud.google.com/spanner/docs/getting-started/python#read_data_using_the_read_api): read_data
1. [Actualizar datos](https://cloud.google.com/spanner/docs/getting-started/python#update-data): write_with_dml_transaction
1. [Búsquedas con transacciones de sólo lectura](https://cloud.google.com/spanner/docs/getting-started/python#retrieve_data_using_read-only_transactions): read_only_transaction

*ENTREGABLES:* M2U3-4-tarea_2-archivo_3-captura_3.jpg al M2U3-4-tarea_2-archivo_9-captura_9.jpg: Capturas de pantalla del resultado de los snippets de código de cada función.

## Resumen de entregas
1. M2U3-4-tarea_1-archivo_1-captura_1.jpg: Captura de pantalla mostrando las tablas en la base de datos.
1. M2U3-4-tarea_2-archivo_1-captura_1.jpg: Captura de pantalla mostrando el resultado de la búsqueda SQL.
1. M2U3-4-tarea_2-archivo_2-captura_2.jpg: Captura de pantalla mostrando el resultado de la última sentencia SQL.
1. M2U3-4-tarea_2-archivo_3-captura_3.jpg al M2U3-4-tarea_2-archivo_9-captura_9.jpg: Capturas de pantalla del resultado de los snippets de código de cada función.

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. Elimina la base de datos e instancia de Cloud Spanner en la consola.
