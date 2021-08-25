# Reglas de cortafuegos
Unidad M2U4

## ¿Qué vamos a hacer?
1. Asignar reglas de cortafuegos con etiquetas de red y según cuentas de servicio.
1. Comprobar el orden de prioridad de aplicación de las reglas de cortafuegos.
1. Ver cómo debuguear problemas de conectividad debidos a reglas de cortafuegos.

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

Para responder a todas las preguntas del ejercicio de forma agrupada, puedes crear antes de empezar un archivo a entregar llamado "M2U4-2-preguntas.txt". Recuerda identificar y ordenar cada pregunta seguida de su respuesta.

Encontrarás las preguntas entre el texto en cursiva: *PREGUNTA: ¿Cómo se llama el servicio de instancias de VMs de Google Cloud?*

### Tarea 1: Creación de la topología de red
xxx

#### Crear la red y subredes
- explicar topología
- crear una red custom
- crear 2 subredes

#### Crear las instancias de VM
- topología: 2 FE y 2 BE, parejas en subred, conectividad de FE a BE y de exterior a FE, sólo entre FE/BE emparejadas
- crear las SA
- crear las instancias (FE externas, BE internas) y asignar SA
- crear 1 cliente interno extra

### Tarea 2: Asignación de reglas de cortafuegos
xxx

#### Asignar reglas de cortafuegos a todas las instancias
- comprobar conexión SSH y ping
- habilitar conexión SSH y ping desde todo origen con FR a todas instancias
- comprobar conexión SSH a todas

#### Comprobar las reglas de cortafuegos implícitas
- comprobar que ping está deshabilitado a instancia
- ssh a instancia y comprobar que hay ping al exterior

#### Asignar reglas de cortafuegos con etiquetas de red
- comprobar conectividad a y entre instancias
- crear FRs con network tag y asignarlas - prioridad 1000
- comprobar conectividad a y entre instancias

- acceder a instancia y cambiar fwr, p. ej tráfico externo a FE
- comprobar conectividad
- reasignar fwr con letra cambiada en net tag
- comprobar conectividad
- reasignar fwr correctamente
- comproar conectividad

#### Asignar reglas de cortafuegos con cuentas de servicio
- eliminar reglas de cortafuegos de net tag (no ssh)
- recrear con SA - prioridad 1000
- acceder a detalles de instancia y comprobar que no hay net tags - no hay posibilidad de problemas

#### Test de conectividad
- crear test de conectividad: ssh y ping sí, otro tráfico no, tráfico permitido sí, tráfico no permitido no
- analizar tests

### Tarea 3: Resolución de problemas de conectividad por reglas de cortafuegos
xxx

- asignar regla a todas para bloquear todo tráfico externo para bloquear ping - mayor prioridad que SSH
- comprobar que no se puede SSH a ninguna vm

#### Ver qué reglas se aplican a la instancia
- ver reglas aplicadas a la instancia
- ver análisis de tráfico permitido o no

#### Habilitar registros para las reglas de cortafuegos
- habilitar registros
- analizar registros
- deshabilitar registros

#### Test de conectividad
- crear test de conectividad: ssh sí, otro tráfico no incluyendo ping, tráfico permitido sí, tráfico no permitido no
- analizar tests

#### Deshabilitar reglas de cortafuegos
- deshabilitar ssh
- repetir test conectividad
- habilitar ssh
- editar prioridad
- repetir tests conectividad
- comprobar conectividad ssh

## Resumen de entregas
1. M2U4-2-preguntas.txt: Respuestas a todas las preguntas planteadas en el ejercicio.
1. [nombre de archivo]: descripción

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. paso1
1. paso2
