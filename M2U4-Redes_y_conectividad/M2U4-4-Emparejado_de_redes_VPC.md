# Emparejamiento de redes VPC
Unidad M2U4

## ¿Qué vamos a hacer?
1. Establecer conectividad interna entre redes con emparejamiento de redes VPC.
1. Comprobar la descentralización de la gestión de redes a través de reglas de cortafuegos.
1. Comprobar cómo el emparejamiento entre redes no es transitivo.

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

Para responder a todas las preguntas del ejercicio de forma agrupada, puedes crear antes de empezar un archivo a entregar llamado "M2U4-4-preguntas.txt". Recuerda identificar y ordenar cada pregunta seguida de su respuesta.

Encontrarás las preguntas entre el texto en cursiva: *PREGUNTA: ¿Cómo se llama el servicio de instancias de VMs de Google Cloud?*

### Tarea 1: Desplegar la topología de red
xxx

#### Crear las redes y subredes
- crear 3 redes custom, vpc-A - vpc-B - vpc-C
- comprobar que no hay solapamiento - calculadora de redes
- crear 1 subred por red, en regiones diferentes - rangos diferentes!
- crear fwr para permitir ssh externo, prioridad 100
- crear fwr para permitir todo tráfico interno, prioridad 10000

#### Crear las instancias de VM
- crear 1 vm por red/subred - vm-A1 - vm-B1 - vm-C1
- comprobar que no hay conectividad por ping

### Tarea 2: Emparejar las redes VPC
xxx

#### Emparejar VPC A y B
- crear emparejamiento
- comprobar rutas
- comprobar conectividad en ambas vías

#### Restringir tráfico
- desplegar 1 vm más en vpc A y B, vm-A2 y vm-B2
- explicar topología de fwr y tráfico permitido
- restringir tráfico en vpc b sólo a y desde vm-B1
- comprobar conectividad en ambas vías

### Tarea 3: Comprobar que el emparejamiento no es transitivo
xxx

#### Emparejar VPC B y C
- crear emparejamiento
- comprobar rutas
- comprobar conectividad entre vm-B1 y vm-C1 en ambas vías
- comprobar conectividad entre vm-A1 y vm-C1 en ambas vías

#### Emparejar VPC A y C
- crear emparejamiento
- comprobar rutas
- comprobar conectividad entre vm-A1 y vm-C1 en ambas vías

#### Detener el emparejamiento
- eliminar emparejamiento A a C
- comprobar rutas
- comprobar conectividad entre vm-A1 y vm-C1 en ambas vías

## Resumen de entregas
1. M2U4-4-preguntas.txt: Respuestas a todas las preguntas planteadas en el ejercicio.
1. [nombre de archivo]: descripción

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. paso1
1. paso2
