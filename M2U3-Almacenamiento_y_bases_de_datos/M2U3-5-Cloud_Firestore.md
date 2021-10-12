# Cloud Firestore
Unidad M2U3 - Ejercicio 5

## ¿Qué vamos a hacer?
1. Crear una instancia de Cloud Firestore.
1. Gestionar datos con las librerías de cliente.
1. Gestionar datos desde la consola.

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

*Nota: No hay preguntas para este ejercicio.*

### Tarea 1: Crear una instancia de Cloud Firestore
Primero vamos a crear la instancia de Cloud Firestore. Cloud Firestore está basada en el antiguo servicio de Cloud Datastore, que aún se puede utilizar como Cloud Firestore en modo Datastore, y mantiene la limitación de sólo poder desplegar una instancia por proyecto, por lo que no podemos tener instancias en localizaciones diferentes o en modos nativo/Datastore diferentes en el mismo proyecto.

Para crear una instancia de Cloud Firestore:
1. En la consola, navega a **Firestore**.
1. Selecciona el modo nativo.
1. Como localización, selecciona `europe-west3`.

Una vez creada la instancia, utilizaremos las librerías de cliente para trabajar con datos, y posteriormente la consola de nuevo para acceder a ellos.

### Tarea 2: Gestionar datos con las librerías de cliente
En esta tarea vamos a gestionar datos con la librería de cliente de Python.

*Nota:* Puedes seguir las siguientes instrucciones desde Cloud Shell o tu instalación de Cloud SDK local.

#### Crear el entorno local
Ahora vamos a crear una cuenta de servicio para autenticar nuestras peticiones desde scripts de librerías de cliente:
1. Crea una carpeta en tu entorno local y accede a ella: `mkdir M2U3-5-Cloud_Firestore && cd M2U3-5-Cloud_Firestore`.
1. Crea una cuenta de servicio llamada `NOMBRE_SA`: `gcloud iam service-accounts create NOMBRE_SA`.
1. Asígnale el rol de usuario de Firestore a la cuenta de servicio: `gcloud projects add-iam-policy-binding ID_PROYECTO --member="serviceAccount:NOMBRE_SA@ID_PROYECTO.iam.gserviceaccount.com" --role="roles/datastore.user"`.
1. Descarga en local una clave de credencial: `gcloud iam service-accounts keys create credentials.json --iam-account=NOMBRE_SA@ID_PROYECTO.iam.gserviceaccount.com`.
1. Habilita las "Application Default Credentials" con el path al archivo de credenciales: `export GOOGLE_APPLICATION_CREDENTIALS="PATH/DIRECTORIO/ACTUAL/credentials.json"` (puedes averiguar el path del directorio actual con `pwd`).

Ahora crea un entorno virtual de Python e instala la librería:
1. `pip install virtualenv`.
1. `virtual env`.
1. `source env/bin/activate`.
1. `pip install -u google-cloud-firestore`.

Puedes explorar el archivo [snippets.py](https://github.com/GoogleCloudPlatform/python-docs-samples/blob/HEAD/firestore/cloud-client/snippets.py) que incluye funciones con el código que vamos a utilizar.

Ahora prepara un script de Python que ejecutaremos con el código de cada paso:
1. Crea un script llamado `script.py`.
1. En dicho archivo, copia las siguientes líneas de código Python (sustituyendo `ID_PROYECTO`):
```Python
from google.cloud import firestore

db = firestore.Client(project='ID_PROYECTO')
```
1. En cada paso, añade en líneas posteriores el código indicado, reemplazando el código de pasos anteriores, pero manteniendo siempre las 2 líneas anteriores.
1. Para ejecutarlo en cada paso, hazlo con `python script.py`.

#### Subir datos
Para añadir un documento con ID `alovelace` a la colección `users`, ejecuta el siguiente código:

```Python
doc_ref = db.collection(u'users').document(u'alovelace')
doc_ref.set({
    u'first': u'Ada',
    u'last': u'Lovelace',
    u'born': 1815
})
```

Para añadir otro documento con ID `aturing` a la misma colección, ejecuta el siguiente código:

```Python
doc_ref = db.collection(u'users').document(u'aturing')
doc_ref.set({
    u'first': u'Alan',
    u'middle': u'Mathison',
    u'last': u'Turing',
    u'born': 1912
})
```

#### Consultar datos
Para consultar todos los documentos de la colección `users`, ejecuta el siguiente código:

```Python
users_ref = db.collection(u'users')
docs = users_ref.stream()

for doc in docs:
    print(f'{doc.id} => {doc.to_dict()}')
```

*BONUS:* Para aprender más sobre cómo gestionar datos con la librería de Firestore para Python, puedes ejecutar los pasos disponibles en la documentación:
1. [Añadir datos](https://cloud.google.com/firestore/docs/manage-data/add-data).
1. [Buscar y filtrar datos](https://cloud.google.com/firestore/docs/query-data/queries).
1. [Ordenar y limitar datos](https://cloud.google.com/firestore/docs/query-data/order-limit-data).

### Tarea 3: Gestionar datos desde la consola
Por último, en esta tarea, vamos a consultar y gestionar datos a través de la consola:

#### Consultar datos
1. Navega a **Firestore > Datos**.
1. Para consultar un documento o colección en un `path` concreto, navega por la interfaz o pulsa el botón de **Editar path** (icono de lápiz).
1. Para filtrar datos en una colección, abre la colección, pulsa el botón de filtrado (3 líneas en pirámide invertida) y añade algunos filtros.

*ENTREGABLES:* M2U3-5-tarea_3-archivo_1-captura_1.jpg: Captura de pantalla de un documento de alguna colección en la consola web.

#### Añadir datos
1. Para añadir una colección, pulsa el botón **Comenzar colección** y usa un ID cualquiera o deja que Firestore genere uno automáticamente.
1. Para añadir un documento, pulsa sobre el botón **Añadir documento** y añade campos o comienza una subcolección.
1. Para editar datos, accede a un documento y pulsa en un campo para editarlo, o pulsa en **Añadir campo** o **Comenzar colección** para añadir campos o comenzar una subcolección.

#### Eliminar datos
1. Para eliminar un documento, selecciona el documento y pulsa sobre el icono de menú y sobre **Eliminar documento**.
1. Para eliminar una colección, selecciona la colección y pulsa sobre el icono de menú y sobre **Eliminar colección**.

## Resumen de entregas
1. M2U3-5-tarea_3-archivo_1-captura_1.jpg: Captura de pantalla de un documento de alguna colección en la consola web.

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. Elimina las colecciones y documentos creados
1. (Opcional) Elimina el entorno local con los archivos y el entorno virtual de Python.
