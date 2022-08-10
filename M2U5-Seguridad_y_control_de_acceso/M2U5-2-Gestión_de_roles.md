# Gestión de roles
Unidad M2U5 - Ejercicio 2

## ¿Qué vamos a hacer?
1. Explorar y comparar los roles disponibles y sus permisos.
1. Administrar roles personalizados.
1. Asignación de roles y políticas de acceso heredadas.

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

Para responder a todas las preguntas del ejercicio de forma agrupada, puedes crear antes de empezar un archivo a entregar llamado "M2U0-0-preguntas.txt". Recuerda identificar y ordenar cada pregunta seguida de su respuesta.

Encontrarás las preguntas entre el texto en cursiva: *PREGUNTA: ¿Cómo se llama el servicio de instancias de VMs de Google Cloud?*

### Tarea 1: Explorar y auditar roles

#### Explorar roles y permisos
En esta primera tarea, vamos primero a explorar los roles disponibles y sus permisos, analizando los 3 tipos de roles comentados: básicos, predefinidos y personalizados.

Para ello, explora los roles asignables en un proyecto de 3 formas diferentes: en la consola, con Cloud SDK y en la referencia de la documentación:
1. Explora los roles asignables en un proyecto desde la consola web:
    1. En el menú de navegación, navega a **IAM y administración > IAM**
    1. Pulsa en **Agregar** y explora las diferentes categorías de roles asignables: "utilizados últimamente", "básicos" y el botón de **Administrar funciones** (o roles personalizados), que te llevará a la categoría **IAM y administración > Funciones**
