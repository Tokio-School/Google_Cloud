# Discos y snapshots
Unidad M2U2 - Ejercicio 2

## ¿Qué vamos a hacer?
1. Trabajar con el disco de arranque de una instancia de VM.
1. Reasignar el disco de arranque de una instancia a otra instancia diferente.
1. Asignar discos adicionales a una instancia en modo lectura y escritura y sólo lectura a varias instancias a la vez.
1. Ampliar la reserva de almacenamiento de un disco en caliente.
1. Crear snapshots de discos de forma manual y automática y recrear instancias a partir de ellos.
1. Trabajar con SSD locales

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

Para responder a todas las preguntas del ejercicio de forma agrupada, puedes crear antes de empezar un archivo a entregar llamado "M2U2-2-preguntas.txt". Recuerda identificar y ordenar cada pregunta seguida de su respuesta.

Encontrarás las preguntas entre el texto en cursiva: *PREGUNTA: ¿Cómo se llama el servicio de instancias de VMs de Google Cloud?*

### Tarea 1: Discos de arranque
xxx

#### Crear una instancia de VM
- crear instancia, indicar no eliminar el disco al eliminar la instancia
- conecdtarse por ssh
- instalar servidor apache 2
- comprobar todo
- detener vm y desvincular disco
- crear segunda instancia, detenerla, actualizar disco de arranque

### Tarea 2: Discos persistentes adicionales
- en 2ª VM, crear disco PD adicional
- montar disco PD adicional
- escribir archivos
- ampliar espacio en caliente
- ampliar FS
- comprobar archivos
- desmontar PD adicional y montarlo de nuevo en modo RO
- crear otra VM 3ª y montar también dicho disco en modo RO

### Tarea 3: Snapshots
- en 2ª instancia, hacer snapshot de disco boot en caliente
- establecer snapshot automático/recurrente
- usar snapshot para crear 4ª instancia

### Tarea 4: Discos SSD locales
- crear 5ª VM, añadiendo ssd local y PD boot
- crear archivos en PD
- copiar info a local SSD
- modificar info en local SSD
- copiar de vuelta a PD
- apagar OS de instancia simulando una pérdida de servicio y comprobar pérdida de info en local ssd pero no en PD, no se puede levantar VM sin eliminar instancia

## Resumen de entregas
1. M2U2-2-preguntas.txt: Respuestas a todas las preguntas planteadas en el ejercicio.
1. [nombre de archivo]: descripción

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. paso1
1. paso2
