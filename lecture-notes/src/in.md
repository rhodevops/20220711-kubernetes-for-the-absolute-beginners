# Introducción

## Kubernetes series

1. Kubernetes for the absolute beginners
    - Lab environment
    - Pre-requisites - YAML
    - PODs, Deployments
    - Networking basics
    - Services
    - Micro-Services Arquitecture
    - Demos
    - Coding exercices

2. Kubernetes for administrators
    - HA Deployment
    - Kuberntes Scheduler
    - Logging/Monitoring
    - Application Lifecycle
    - Maintence
    - Secutity
    - Troubleshooting
    - Core Concepts
    - Demos
    - Coding exercices

3. Kubernetes for developers
    - Core concepts
    - ConfigMaps, Secrets
    - Multi-Container Pods
    - Readiness & 
    - Logging/Monitoring
    - Pod Design
    - Jobs
    - Services & Networking
    - Demos
    - Coding exercices


# ¿Qué es kubernetes?

Es un sistema de orquestación de contenedores.

## ¿Qué es un contenedor?

Problemas que resuelve docker:

- El infierno de las dependencias
- El infierno de las librerias
- Manejo de distintias versiones

Lo contenedores:

- Es un entorno aislado
- Tiene sus propios procesos/servicios, interfaces de red, mounts
- Comparte con el resto de contenedores el kernel del SO
- Tipos de contenedores: LXC, LXD, LXCFS
- Docker utiliza LXC containers (funcionan en linux)

Diferencias entre contendores y máquinas virtuales:

- Los contenedores son mucho más ligeros (MB vs GB)
- Cada vm tiene su propio SO
- Las vm están completamente aisladas
- Se pueden tener vm con distintos SO en el mismo hipervisor

Docker Hub:

- Registro de imágenes de docker

Contenedores vs imágenes:

- Una imagen es una plantilla o paquete que se utiliza para generar uno o más contenedores
- Los operadores utilizan las imágenes que crean los desarrolladores para desplegar las aplicaciones

## ¿Qué es la orquestación de contenedores?

Conceptos:

- Conexión entre contenedores
- Escalabilidad
- Balanceador de cargas

Algunas tecnologías de orquestación:

- Docker Swarm
- Kubernetes
- MESOS

## Arquictura de Kubernetes

### Nodos

Un `nodo` o `minion` es una máquina física o virtual donde se instala kubernetes. Un nodo es una `worker machine` y es donde Kubernetes lanza los contendores. Se necesita tener más de un nodo, por si la aplicación cae.

## Cluster

Un clúster es un conjunto de nodos agrupados. Los nodos se reparten las cargas de trabajo.

## Master

El `master` es el nodo encargado de la orquestación.

## Componentes de Kubernetes

### API Server

El `API Server` actúa como el front-end de Kubernetes.

### etcd

`etcd` es una "distributed reliable key-value store", es decir, es la base de datos de Kubernetes. Guarda toda la información relacionado con la gestión del cluster.

### Scheduller

El `Scheduler` es el encargado de repartir los contenedores o las cargas a través de múltiples nodos. Crea contenedores y los asigna a los nodos.

### Controller

Los `controllers` se encargan de avisar y responder ante las caídas de los nodos, los contenedores o los end points.

### Container Runtime

El `container runtime` es el software que existe por debajo para ejecutar los contenedores. No tiene por qué ser `Docker` (ver `rkt`, `CRI-O`).

### Kubelet

`kubelet` es el agente que se ejecuta en cada nodo. Es reposable de asegurar que los contenedores se ejecutan en los nodos del modo esperado.

## Master vs worker nodes

Hemos visto dos tipos de servidores:

- Master --> `kube-apiserver`, `etcd`, `controller`, `scheduler`
- Worker --> `kubelet`, `container runtime`

## kubectl

`kubectl` es la herramienta de línea de comandos que se utiliza para controlar y gestionar el cluster de kubernetes. Se utiliza para:

- Desplegar y gestionar las aplicaciones del clúster
- Consultar información del clúster o el estado de los nodos
- Listar los nodos del clúster

# Configuración de Kubernetes

## Algunas opciones para configurar kubernetes

`minikube` es la opción más sencilla para comenzar a aprender kubernetes. 

Características:

- Se ejecuta en un entorno local
- El cluster dispone de un solo nodo

`kubeadmin` es la opción más sencilla para comenzar a aprender kubernetes. 

Características:

- Se ejecuta en un entorno local
- Cluster multi nodo

## Demo. Minikube

### Configurar kubernetes con minikube

Configurar kubernetes con minikube. Dos opciones posibles:

1. En un entorno `linux`, instalar minikube en un contenedor `docker`.
2. Instalar minikube en `Virtual Box`.

Notas:

- Si máquina es `linux`, se recomienda la primera opción.
- Si la máquina es `windows`, se recomienda la segunda opción.

La opción `1` está documentada en el curso `20220211-docker-jenkins-kubernetes-git`, con la salvedad que el linux está dentro de un Virtual Box. La opción `2` es más comoda en una maquina Windows


### Instalar kubectl y minikube en Windows

Requisitos:

- Consultar la documentación oficial de kubernetes y de minikube.
- Minikube puede desplegarse como una VM, un container o bare-metal. Consultar los [drivers](https://minikube.sigs.k8s.io/docs/drivers/) de minikube.
- En este caso, se requiere `Virtual Box` instalado.

Pasos:

1. [Instalar y configurar kubectl en Windows](https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/)
    - Hay que crear el directorio, p.e: `C:\kubectl` y lanzar el `curl` (desde git bash)
    - Opcional, validar el binario contra el `cheksum file` .
    - Añadir al `PATH` la ruta donde está el binario (comprobar en ps con `$env:Path`)

2. [Instalar y configurar minikube en Windows](https://minikube.sigs.k8s.io/docs/start/)
    - El comando de la documentación crea el directorio `C:\minikube` . Podemos crear dentro la carpeta `bin` para meter el binario dentro.
    - Añadir al `PATH` la ruta donde está el binario (comprobar en ps con `$env:Path`) o lanzar el comando de la documentación.
    - Iniciar el cluster indicando el driver deseado: `minikube start --driver=virtualbox` 

Notas:

- El último comando mencionado, inicia el cluster de minikube, creando una vm en Virtual Box.
- Es importante instalar `kubectl` antes que minikube.






