1. Explora los roles asignables con Cloud SDK:
    1. En la terminal con Cloud SDK disponible (Cloud Shell o local), ejecuta el comando `gcloud iam list-grantable-roles //cloudresourcemanager.googleapis.com/projects/[ID_PROYECTO]`
    1. Explora la larga lista de roles que devuelve el comando. Puedes filtrarla con la opción de [filtros](https://cloud.google.com/sdk/gcloud/reference/topic/filters) o añadiendo `| grep [texto a filtrar]` al comando
1. Explora los roles disponibles y sus permisos es en la referencia de la documentación:
    1. Explora los roles en la referencia: [Descripción de las funciones](https://cloud.google.com/iam/docs/understanding-roles)
    1. Explora los servicios individuales en la referencia: [Referencia de permisos de IAM](https://cloud.google.com/iam/docs/permissions-reference)

*PREGUNTAS*:
1. *¿Cuál crees que es la forma más efectiva de analizar los roles disponibles?*
1. *¿Qué roles sobre redes hay disponibles en Compute Engine?*
1. *¿Qué roles pueden crear y/o modificar una regla de cortafuegos (que no política de cortafuegos)?*

Ahora vamos a escoger los roles más apropiados para un técnico. Para ello, vamos a:
1. Listar las acciones que tiene que realizar en su trabajo del día a día
1. Identificar los permisos necesarios para las mismas
1. Escoger entre los roles disponibles aquellos que debemos asignarle

Y como siempre, lo hacemos manteniendo el **Principio de Mínimo Privilegio**.

Para ello, sigue los pasos anteriormente indicados y responde a las siguientes preguntas:
- El técnico que escogeremos como ejemplo es un desarrollador web, encargado de desarrollar, desplegar su aplicación web en una instancia de VM, conectarse a la misma a través de SSH, monitorizarla y administrarla, y gestionar la conexión a la BBDD.
- Otros técnicos serán los únicos encargados de la administración de redes y de la BBDD en el proyecto.
- Los servicios que utilizará en este proyecto serán Compute Engine y Cloud SQL únicamente.
- Por aplicación del Principio de Mínimo Privilegio, no debe disponer de acceso a más roles/permisos de los estrictamente necesarios en su día a día.
- Una de las mejores formas de listar las tareas habituales e identificar los roles/permisos necesarios es utilizar los quickstarts, guías y tutoriales para las acciones habituales en la documentación, p. ej. 

Puedes guiarte por la siguiente guía: [Escoger roles predefinidos](https://cloud.google.com/iam/docs/choose-predefined-roles).

*PREGUNTAS*:
1. *Lista las acciones que tiene que realizar en su trabajo en el día a día en los servicios indicados*.
1. *Identifica los permisos necesarios para las mismas*.
1. *Manteniendo el principio de mínimo privilegio, escoge los roles apropiados que debemos asignarle*.

#### Auditar roles y accesos
Auditar roles y accesos es una de las tareas de seguridad más importantes en la nube que debemos realizar frecuentemente y de forma minuciosa.

En este contexto, nos referimos a auditar o comprobar si cada pricipal (persona, grupo, cuenta de servicio o dominio) tiene los accesos que requiere y sólo los que requiere en su trabajo del día a día, no faltándole ningún acceso ni disponiendo de uno que pueda significar un problema de seguridad en el futuro.

Del mismo modo, nos aseguramos de que la asignación de roles y accesos se mantiene al día, y que ningún principal cuenta con un acceso que ya no requiera.

Una parte igualmente fundamental de esta auditoría sería auditar la perteniencia de usuarios y cuentas de servicio a grupos, a qué grupos pertenecen, si deben pertenecer (aún) o no, a qué grupos más deberían pertenecer o si habría que administrar, dividir o agrupar los grupos de una forma diferente, manteniéndolos actualizados. Sin embargo, al no disponer de una organización en GCP ni en Cloud Identity/Google Workspace, no podremos utilizar más de una única cuenta de usuario en estas prácticas.

La auditoría de roles y accesos frecuente sigue, entre otras, los siguientes pasos:
- Auditar los usuarios creados
- Auditar sus sistemas de logueo, contraseñas, MFA, etc.
- Auditar los grupos y sus miembros
- Auditar las cuentas de servicio, sus credenciales, roles y quién puede acceder o administrarlas
- Auditar los dominios, como la asignación de cuentas de usuario desde un dominio u otro o su asignación duplicada al mismo usuario
- Auditar la jerarquía de recursos de la organización: carpetas, proyectos y cuentas de facturación
- Auditar la asignación de roles a todos estos principales

Para auditar los accesos puedes comprobar la política de IAM desde cualquier interfaz (consola web, Cloud SDK, etc.), el [Analizador de Políticas](https://cloud.google.com/policy-intelligence/docs/policy-analyzer-overview) y su [Analizador de políticas de IAM](https://cloud.google.com/policy-intelligence/docs/analyze-iam-policies), y la guía [Soluciona problemas de permisos de IAM](https://cloud.google.com/policy-intelligence/docs/troubleshoot-access).

Para esta práctica vamos a optar por una versión simplificada de la misma:
*PREGUNTAS*:
1. *¿Qué usuarios tienen algún rol asignado a tu proyecto? ¿Por qué deben tenerlo, cuáles son sus funciones en el mismo?*
1. *¿Qué cuentas de servicio existen en tu proyecto? ¿Por qué existen, deberían seguir existiendo?*
1. *¿Quién tiene acceso a dichas cuentas de servicio?*
1. *¿Qué grupos y dominios tienen acceso a tu proyecto?*
1. *¿Qué usuarios y cuentas de servicio tienen acceso a tu proyecto de forma directa, y cuáles un acceso heredado de una carpeta/organización, de cuál, o de un grupo y dominio?*
1. *¿Qué roles tienen asignados cada uno de los principales mencionados? ¿Por qué tienen asginados esos roles? ¿Deben seguir contando con ellos? ¿Podríamos sustituirlos por otros roles más convenientes?*

Relacionado con la auditoría de roles y accesos, contamos en Google Cloud con el Recomendador que sugiere, entre otras, [recomendaciones de roles](https://cloud.google.com/policy-intelligence/docs/role-recommendations-overview).

### Tarea 2: Administrar roles y accesos
En esta tarea vamos a comprobar la asignación de roles a un principal.

Para ello, comienza por crear una cuenta de servicio, un bucket de Cloud Storage y una instancia de VM con dicha cuenta de servicio asignada, siguiendo los pasos vistos en ejercicios previos como p. ej. [M2U5-1-Cuentas_de_servicio](https://github.com/Tokio-School/Google_Cloud/blob/main/M2U5-Seguridad_y_control_de_acceso/M2U5-1-Cuentas_de_servicio.md).

Una vez creados, asigna un rol a dicha cuenta de servicio:
1. En la consola web, en el menú de navegación, navega a **IAM y administración > IAM**, pulsa **Agregar** y asigna el rol `Cloud Storage Editor` a la cuenta de servicio, habiendo copiado su ID o "email" previamente.
1. Conéctate a la instancia de VM por SSH, a través de la consola web o Cloud SDK.
1. Comprueba el acceso de la cuenta de servicio o instancia de VM a Cloud Storage subiendo y descargando un archivo al buket de GCS desde la instancia de VM.
1. (OPCIONAL) Comprueba dicho acceso con el [Analizador de políticas de IAM](https://cloud.google.com/policy-intelligence/docs/analyze-iam-policies) previamente mencionado.

Ahora elimina el rol asignado a la cuenta de servicio y sigue los mismos pasos para comprobar que ya no puedes realizar dichas acciones con GCS.

OPCIONAL: Explora cómo asignar y administrar roles utilizando [Cloud SDK](https://cloud.google.com/iam/docs/granting-changing-revoking-access).

#### Administrar roles personalizados
Por último, vamos a practicar la creación y administración de roles personalizados. Para ello, básate en los apartados de la siguiente guía para completar los pasos siguiendo la consola o Cloud SDK: [Crear y administrar funciones personalizadas](https://cloud.google.com/iam/docs/creating-custom-roles).
1. Crea un rol personalizado, bien a partir de uno o varios roles predefinidos o seleccionando los permisos individualmente:
    - Nombre: `gcs-ver-buckets-y-objetos`
    - Etapa de lanzamiento: `ALPHA`
    - Permisos: lista los necesarios para tener únicamente acceso de lectura a los buckets y objectos de GCS
1. Asigna dicho rol a la cuenta de servicio
1. Compruba que la instancia de VM con la cuenta de servicio asignada puede realizar dichas operaciones

Ahora vamos a deshabilitar el rol para comprobar cómo podemos detener su uso por cualquier razón de mantenimiento, auditoría o seguridad, sin tener que llegar a eliminar el rol personalizado:
1. Deshabilita el rol personalizado, usando la consola web o Cloud SDK.
1. Comprueba cómo el rol sigue existiendo y sigue asignado a la cuenta de servicio
1. Comprueba cómo la cuenta de servicio ya no puede realizar dichas operaciones
1. Habilita el rol de nuevo
1. Comprueba cómo ahora la cuenta de servicio puede realizar dichas operaciones de nuevo

Por último, elimina el rol y comprueba:
1. Que el rol no aparece en la lista de roles
1. Que el rol no aparece asignado a la cuenta de servicio
1. Que la cuenta de servicio no puede realizar dichas operaciones

## Resumen de entregas
1. M2U5-2-preguntas.txt: Respuestas a todas las preguntas planteadas en el ejercicio.

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. Eliminar el rol personalizado creado.
1. Eliminar la instancia de VM.
1. Eliminar el bucket de GCS.
1. Eliminar la cuenta de servicio creada.
