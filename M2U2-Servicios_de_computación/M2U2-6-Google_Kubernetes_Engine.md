# Google Kubernetes Engine
Unidad M2U2 - Ejercicio 6

## ¿Qué vamos a hacer?
1. Crear un clúster de Kubernetes con Google Kubernetes Engine.
1. Desplegar una aplicación sobre el clúster de GKE.
1. Comprobar cómo escala el clúster de GKE en función de la carga.

### Antes de empezar
1. Accede a la consola web: [Google Cloud Console](https://console.cloud.google.com).
1. Inicia sesión con tu cuenta de usuario si es necesario.
1. Una vez en la consola, comprueba que estás trabajando en tu proyecto. Puedes comprobarlo en la tarjeta de información del proyecto en el panel de control de la página principal o en el menú desplegable de selección de proyecto a la izquierda de la barra superior azul (junto a los 3 hexágonos en blanco).
1. Puedes dividir tu escritorio en 2 ventanas horizontales, una con la consola y otra con las instrucciones.
1. Si durante el ejericio debes acceder a Cloud Shell, puedes activarlo en el icono ">_" a la derecha en la barra superior azul. Puedes utilizar la terminal en la misma ventana de la consola o maximizarla y abrirla en una ventana nueva. También puedes acceder directamente a la terminal en [shell.cloud.google.com](https://shell.cloud.google.com) y al IDE en [ide.cloud.google.com](https://ide.cloud.google.com/).

*Nota: No hay preguntas para este ejercicio.*

### Tarea 1: Crear un clúster de Kubernetes y desplegar una aplicación web
Vamos a comenzar creando un clúster de Kubernetes, desplegando algunas aplicaciones y comprobando su autoescalado.

Antes de comenzar, habilita la API de Gooogle Kubernetes Engine:
1. En la consola, navega a **API y servicios > Biblioteca**.
1. Busca la API de Google Kubernetes Engine y habilítala o comprueba que ya está habilitada.

#### Crear un clúster de GKE
Vamos a desplegar un clúster de Kubernetes con GKE utilizando la consola:

1. En la consola, navega a **Kubernetes Engine > Clústeres** y pulsa **Crear**.
1. Selecciona **GKE Standard** como modo de despliegue.
1. En la página de creación del clúster, explora todas las opciones para descubrir qué características están disponibles.
1. Crea un clúster con las siguientes características:
    - Tipo de ubicación: zonal.
    - Zona: europe-west1-b.
    - Grupos de nodos: 1 grupo de nodos de 3-5 nodos e2-medium con autoescalado.

Cuando creamos un clúster desde Cloud SDK, las credenciales de la herramienta `kubectl` quedan automáticamente configuradas. Sin embargo, si lo hacemos desde la consola, debemos configurarlas:
1. En la consola, en **Kubernetes Engine > Clústeres**, busca la columna de **Acciones** para tu clúster y, en el menú de 3 puntos verticales, pulsa **Conectar**.
1. Te mostrará un comando que puedes ejecutar en Cloud Shell pulsando el botón **Ejectuar en Cloud Shell**.
1. Una vez configurado `kubectl`, podemos comprobarlo con `kubectl cluster-info` o `kubectl get all --all-namespaces`.

De esta forma hemos creado un clúster de Kubernetes y configurado `kubectl` con las credenciales de acceso al mismo.

*ENTREGABLES:*
1. M2U2-6-tarea_1-archivo_1-captura_1.jpg: Captura de pantalla de la página de detalles del clúster, mostrando toda su información.
1. M2U2-6-tarea_1-archivo_2-captura_2.jpg: Captura de pantalla de la respuesta del comando `kubectl cluster-info`.

#### Desplegar una aplicación web
Ahora vamos a desplegar una aplicación web stateless en el clúster:

1. Crea un archivo llamado `my-deployment-50001.yaml` con el siguiente manifiesto de Kubernetes:
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment-50001
spec:
  selector:
    matchLabels:
      app: products
      department: sales
  replicas: 3
  template:
    metadata:
      labels:
        app: products
        department: sales
    spec:
      containers:
      - name: hello
        image: "us-docker.pkg.dev/google-samples/containers/gke/hello-app:2.0"
        env:
        - name: "PORT"
          value: "50001"
```
1. Ahora crea el Despliegue de Kubernetes con la aplicación en el clúster con el comando `kubectl apply -f my-deployment-50001.yaml`.
1. Para verificar que la aplicación se ha desplegado correctamente, comprueba los Pods y el Despliegue con los comandos `kubectl get pods`, `kubectl get deployments` y `kubectl describe deployment my-deployment-50001`.

Una vez desplegada la aplicación, vamos a crear un Servicio de tipo balanceador de carga para poder acceder a la aplicación:
1. Crea un archivo llamado `my-lb-service.yaml` con el siguiente manifiesto de Kubernetes:
```
apiVersion: v1
kind: Service
metadata:
  name: my-lb-service
spec:
  type: LoadBalancer
  selector:
    app: products
    department: sales
  ports:
  - protocol: TCP
    port: 60000
    targetPort: 50001
```
1. Ahora crea el Servicio de Kubernetes desplegando el objeto con el comando `kubectl apply -f my-lb-service.yaml`.
1. Espera a que el balanceador de carga se cree y su IP externa esté disponible con el comando `kubectl get services --watch`, y cuando la IP externa está disponible cancélalo con `CTRL + C`.
1. Una vez disponible la IP externa, accede a la misma desde tu navegador por HTTP al puerto 60000: `http://IP_EXTERNA_LB:60000`.
1. Puedes comprobar que se ha creado un balanceador de carga de verdad en GCP navegando en la consola a la sección **Servicios de red > Balanceo de cargas**.

*ENTREGABLES:*
1. M2U2-6-tarea_1-archivo_3-captura_3.jpg: Captura de pantalla del navegador mostrando la aplicación y la URL del balanceador de carga.
1. M2U2-6-tarea_1-archivo_4-captura_4.jpg: Captura de pantalla de la página de detalles del balanceador de carga, mostrando toda su información.

#### Comprobar el autoescalado
Ahora vamos a habilitar el autoescalado horizontal de Pods y comprobar cómo autoescalan el número de réplicas de Pods y Nodos del clúster.

Primero vamos a habilitar el autoescalado con un HorizontalPodAutoescaler para el Despliegue:
1. Habilita el autoescalado del Despliegue en función de la CPU: `kubectl autoscale deployment my-deployment-50001my-deployment-50001 --min 3 --max -1 --cpu-percent 50`.
1. Comprueba la configuración del HorizontalPodAutoescaler con los comandos `kubectl get hpa` y `kubectl describe hpa NOMBRE_HPA` (puedes encontrar el `NOMBRE_HPA` en la respuesta de `kubectl get hpa`).
1. Comprueba el número actual de Pods y la configuración de autoescalado del Despliegue con los comandos `kubectl get pods`, `kubectl get deployments` y `kubectl describe deployment my-deployment-50001`.

Una vez habilitado el autoescalado, vamos a sobrecargar nuestra aplicación y ver cómo aumenta el número de Pods y Nodos:
1. Recupera la IP externa del balanceador de carga: `kubectl get services`.
1. Sobrecarga la aplicación con el comando `ab -n 500000 -c 1000 http://IP_EXTERNA:60000` y revisa el número de Pods y Nodos desplegados:
    1. Para incrementar la carga puedes modificar el valor de las flags `-n` (nº de peticiones total) y `-c` (nº de peticiones concurrentes) hasta que veas que se incrementa el número de Pods y Nodos.
    1. Si necesitas más capacidad, puedes ejecutar la sobrecarga desde una instancia de VM Linux creada sólo para ello, con un número alto de vCPUs para poder alcanzar su ancho de banda máximo.
    1. Puedes comprobar el nº de Pods con `kubectl get pods` y el de Nodos con `kubectl get nodes`.

De esta forma hemos podido habilitar el autoescalado de una aplicación y comprobarlo con una prueba de carga.

*ENTREGABLES:* M2U2-6-tarea_1-archivo_5-captura_5.jpg: Captura de pantalla del resultado de los comandos mostrando el número de Pods y Nodos, diferente al número mínimo.

#### Desplegar una aplicación stateful
Ahora vamos a desplegar una aplicación stateful que utilizará los discos persistentes de Compute Engine:

1. Crea un archivo llamado `my-statefulset.yaml` con el siguiente manifiesto de Kubernetes:
```
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: web
spec:
  serviceName: "nginx"
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: k8s.gcr.io/nginx-slim:0.8
        ports:
        - containerPort: 80
          name: web
        volumeMounts:
        - name: www
          mountPath: /usr/share/nginx/html
  volumeClaimTemplates:
  - metadata:
      name: www
    spec:
      accessModes: [ "ReadWriteOnce" ]
      resources:
        requests:
          storage: 1Gi
```
1. Ahora crea el StatefulSet con el comando `kubectl apply -f my-statefulset.yaml`.
1. Crea un archivo llamado `my-statefulset-service.yaml` con el siguiente manifiesto de Kubernetes:
```
apiVersion: v1
kind: Service
metadata:
  name: nginx
  labels:
    app: nginx
spec:
  ports:
  - port: 80
    name: web
  clusterIP: None
  selector:
    app: nginx
```
1. Ahora crea el Servicio de Kubernetes desplegando el objeto con el comando `kubectl apply -f my-statefulset-service.yaml`.
1. Para verificar que la aplicación stateful se ha desplegado correctamente, comprueba los Pods, el StatefulSet y el servicio con los comandos `kubectl get pods -l app=nginx`, `kubectl get statefulset`, `kubectl describe statefulset web` y `kubectl get services`.

De esta forma, hemos desplegado una aplicación stateful usando un StatefulSet de Kubernetes.

*ENTREGABLES:* M2U2-6-tarea_2-archivo_1-captura_1.jpg: Captura de pantalla del resultado de los comandos mostrando los Pods del StatefulSet y la descripción del StatefulSet.

## Resumen de entregas
1. M2U2-6-tarea_1-archivo_1-captura_1.jpg: Captura de pantalla de la página de detalles del clúster, mostrando toda su información.
1. M2U2-6-tarea_1-archivo_2-captura_2.jpg: Captura de pantalla de la respuesta del comando `kubectl cluster-info`.
1. M2U2-6-tarea_1-archivo_3-captura_3.jpg: Captura de pantalla del navegador mostrando la aplicación y la URL del balanceador de carga.
1. M2U2-6-tarea_1-archivo_4-captura_4.jpg: Captura de pantalla de la página de detalles del balanceador de carga, mostrando toda su información.
1. M2U2-6-tarea_1-archivo_5-captura_5.jpg: Captura de pantalla del resultado de los comandos mostrando el número de Pods y Nodos, diferente al número mínimo.
1. M2U2-6-tarea_2-archivo_1-captura_1.jpg: Captura de pantalla del resultado de los comandos mostrando los Pods del StatefulSet y la descripción del StatefulSet.

## Limpiar recursos
Sigue las siguientes instrucciones con atención para limpiar los recursos y configuración utilizada en tu proyecto. De esta forma evitarás especialmente costes continuados y problemas en siguientes ejercicios.

1. En la consola web, elimina el clúster de GKE.
1. (Opcional) Elimina los archivos de manifiestos locales.
