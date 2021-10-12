# BigQuery
Unidad M2U3 - Ejercicio 8

## ¿Qué vamos a hacer?
1. Gestionar datasets y tablas.
1. Consultar datos públicos.
1. Administrar datos con la herramienta de línea de comandos `bq`.
1. Consultar datos desde las librerías de cliente.
1. Trabajar con tablas particionadas.
1. Visualizar datos con Data Studio.

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

Para responder a todas las preguntas del ejercicio de forma agrupada, puedes crear antes de empezar un archivo a entregar llamado "M2U3-8-preguntas.txt". Recuerda identificar y ordenar cada pregunta seguida de su respuesta.

Encontrarás las preguntas entre el texto en cursiva: *PREGUNTA: ¿Cómo se llama el servicio de instancias de VMs de Google Cloud?*

### Tarea 1: Buscar en los datasets públicos
En esta tarea vamos a comenzar por lanzar búsquedas de SQL en los datasets públicos disponibles en BigQuery.

Primero habilita la API de BigQuery.

Explora los datasets públicos disponibles:
1. En la consola, navega a **BigQuery**.
1. Explora las opciones disponibles en la consola de BigQuery.
1. En el **Explorador**, localiza los proyectos públicos `bigquery-public-data` y `bigquery-samples`.
    1. Si no los encuentras, pulsa los 3 puntos verticals del menú desplegable del **Explorador**, pulsa en **+ Agregar datos > Fijar un proyecto > Ingresar nombre del proyecto**.
1. Analiza los datasets disponibles en ambos proyectos y algunas de sus tablas. Pulsando en la tabla, podrás ver información al respecto.

Ejecuta 2 búsquedas sobre datasets públicos:
1. Mensajes de commit más utilizados en GitHub:
```SQL
SELECT subject AS subject,
  COUNT(*) AS num_duplicates
FROM `bigquery-public-data.github_repos.sample_commits`
GROUP BY subject
ORDER BY num_duplicates DESC
LIMIT 100
```
1. Proyectos más populares en Libraries.io deprecados o no mantenidos aún utilizados como dependencias en otros proyectos:
```SQL
SELECT
  name,
  dependent_projects_count,
  language,
  status
FROM
  `bigquery-public-data.libraries_io.projects_with_repository_fields`
WHERE status IN ('Deprecated', 'Unmaintained')
ORDER BY dependent_projects_count DESC
LIMIT 100
```
1. Visitas a Wikipedia a páginas que incluyen "Google" en su título:
```SQL
SELECT
  LANGUAGE,
  SUM(views) AS views
FROM
  `bigquery-samples.wikipedia_benchmark.Wiki1k`
WHERE
  REGEXP_CONTAINS(title, "Google")
GROUP BY
  LANGUAGE
ORDER BY
  views DESC
```
1. Repite la query anterior sobre la tabla `Wiki1B`:
    1. *PREGUNTAS: ¿Cuántos datos procesaría dicha query? ¿Cuánto tiempo tarda? ¿Cuántos datos procesaría dicha query sobre las tablas Wiki10B y Wiki100B (sin ejecutarlas)?*

De esta forma podemos ejecutar búsquedas de SQL sobre datasets públicos de BigQuery.

*ENTREGABLES:*
1. M2U3-8-tarea_1-archivo_1-captura_1.jpg: Captura de pantalla mostrando la búsqueda y resultados de la primera búsqueda SQL.
1. M2U3-8-tarea_1-archivo_2-captura_2.jpg: Captura de pantalla mostrando la búsqueda y resultados de la segunda búsqueda SQL.

### Tarea 2: Subir y consultar datos utilizando la consola
Para esta tarea vamos a utilizar la consola web de BigQuery para crear un dataset y tabla, subir datos y realizar consultas SQL a los mismos.

