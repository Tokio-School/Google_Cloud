# Balanceadores de carga
Unidad M2U4

## ¿Qué vamos a hacer?
1. Crear 2 clústeres de frontend regionales y otro de backend regional con una aplicación web.
1. Desplegar un balanceador de carga interno regional para el clúster de backend y otro externo global para los clústeres de frontend.
1. Añadir un bucket de Cloud Storage al balanceador de carga externo y redirigir tráfico según destino.
1. Comprobar la alta escalabilidad y disponibilidad de los clústeres.

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

Para responder a todas las preguntas del ejercicio de forma agrupada, puedes crear antes de empezar un archivo a entregar llamado "M2U4-3-preguntas.txt". Recuerda identificar y ordenar cada pregunta seguida de su respuesta.

Encontrarás las preguntas entre el texto en cursiva: *PREGUNTA: ¿Cómo se llama el servicio de instancias de VMs de Google Cloud?*

### Tarea 1: Creación del clúster de backend y el balanceador de carga interno
xxx

#### Crear la red VPC
- crear la red y 2 subredes
- crear fwr para ssh
- crear fwr para tráfico interno FE a BE con net tags

#### Crear el clúster de backend
- crear plantilla de disco, script de startup, asignarle fwr con net tag backend
- Crear mig

#### Crear el balanceador de carga interno
- crear lb interno
- crear vm cliente interna
- testear lb interno

### Tarea 2: Creación de los clústeres de frontend y el balanceador de carga externo
xxx

#### Crear los clústeres de frontend
- duplicar plantilla de instancia, cambiar net tag a frontend
- crear mig en 2 regiones: fe próximo sin escalado, fe lejano con baja capacidad y autoescalado

#### Subir archivos estáticos a Cloud Storage
- crear bucket gcs y subir archivos estáticos - 1 imagen

#### Crear el balanceador de carga global externo
- crear lb
- healthcheck
- crear y aplicar fwr lb-a-fe
- comprobar conectividad a fe a través de lb

### Tarea 3: Comprobar la alta escalabilidad y disponibilidad
xxx

#### Alta disponibilidad
- apagar vm en clúster interno y externo
- comprobar conectividad a ambos
- comprobar cómo se recrean

#### Alta escalabilidad
- sobrecargar fe próximo
- ver cómo viene tráfico de otro fe
- ver cómo escala el otro fe

## Resumen de entregas
1. M2U4-3-preguntas.txt: Respuestas a todas las preguntas planteadas en el ejercicio.
1. [nombre de archivo]: descripción

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. paso1
1. paso2
