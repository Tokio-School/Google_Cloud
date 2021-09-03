# Análisis de costes
Unidad M2U1

## ¿Qué vamos a hacer?
1. Exportar de forma automática los costes de GCP a BigQuery.
1. Analizar los costes con SQL.
1. Visualizar los costes con Data Studio.

*NOTA: Para completar este ejercicio deberás exportar los costes a BigQuery y esperar al día siguiente para que la primera exportación se haya realizado.*

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

*NOTA: No hay preguntas para este ejercicio.*

### Tarea 1: Exportar los costes a BigQuery
En esta tarea vamos a establecer la exportación automática de costes a BigQuery y a analizarlos con SQL.

#### Exportar los costes
Para exportar los costes diarios a una tabla de BigQuery, puedes seguir las instrucciones detalladas en la [documentación](https://cloud.google.com/billing/docs/how-to/export-data-bigquery-setup#create-bq-dataset):
- Puedes empezar por el punto 4 (enlazado) directamente, ya que no tienes que crear un proyecto, verificar la facturación o habilitar la API de BigQuery Data Transfer Service.
- Como nombre de dataset puedes usar `billing_export`.
- Utiliza la multi-región de la Unión Europea ("EU") para crear el dataset.
- Como tipo de exportación, usa la exportación de costes estándar únicamente.
- La frecuencia de exportación puede variar, desde unas horas para la primera exportación hasta varias horas al día siguiente ([documentación](https://cloud.google.com/billing/docs/how-to/export-data-bigquery-tables#data-loads)).

***Espera unas horas o hasta un día para que se exporten los costes y continuar con la siguiente parte del ejercicio***

#### Analizar los costes con SQL
Antes de analizar los costes, familiarízate con el esquema de la tabla de costes estándar: [documentación del esquema](https://cloud.google.com/billing/docs/how-to/export-data-bigquery-tables#standard-usage-cost-data-schema).

También puedes navegar a **BigQuery > Espacio de trabajo SQL**, navegar al dataset y tabla creadas y analizar la pestaña de **Esquema** al pulsar sobre la tabla.

Una vez familiarizado con el esquema de la exportación, navega al editor de búsquedas SQL de BigQuery y ejecuta las siguientes queries:
1. Costes totales sumados por factura mensual: [query SQL](https://cloud.google.com/billing/docs/how-to/bq-examples#sum-costs-per-invoice).
1. Costes detallados por tipo, por factura mensual: [query SQL](https://cloud.google.com/billing/docs/how-to/bq-examples#costs-details-per-invoice).
1. Costes y créditos detallados por proyecto, para una factura mensual: [query SQL](https://cloud.google.com/billing/docs/how-to/bq-examples#query-by-project-by-invoice).
1. *Nota: Recuerda en cada query SQL sustituir el nombre de la tabla por el nombre de tu tabla de BiQuery.*

*ENTREGABLES:*
1. M2U1-6-tarea_1-archivo_1-captura_1.jpg: Captura del resultado de la 1ª query "Costes totales sumados por factura mensual".
1. M2U1-6-tarea_1-archivo_2-captura_2.jpg: Captura del resultado de la 2ª query "Costes detallados por tipo, por factura mensual".
1. M2U1-6-tarea_1-archivo_3-captura_3.jpg: Captura del resultado de la 3ª query "Costes y créditos detallados por proyecto, para una factura mensual".

### Tarea 2: Visualizar los costes con Data Studio
Uno de los primeros pasos recomendados al comenzar a trabajar con Google Cloud es exportar los costes a BigQuery y visualizarlos con Data Studio.

Data Studio es un sevicio de Google integrado con las BBDDs de Google Cloud Platform y otros servicios, como Google Drive, Google Analytics, AdWords, etc.

Las instrucciones para desplegar la plantilla del reporte de gastos de Data Studio y conectarlo a la tabla exportada de BigQuery, paso a paso, están disponibles en la [documentación](https://cloud.google.com/billing/docs/how-to/visualize-data#start-working-with-the-sample-cloud-billing-report) y en la propia plantilla de Data Studio.

*NOTA: Puede ser recomendable que vigiles los gastos durante el curso, utilizando los reportes de facturación de la consola y/o utilizando el reporte de Data Studio*.

*BONUS: ¿Te atreves a añadir una página nueva al dashboard de Data Studio con gráficas personalizadas?*

*ENTREGABLE*: M2U1-6-tarea_2-archivo_1-captura_1.jpg: Captura de la 1ª página del reporte de Data Studio.

## Resumen de entregas
1. M2U1-6-tarea_1-archivo_1-captura_1.jpg: Captura del resultado de la 1ª query "Costes totales sumados por factura mensual".
1. M2U1-6-tarea_1-archivo_2-captura_2.jpg: Captura del resultado de la 2ª query "Costes detallados por tipo, por factura mensual".
1. M2U1-6-tarea_1-archivo_3-captura_3.jpg: Captura del resultado de la 3ª query "Costes y créditos detallados por proyecto, para una factura mensual".
1. M2U1-6-tarea_2-archivo_1-captura_1.jpg: Captura de la 1ª página del reporte de Data Studio.

## Limpiar recursos
*No hay recursos que eliminar tras este ejercicio.*
