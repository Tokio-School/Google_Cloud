# Cloud SQL
Unidad M2U3 - Ejercicio 3

## ¿Qué vamos a hacer?
1. Crear una instancia de Cloud SQL.
1. Conectarnos a la misma de forma pública y privada.

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

*Nota: No hay preguntas para este ejercicio.*

Antes de comenzar, habilita las APIs de Cloud SQL y Cloud SQL Admin.

### Tarea 1: Crear una instancia de Cloud SQL
Vamos a comenzar creando la instancia de Cloud SQL y una instancia de VM que actúe como cliente:

#### Crear la instancia de VM cliente
En este primer paso, crea una instancia de VM con las siguientes características:
- Nombre: `client-vm`.
- Zona: `europe-west1-b`.
- Tipo de máquina: `e2-micro`.
- Disco de arranque: Debian 11, 10 GB estándar.
- Redes: Deshabilita la IP externa, para que sólo disponga de IP interna.

#### Crear la instancia de Cloud SQL
Ahora vamos a crear la instancia de MySQL de Cloud SQL:

1. En la consola, navega a **SQL** y pulsa en **Crear instancia**.
1. Escoge **MySQL**.
1. Explora las opciones y crea una instancia con las siguientes características (*nota:* recuerda desplegar el menú de **Personaliza tu instancia**):
    - ID de instancia: `mysql-bdd`.
    - Contraseña de root: escoge una contraseña que recuerdes o apúntala, como `root-passw`. La referenciaremos como `ROOT_PASSW`.
    - Versión: MySQL 8.0.
    - Región: europe-west1.
    - Disponibilidad zonal: varias zonas, europe-west1 como zona principal y cualquiera como secundaria.
    - Tipo de máquina: núcleo compartido de 1 vCPU y 1,7 GB.
    - Almacenamiento: 20 GB HDD.
    - Conexiones: IP pública.

*Nota:* La instancia de Cloud SQL puede tardar hasta 3-5 minutos en estar disponible.

De esta forma simple podemos crear una instancia de Cloud SQL con MySQL 8.0.

*ENTREGABLES:* M2U3-3-tarea_1-archivo_1-captura_1.jpg: Captura de pantalla de la página de **Descripción general** de la instancia de Cloud SQL.

### Tarea 2: Conexión pública a la instancia
Ahora vamos a conectarnos a la instancia de Cloud SQL de forma pública, a través de su IP pública.

Para ello utilizaremos Cloud Shell o una instalación local de Cloud SDK, entornos que no pueden acceder con IP interna ya que no están conectados internamente a Cloud SQL.

#### Conexión a través de usuario de MySQL
Para conectarnos a la instancia, primero debemos crear un usuario para la BBDD y no utilizar el usuario "root":
1. En la consola, navega a **SQL > Usuarios**.
1. Pulsa en **Agregar cuenta de usuario** y crea uno con las siguientes características:
    1. Selecciona **Autenticación integrada**.
    1. Nombre de usuario: `usuario_sql`.
    1. Contraseña: escoge una contraseña que recuerdes o apúntala, como `usuario-passw`. La referenciaremos como `USUARIO_PASSW`.
    1. Nombre de host: permitir cualquier host.

Ahora vamos a conectarnos utilizando el Cloud SDK en Cloud Shell o localmente, habilitando la IP de origen como red autorizada a conectarse a Cloud SQL:
1. Conéctate a la instancia con el usuario creado: `gcloud sql connect mysql-bdd -u usuario_sql`.
1. Usando la terminal de mysql, comprueba la conexión: `show databases;`.
1. Una vez comprobada, cierra la conexión: `exit`.

*ENTREGABLES:* M2U3-3-tarea_2-archivo_1-captura_1.jpg: Captura de pantalla mostrando el resultado del comando de `gcloud sql connect mysql-bdd -u usuario_sql`.

