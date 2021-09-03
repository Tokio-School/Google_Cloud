# Entorno de trabajo
Unidad M2U1

## ¿Qué vamos a hacer?
1. Crear nuestro proyecto de Google Cloud y cuenta de facturación.
1. Activar los créditos gratuitos para nuevos usuarios.
1. Configurar el entorno de trabajo.

## Creación inicial del entorno de trabajo
*NOTA:* Recuerda acostumbrarte a crear la carpeta de entrega de prácticas y subcarpetas para cada ejercicio antes de comenzar con ellos.

### Tarea 1: Gestionar la facturación y créditos gratuitos
Vamos a activar el programa de créditos gratuitos para nuevos usuarios para poder usarlo durante el curso.

#### Cuenta de usuario
Lo primero que necesitas para utilizar Google Cloud es una cuenta de usuario de Google.

Como cuentas de usuario puedes utilizar una cuenta de correo electrónico de GMail, una cuenta de @google.com, una cuenta creada en Google Workspace (anteriormente conocido como GSuite) de negocio o académica, de Cloud Identity o una cuenta de usuario de Google sin dirección de correo electrónico de GMail asociada.

Si no dispones de ninguna de ellas, puedes crear una cuenta de usuario con GMail asociado o no aquí: [Google Sign in](https://accounts.google.com/servicelogin).

Una vez disponible tu cuenta de usuario, loguéate en [google.com](https://www.google.com/) con ella.

#### Habilitar créditos gratuitos
El programa de créditos gratuitos de Google Cloud tiene 2 ventajas:
1. $300 en créditos de Google Cloud gratuitos para nuevos usuarios durante 90 días.
1. Servicios "always free" que, si no sobrepasamos un tier de uso, serán siempre gratuitos.

Puedes ver las condiciones del programa aquí: [Google Cloud Free Program](https://cloud.google.com/free/docs/gcp-free-tier/).

*Nota: Si tienes algún problema para cumplir las condiciones o durante la activación del programa, ponte en contacto con el profesor mediante la plataforma.*

***Nota importante:** Para comprobar tu identidad y que eres un usuario nuevo y real, te solicitarán tu tarjeta de débito o crédito, pero no harán ningún cargo tras acabarse los $300 si no actualizas tu cuenta de facturación a una cuenta de pago. Más información en las condiciones del programa.*

Puedes darte de alta en el programa siguiendo las instrucciones del botón **Empezar gratis** (esquina superior derecha) que encontrarás en el siguiente enlace: [Google Cloud Free Program](https://cloud.google.com/free).

Al hacerlo:
- Se creará una cuenta de facturación con los créditos gratuitos activados.
- Se creará un proyecto de Google Cloud llamado "My First Project" para poder trabajar directamente en él.

Videos de ejemplo:
- [The Google Cloud Platform Free Trial and Free Tier](https://www.youtube.com/watch?v=P2ADJdk5mYo)
- [How to activate Google Cloud Platform's Free Trial account detailed steps to get $300 for 90 days?](https://www.youtube.com/watch?v=jbpSPO9np3E)

Una vez activado el programa, accederás a la consola web de Google Cloud o [Google Cloud Console](https://console.cloud.google.com).

Al acceder a la consola, encontrarás una barra superior con la información de tu uso del programa gratuito.

También podrás acceder siempre a la información de la cuenta de facturación creada con los créditos activados en la sección **Facturación** del menú de navegación (menú de hamburguesa en la esquina superior izquierda).

### Tarea 2: Configurar el proyecto en Google Cloud
Ahora vamos a configurar el proyecto de Google Cloud creado automáticamente.

Para ello, en la consola, utilizando el menú de navegación navega a [**IAM y administración > Configuración**](https://console.cloud.google.com/iam-admin/settings) (enlace directo a la sección de la consola).

La ID de proyecto es un valor inmutable, pero puedes cambiar el nombre del proyecto a uno más descriptivo.

También puedes crear un proyecto nuevo con una ID globalmente única de proyecto diferente navegando a [**IAM y administración > Crear proyecto**](https://console.cloud.google.com/projectcreate) (enlace directo a la sección de la consola), seleccionando la cuenta de facturación creada automáticamente.

Para las prácticas, utilizaremos sólo un proyecto, por lo que por favor selecciona uno y utiliza el mismo para todas ellas.
Siempre puedes eliminar un proyecto creado en [**IAM y administración > Configuración**](https://console.cloud.google.com/iam-admin/settings) (***Nota: ten mucho cuidado de no eliminar todos tus proyectos o tendrás que seguir el enlace del párrafo anterior para crear uno nuevo.***).

#### Conceder permiso al profesor
Para poder ayudarte si tienes cualquier problema durante el curso y para corregir alguna práctica si es necesario, debes concederle acceso al profesor a tu proyecto de trabajo.

Para ello:
1. Navega a [**IAM y administración > IAM**](https://console.cloud.google.com/iam-admin/iam) (enlace directo a la sección de la consola).
1. Pulsa el botón **Agregar**.
1. Agrega como nuevo miembro al profesor: info@indavelopers.com - Marcos Manuel Ortega
1. Como función o rol, selecciona **Básico > Propietario**.
1. Pulsa **Guardar**.

*ENTREGABLE:*
Como entregable de esta práctica, si has realizado todo correctamente debe llegarle un email automático con la invitación a tu proyecto al profesor, que comprobará que tiene acceso y todo funciona correctamente.

Por tanto, únicamente genera un archivo "M2U1-0-preguntas.txt" con la siguiente información:
- Tu nombre completo.
- La dirección de correo electrónico usada para loguearte en Google Cloud.
- Tu ID de proyecto (*nota*: la ID única del proyecto, no el nombre del mismo).

## Resumen de entregas
1. M2U1-0-preguntas.txt: Nombre, correo electrónico e ID del proyecto.

## Limpiar recursos
*No hay recursos que eliminar tras este ejercicio.*
