# Redes y subredes
Unidad M2U4

## ¿Qué vamos a hacer?
1. Explorar las posibilidades de creación de redes y subredes.
1. Comprobar la conectividad entre regiones.
1. Comprobar la conectividad entre recursos en la misma red y en distintas redes.
1. Modificar los rangos de direccionamiento y crear rangos de IP alias.
1. Analizar los logs de tráfico de subred.

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

Para responder a todas las preguntas del ejercicio de forma agrupada, puedes crear antes de empezar un archivo a entregar llamado "M2U4-1-preguntas.txt". Recuerda identificar y ordenar cada pregunta seguida de su respuesta.

Encontrarás las preguntas entre el texto en cursiva: *PREGUNTA: ¿Cómo se llama el servicio de instancias de VMs de Google Cloud?*

### Tarea 1: Creación de redes y subredes
xxx

#### Crear una red de modo automático
- analizar red default
- crear red de modo automático
- explorar fwr por defecto
- analizar red auto, rutas, nº subredes y nombre

#### Crear una red de modo personalizado
- crear red custom
- crear 3 subredes - rangos diferentes
- explorar fwr, rutas

### Tarea 2: Comprobación de conectividad
xxx

#### Crear de las instancias de VM
- crear 3 vms: 2 en red custom, 1 en red auto

#### Comprobar la conectividad
- ssh a vm1
- comprobar ping a IP externa de vm2 y vm3
- comprobar ping a IP interna de vm2 y vm3
- test de conectividad con network intelligence center

### Tarea 3: Direccionamiento
xxx

#### Ampliar rangos de direcciones IP
- ampliar rango de subred auto, probar a más de /16
- ampliar rango de subred custom, probar

#### Utilizar IP de alias
- crear rango de IP alias en subred custom
- crear vm con webapp con ip alias
- comprobar webapp en vm
- comprobar por ip primaria
- test conectividad a webapp

### Tarea 4: Registros de tráfico
xxx

- habilitar logs vpc en subred
- provocar tráfico con bucle for entre vms
- revisar logs de tráfico en cloud logging
- deshabilitar logs vpc

## Resumen de entregas
1. M2U4-1-preguntas.txt: Respuestas a todas las preguntas planteadas en el ejercicio.
1. [nombre de archivo]: descripción

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. paso1
1. paso2

* a veces los recursos pueden tardar en borrarse o indicar que necesitan antes que se borren otras dependencias, o tener que probar de nuevo
