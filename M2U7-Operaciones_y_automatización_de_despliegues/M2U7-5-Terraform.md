# IaC con Terraform
Unidad M2U7 - Ejercicio 5

## ¿Qué vamos a hacer?
1. Crear despliegues de IaC con Terraform.

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

Para responder a todas las preguntas del ejercicio de forma agrupada, puedes crear antes de empezar un archivo a entregar llamado "M2U7-5-preguntas.txt". Recuerda identificar y ordenar cada pregunta seguida de su respuesta.

Encontrarás las preguntas entre el texto en cursiva: *PREGUNTA: ¿Cómo se llama el servicio de instancias de VMs de Google Cloud?*

### Tarea 1: Despliegues de IaC
En esta tarea vamos a crear un despliegue de infraestructura utilizando Terraform, donde desplegaremos una red VPC, una regla de cortafuegos y 2 instancias de VM.

Para este ejercicio, recomendamos seguirlo utilizando Cloud Shell, ya que cuenta con Terraform ya instalado. Si quieres seguirlo desde un entorno local con Cloud SDK, instala Terraform para el mismo siguiendo las instrucciones de su documentación.

Para trabajar con archivos y directorios en Cloud Shell, tienes la opción de hacerlo con el IDE gráfico de Cloud Shell o con un editor de texto de línea de comandos: Nano, Vim y Emacs. Si quieres utilizar el IDE gráfico, actívalo con el botón de un lápiz en la parte superior de Cloud Shell y familiarízate con su explorador y árbol de directorios y archivos.

Vamos a comenzar nuestro despliegue con Terraform:
1. Comprueba que Terraform está disponible: `terraform --version`
1. Crea un directorio para tu despliegue con el editor gráfico o la terminal: `mkdir tfinfra && cd tfinfra`
1. Declara el proveedor de Terraform para Google Cloud en un archivo llamado `provider.tf` con este conenido: `provider "google" {}`
1. Inicializa el despliegue de Terraform: `terraform init`

De esta forma hemos inicializado Terraform y el proveedor de Google Cloud en nuestro entorno.

#### Crear plantilla de despliegue
Vamos a crear la plantilla con el código que definirá nuestro despliegue.

Para crear la plantilla necesitamos crear este árbol de directorios y archivos:
1. `tfinfra`
    1. `instance`
        1. `main.tf`
    1. `mynetwork.tf`
    1. `provider.tf`

Como contenido para dichos archivos, encontrarás las plantillas en los siguientes enlaces:
- `main.tf`: [main.tf](https://storage.googleapis.com/cloud-training/archinfra/terraform/instance/main.tf)
- `mynetwork.tf`: [mynetwork.tf](https://storage.googleapis.com/cloud-training/archinfra/terraform/mynetwork.tf)
- `provider.tf`: [provider.tf](https://storage.googleapis.com/cloud-training/archinfra/terraform/provider.tf)

#### Desplegar la plantilla
Una vez creado nuestro despliegue, vamos a desplegar la arquitectura a través de Terraform:
1. Formatea los archivos de Terraform con un formato y estilo estándar: `terraform fmt`
1. Inicializa de nuevo Terraform para que cargue los módulos creados: `terraform init`
1. Revisa el plan de despliegue de Terraform con cuidado: `terraform plan`
1. Si el plan es correcto, despliégalo: `terraform apply`

Ahora comprueba desde la consola web que se han desplegado las instancias, la red de VCP y su regla de cortafuegos, y que puedes conectarte a las instancias por SSH.

*ENTREGABLE: M2U7-5-tarea_1-archivo_1-captura_1.jpg: Captura de pantalla mostrando las últimas líneas de la respuesta del comando de despliegue de Terraform.*

#### Modificar el despliegue
*BONUS: ¿Te atreves a modificar el despliegue, a desplegar una tercera instancia de VM en otra región?*

## Resumen de entregas
1. M2U7-5-tarea_1-archivo_1-captura_1.jpg: Captura de pantalla mostrando las últimas líneas de la respuesta del comando de despliegue de Terraform.

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. Elimina los archivos de Terraform del entorno utilizado
1. Elimina los recursos desplegados: `terraform destroy`
