# Cloud VPN y Cloud Router
Unidad M2U4

## ¿Qué vamos a hacer?
1. Crear una conexión por VPN entre 2 redes con Cloud VPN con enrutamiento estático.
1. Crear una conexión por VPN entre 2 redes con Cloud VPN con enrutamiento dinámico con Cloud Router.

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

Para responder a todas las preguntas del ejercicio de forma agrupada, puedes crear antes de empezar un archivo a entregar llamado "M2U4-5-preguntas.txt". Recuerda identificar y ordenar cada pregunta seguida de su respuesta.

Encontrarás las preguntas entre el texto en cursiva: *PREGUNTA: ¿Cómo se llama el servicio de instancias de VMs de Google Cloud?*

### Tarea 1: Desplegar la topología de red
xxx

- crear vpc-A y vpc-B, con 1 subred cada en regiones diferentes, rangos diferentes
- crear 2 vms, una por vpc
- fwr ssh en ambas redes, prioridad 100
- fwr tráfico interno en ambas redes, prioridad 1000

### Tarea 2: Conexión VPN con enrutamiento estático
xxx

#### Crear los túneles VPN
- comprobar que no hay solapamiento, calculadora redes
- crear vpn estática en vpc-A y vpc-B
- comprobar rutas
- comprobar conectividad en ambas vías

#### Comprobar que el enrutamiento es estático
- crear 1 nueva subred en vpc-A
- crear 1 nueva vm en nueva subred
- comprobar rutas
- comprobar conectividad en ambas vías
- reiniciar tunel (?)
- comprobar conectividad en ambas vías
- eliminar vpn
- comprobar conectividad perdida en ambas vías

### Tarea 3: Conexión VPN con enrutamiento dinámico
- crear vpn HA en vpc-A y vpc-B, creando cloud router
- comprobar rutas
- comprobar conectividad en ambas vías

#### Comprobar que el enrutamiento es dinámico
- crear 1 nueva subred en vpc-B
- crear 1 nueva vm en nueva subred
- comprobar rutas
- comprobar conectividad en ambas vías

## Resumen de entregas
1. M2U4-5-preguntas.txt: Respuestas a todas las preguntas planteadas en el ejercicio.
1. [nombre de archivo]: descripción

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. paso1
1. paso2