Puedes comprobar que se ha autorizado la IP de origen (Cloud Shell o la local) como red autorizada:
1. Navega a **SQL > Conexiones**, en **Redes autorizadas**.
1. Cloud SDK automáticamente registra la IP de origen y la autoriza durante al menos 5 minutos extendibles tras el último comando.

De esta forma podemos conectarnos de una forma sencilla a Cloud SQL desde un origen externo.

#### Conexión a través de Cloud SQL Auth proxy
Ahora vamos a utilizar el Cloud SQL Auth proxy para establecer una conexión de forma automática y encriptada desde cualquier entorno o aplicación:
1. Descarga el ejecutable y habilítalo como ejecutable siguiendo las instrucciones de la [documentación](https://cloud.google.com/sql/docs/mysql/connect-admin-proxy#install).
1. En la consola, navega a **SQL**, pulsa sobre la instancia y revisa la página de **Descripción general**.
1. Localiza la tarjeta de **Conectarse a esta instancia**, y dentro de ella copia el **Nombre de conexión** de la instancia.
1. Si no utilizas Cloud Shell, donde disponemos de un cliente de MySQL ya instalado, instala el cliente en local: [instrucciones](https://dev.mysql.com/doc/refman/5.7/en/installing.html).
1. Inicia el Cloud SQL Auth proxy: `./cloud_sql_proxy -instances=NOMBRE_CONEXION_INSTANCIA=tcp:3306`.
1. Abre otra pestaña de terminal o terminal y conéctate al proxy local: `mysql -u usuario_sql -p --host 127.0.0.1` (utiliza la contraseña de usuario).
1. Usando la terminal de mysql, comprueba la conexión: `show databases;`.

*Nota:* Si quieres comprobar cómo subir y trabajar con datos en MySQL, puedes seguir las [instrucciones del quickstart](https://cloud.google.com/sql/docs/mysql/quickstart#create_a_database_and_upload_data) de la documentación.

De esta forma podemos conectarnos a Cloud SQL a través de un proxy local con Cloud SQL Auth proxy.

*ENTREGABLES:* M2U3-3-tarea_2-archivo_2-captura_2.jpg: Captura de pantalla mostrando el resultado del comando de `./cloud_sql_proxy -instances=NOMBRE_CONEXION_INSTANCIA=tcp:3306`.

### Tarea 3: Conexión privada a la instancia
Ahora vamos a conectarnos de forma privada a la instancia, a través de IP interna.

#### Configurar la instancia de Cloud SQL como interna
Vamos a configurar Private Servicess Access para habilitar una conexión privada entre un rango de IPs privadas en nuestra VPC y la instancia de Cloud SQL:

1. En la consola, navega a **SQL**, pulsa sobre tu instancia y, en la columna de la izquierda, navega a **Conexiones**.
1. Habilita la **IP privada** para la instancia, selecciona la red `default` y, en el cuadro de advertencia, pulsa **Configurar conexión**.
1. Sigue las instrucciones para habilitar la conexión de acceso a servicios privados:
    1. Habilita la API de Service Networking.
    1. Usa un rango de IP asignado automáticamente.
1. Recuerda guardar los cambios al acabar.

De esta forma hemos habilitado Private Servicess Access en nuestra VPC `default` y una IP privada para la instancia de Cloud SQL.

*Nota:* La configuración de la instancia necesita reiniciarse y puede tardar hasta 10 minutos.

#### Conectarse a instancia privada
Para comprobar la conexión privada, conéctate a la instancia de VM cliente creada previamente, instala el Cloud SQL Auth Proxy y el cliente de mysql siguiendo las instrucciones anteriores, y comprueba la conexión a la instancia.

*ENTREGABLES:* M2U3-3-tarea_3-archivo_1-captura_1.jpg: Captura de pantalla mostrando la conexión desde la instancia de VM cliente.

#### Conexión a través de autenticación en la base de datos con IAM
Para autenticarse con nuestro usuario y acceso de Cloud IAM en Cloud SQL, lo primero es habilitar la autenticación con IAM en la instancia:
1. En la consola, navega a la instancia de Cloud SQL y pulsa sobre **Editar**.
1. En **Personaliza tu instancia** expande **Marcas**.
1. Selecciona `cloudsql_iam_authentication` y establécela como `On`.

Ahora vamos a crear un usuario en la BBDD con la autenticación habilitada:
1. En la página de detalles de la instancia de Cloud SQL, selecciona **Usuarios** en la columna de la izquierda.
1. Pulsa en **Agregar cuenta de usuario** y selecciona **Cloud IAM**.
1. Agrega tu cuenta de usuario, indicando tu dirección de email.

Por tener rol de propietario de proyecto, ya tenemos los permisos suficientes para loguearnos en la instancia. Sin embargo, vamos a ver cómo se añadirían para otro usuario:
1. En la consola, navega a **IAM y administración > IAM**, busca tu usuario y pulsa el botón de editar (icono de un lápiz).
1. Agrega los roles de "Cliente de Cloud SQL y "Usuario de instancia de Cloud SQL".

*Nota:* Puede que el rol de "Usuario de instancia de Cloud SQL" se haya asociado automáticamente.

Ahora por último vamos a conectarnos a la instancia con autenticación de IAM:

*Nota:* Por simplicidad, vamos a conectarnos de forma externa a través de Cloud Shell o una instalación de Cloud SDK local.
1. Sigue las instrucciones para crear un nuevo certificado y claves: [documentación](https://cloud.google.com/sql/docs/mysql/configure-ssl-instance#new-client).
1. En la consola, copia la dirección IP externa de la instancia de Cloud SQL, que referenciaremos como `IP_EXTERNA_SQL`.
1. Utiliza el cliente mysql para conectarte a la instancia: 
```
MYSQL_PWD=`gcloud auth print-access-token` mysql --enable-cleartext-plugin --ssl-ca=server-ca.pem --ssl-cert=client-cert.pem --ssl-key=client-key.pem --host=IP_EXTERNA_SQL --user=NOMBRE_USUARIO
```
    1. Asegúrate de que el certificado y claves están en el directorio actual, o modifica el path a los mismos.
    1. Como `NOMBRE_USUARIO`, usa la parte de nombre de tu dirección de correo electrónico, sin "@" ni el dominio. P. ej., `info@indavelopers.com` -> `info`.
1. Comprueba el acceso a la BBDD: `show databases;`.

De esta forma podemos autenticarnos en la BBDD con un usuario sin tener que usar una contraseña en la misma.

*ENTREGABLES:* M2U3-3-tarea_3-archivo_2-captura_2.jpg: Captura de pantalla mostrando el resultado del comando de conexión utilizando el token de autenticación.

## Resumen de entregas
1. M2U3-3-preguntas.txt: Respuestas a todas las preguntas planteadas en el ejercicio.
1. M2U3-3-tarea_1-archivo_1-captura_1.jpg: Captura de pantalla de la página de **Descripción general** de la instancia de Cloud SQL.
1. M2U3-3-tarea_2-archivo_1-captura_1.jpg: Captura de pantalla mostrando el resultado del comandos de `gcloud sql connect mysql-bdd -u usuario_sql`.
1. M2U3-3-tarea_2-archivo_2-captura_2.jpg: Captura de pantalla mostrando el resultado del comando de `./cloud_sql_proxy -instances=NOMBRE_CONEXION_INSTANCIA=tcp:3306`.
1. M2U3-3-tarea_3-archivo_1-captura_1.jpg: Captura de pantalla mostrando la conexión desde la instancia de VM cliente.
1. M2U3-3-tarea_3-archivo_2-captura_2.jpg: Captura de pantalla mostrando el resultado del comando de conexión utilizando el token de autenticación.

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. Elimina la instancia de VM de cliente.
1. Elimina la instancia de Cloud SQL.
1. Sigue las instrucciones para [eliminar la conexión privada a servicios](https://cloud.google.com/vpc/docs/configure-private-services-access#removing-connection).