#### Crear un dataset
Vamos a comenzar por crear un dataset para organizar los datos:
1. En la consola web de **BigQuery**, localiza tu proyecto en el **Explorador** y pulsa sobre él.
1. Pulsa sobre el menú desplegable de 3 puntos verticales y sobre **Crear un conjunto de datos**.
1. Explora las opciones y crea un dataset con las siguientes características:
    - ID de dataset: `babynames`.
    - Localización: europe-west1.

#### Cargar los datos
Vamos a utilizar un dataset público con los nombres más populares de bebés en EE.UU de unos 7 MB en formato CSV. Puedes encontrar más información del mismo [aquí](http://www.ssa.gov/OACT/babynames/background.html).

1. Descarga el dataset en formato ZIP: https://www.ssa.gov/OACT/babynames/names.zip
1. Consulta el esquema en el documento `NationalReadMe.pdf`.
1. Consulta alguno de los archivos para explorar la información contenida.

Ahora vamos a cargar los datos en una tabla de BigQuery:
1. En el **Explorador**, pulsa sobre el dataset creado anteriormente.
1. Pulsa sobre el menú desplegable de 3 puntos verticales y sobre **Abrir**.
1. En la zona de la derecha, pulsa sobre **Crear tabla**.
1. Explora las opciones y crea una tabla subiendo los datos con las siguientes características:
    - Fuente > Crear tabla desde > Subir > Seleccionar archivo: `yob2014.txt` > Formato del archivo: CSV.
    - Destino: dataset `babynames`, tabla `names_2014`.
    - Esquema: editar como texto: `name:string,gender:string,count:integer`

Espera a que BigQuery finalice la creación de la tabla y la subida de datos en el panel de **Historial de trabajos**.

Una vez subidos los datos, consulta la tabla:
1. En el **Explorador**, localiza el dataset y pulsa sobre la tabla.
1. En el panel de detalles, pulsa sobre **Vista previa** y previsualiza los datos.
1. En el espacio de trabajo, pulsa sobre **Redactar consulta nueva** y lanza la siguiente búsqueda SQL:
```SQL
SELECT
  name,
  count
FROM
  `babynames.names_2014`
WHERE
  gender = 'M'
ORDER BY
  count DESC
LIMIT
  5
```

De esta forma podemos subir y realizar búsquedas sobre datos utilizando la consola web de BigQuery.

*ENTREGAS:* M2U3-8-tarea_2-archivo_1-captura_1.jpg: Captura de pantalla mostrando el resultado de la búsqueda SQL sobre los datos subidos.

### Tarea 3: Subir y consultar datos utilizando la línea de comandos
En esta tarea vamos a crear datasets, tablas y subir y consultar datos utilizando la línea de comandos `bq`.

#### Crea un dataset
Utiliza Cloud Shell o tu instalación de Cloud SDK local:
1. Crea un dataset llamado `bq_load_test`: `bq mk bq_load_test`.
1. Muestra sus propiedades: `bq show bq_load_test`.

#### Crear los datos a subir
Crea un archivo CSV de ejemplo:
1. En Cloud Shell: `touch customer_transactions.csv` y `cloudshell edit customer_transactions.csv`.
1. En local: utiliza cualquier editor de texto para crear un archivo llamado `customer_transactions.csv`.
1. Añade los siguientes valores:
```
ID,Zipcode,Timestamp,Amount,Feedback,SKU
c123,78757,2018-02-14 17:01:39Z,1.20,4.7,he4rt5
c456,10012,2018-03-14 15:09:26Z,53.60,3.1,ppiieee
c123,78741,2018-04-01 05:59:47Z,5.98,2.0,ch0c0
```

#### Crear una tabla y subir los datos
Carga los datos en una tabla nueva con el comando:
```
bq load \
    --source_format=CSV \
    --skip_leading_rows=1 \
    bq_load_test.customer_transactions \
    ./customer_transactions.csv \
    id:string,zip:string,ttime:timestamp,amount:numeric,fdbk:float,sku:string
```

Una vez creada, consulta sus propiedades: `bq show bq_load_test.customer_transactions`.

#### Consultar los datos
Ahora vamos a realizar una búsqueda conjunta aunando los datos subidos con los datos de códigos postales de los estados de EE.UU.:
```
bq query --nouse_legacy_sql '
SELECT SUM(c.amount) AS amount_total, z.state_code AS state_code
FROM `bq_load_codelab.customer_transactions` c
JOIN `bigquery-public-data.utility_us.zipcode_area` z
ON c.zip = z.zipcode
GROUP BY state_code
'
```

*Nota:* Si no puedes ejecutar la consulta porque el dataset creado no está en la localización multi-regional de US, crea otro dataset en dicha localización y sube los datos utilizando la consola web.

De esta forma podemos gestionar datasets y tablas y subir y consultar datos utilizando la línea de comandos `bq`.

*ENTREGABLES:* M2U3-8-tarea_3-archivo_1-captura_1.jpg: Captura de pantalla mostrando el resultado del comando con la búsqueda de SQL.

### Tarea 4: Gestionar datos con librerías de cliente
En esta tarea gestionaremos datos con las librerías de cliente, utilizando un código de Python en este caso.

Para esta tarea, encontrarás las instrucciones y código en uno de los codelabs públicos: [Using BigQuery with Python](https://codelabs.developers.google.com/codelabs/cloud-bigquery-python).

Recuerda que no necesitas crear un nuevo proyecto ni habilitar la API, por lo que puedes comenzar directamente en el paso 4 del mismo.

*ENTREGABLES:* M2U3-8-tarea_4-archivo_1-captura_1.jpg: Captura de pantalla mostrando en la consola web la vista previa con los datos subidos a la tabla.

### Tarea 5: Trabajar con tablas particionadas y agrupadas
En esta tarea vamos a comprobar las diferencias entre varias búsquedas, utilizando tablas sin particionar, particionadas y agrupadas.

Primero, utilizando la consola o la línea de comandos, crea un dataset llamado `stackoverflow` en la multi-región US.

#### Crear una tabla con los posts de 2018 de StackOverflow
Vamos a comenzar por crear una tabla nueva con parte de la información del dataset público de StackOverflow.

En la consola, crea una nueva búsqueda y ejecuta esta consulta de SQL:
```SQL
CREATE OR REPLACE TABLE `stackoverflow.questions_2018` AS
SELECT id, title, accepted_answer_id, creation_date, answer_count , comment_count , favorite_count, view_count, tags
FROM `bigquery-public-data.stackoverflow.posts_questions`
WHERE creation_date BETWEEN '2018-01-01' AND '2019-01-01';
```

Espera a que termine el trabajo y consulta los datos cargados en la nueva tabla.

Esta búsqueda devuelve las preguntas creadas en enero de 2018 con la etiqueta de `android`:
```SQL
SELECT id, title, accepted_answer_id, creation_date, answer_count , comment_count , favorite_count, view_count 
FROM  `stackoverflow.questions_2018` 
WHERE creation_date BETWEEN '2018-01-01' AND '2018-02-01'
AND tags = 'android';
```

Ahora, en **Más > Configuración de consulta** y deshabilita los resultados cacheados. Usaremos esta configuración el resto de la tarea para comprobar las diferencias entre las distintas búsquedas.

Ahora reejecuta la última búsqueda y asegúrate de que muestra resultados sin cachear
*PREGUNTA: ¿Cuánto ha tardado y cuántos bytes ha procesado la primera búsqueda?*

#### Utilizar una tabla particionada
Ahora repite los pasos anteriores para crear otra tabla, en esta ocasión particionada:
```SQL
CREATE OR REPLACE TABLE `stackoverflow.questions_2018_partitioned` 
PARTITION BY DATE(creation_date) AS
SELECT id, title, accepted_answer_id, creation_date, answer_count , comment_count , favorite_count, view_count, tags
FROM `bigquery-public-data.stackoverflow.posts_questions`
WHERE creation_date BETWEEN '2018-01-01' AND '2019-01-01';
```

Ejecuta la búsqueda anterior sobre la tabla particionada y compara su rendimiento:
```SQL
SELECT id, title, accepted_answer_id, creation_date, answer_count , comment_count , favorite_count, view_count 
FROM  `stackoverflow.questions_2018_partitioned` 
WHERE creation_date BETWEEN '2018-01-01' AND '2018-02-01'
AND tags = 'android';
```

*PREGUNTA: ¿Cuánto ha tardado y cuántos bytes ha procesado la búsqueda sobre la tabla particionada? ¿Qué mejora de rendimiento hemos alcanzado en porcentaje?*

#### Utilizar una tabla agrupada y particionada
Por último, comprobemos la mejora al agrupar las columnas y ordenarlas con una nueva tabla:
```SQL
CREATE OR REPLACE TABLE `stackoverflow.questions_2018_clustered`
PARTITION BY
  DATE(creation_date)
CLUSTER BY
  tags AS
SELECT
  id, title, accepted_answer_id, creation_date, answer_count , comment_count , favorite_count, view_count, tags
FROM
  `bigquery-public-data.stackoverflow.posts_questions`
WHERE
  creation_date BETWEEN '2018-01-01' AND '2019-01-01';
```

Ejecuta la búsqueda anterior sobre la tabla particionada y agrupada y compara su rendimiento:
```SQL
SELECT id, title, accepted_answer_id, creation_date, answer_count , comment_count , favorite_count, view_count 
FROM  `stackoverflow.questions_2018_clustered` 
WHERE creation_date BETWEEN '2018-01-01' AND '2018-02-01'
AND tags = 'android';
```

*PREGUNTA: ¿Cuánto ha tardado y cuántos bytes ha procesado la búsqueda sobre la tabla particionada y agrupada? ¿Qué mejora de rendimiento hemos alcanzado en porcentaje frente a la búsqueda sobre la tabla particionada?*

### Tarea 6: Visualizar datos con Data Studio
Data Studio es un servicio de visualización y reportes de datos, que podemos conectar a las BBDDs de Google Cloud, a Google Drive, a los servicios de márketing de Google y a una gran multitud de otros orígenes, que es habitualmente utilizado para visualizar datos almacenados en BigQuery.

Para ello, sigue las instrucciones del siguiente codelab: [Visualizing your BigQuery Data in Data Studio](https://codelabs.developers.google.com/codelabs/bigquery-data-studio).

*ENTREGABLES:* M2U3-8-tarea_6-archivo_1-captura_1.jpg: Captura de pantalla del dashboard final.

## Resumen de entregas
1. M2U3-8-preguntas.txt: Respuestas a todas las preguntas planteadas en el ejercicio.
1. M2U3-8-tarea_1-archivo_1-captura_1.jpg: Captura de pantalla mostrando la búsqueda y resultados de la primera búsqueda SQL.
1. M2U3-8-tarea_1-archivo_2-captura_2.jpg: Captura de pantalla mostrando la búsqueda y resultados de la segunda búsqueda SQL.
1. M2U3-8-tarea_2-archivo_1-captura_1.jpg: Captura de pantalla mostrando el resultado de la búsqueda SQL sobre los datos subidos.
1. M2U3-8-tarea_3-archivo_1-captura_1.jpg: Captura de pantalla mostrando el resultado del comando con la búsqueda de SQL.
1. M2U3-8-tarea_4-archivo_1-captura_1.jpg: Captura de pantalla mostrando en la consola web la vista previa con los datos subidos a la tabla.
1. M2U3-8-tarea_6-archivo_1-captura_1.jpg: Captura de pantalla del dashboard final.

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. Elimina los datasets creados en BigQuery con la consola.
