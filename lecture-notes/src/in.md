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

### Algunos comandos

Con el cluster iniciado, se utilizan los siguiente comandos:

```bash
minikube status
kubectl get pods -A
kubectl get pods
kubecetl cluster-info
```

Se despliega un `hello-minikube`. Consultar los `<flags>` no especificados de los siguiente comandos en la documentación oficial:

```
kubectl create deployment hello-miinilube <flag>
kubectl get pods
kubectl get deployments
kubectl get pods
kubectl expose deployment hello-miinilube --type=NodePort --pot=8080
kubectl get services
```

Si se elimina un pod, `kubectl delete pods <name>` , se vuelve a crear automáticamente. Para eliminar lo que se ha desplegado:

```bash
kubectl delete deployments <name>
kubectl delete services <name>
```

# Conceptos de kubernetes

## ¿Qué es un pod?

Un `pod` es el objeto más pequeño que kubernetes puede crear/desplegar.

Supongamos el siguiente punto de partida:

- Tenemos una aplicación desarrollada y construída en una imagen de docker disponible en un repo.
- Tenemos un cluster de kubernetes configurado.

Kubernetes no hace el despliegue directamente a los nodos worker, sino que encapsula los contenedores en un objeto de kubernetes llamado `pod`.

Un `pod` es una instancia unitaria (single instance) de una aplicación. Es el objeto más pequeño que kubernetes puede crear.

Imaginamos que tenemos una aplicación ejecutandose en un cluster de kubernetes de un solo nodo, en un único contenedor encapsulado en un pod. ¿Cómo se escala la aplicación?

Para escalar una aplicación en kubernetes, se necesita crear una nueva instancia de la aplicación, de modo que comparta la carga de trabajo con la primera. ¿Cómo se hace esto? Creando un nuevo pod junto a una nueva instancia de la misma aplicación. Así, tenemos dos intancias de la misma aplicación ejecutandose en dos pods separados dentro de un mismo nodo. 

Si hay que escalar más, también se puede añadir un nuevo nodo al cluster y crear nuevos pods dentro de este segundo nodo. 

Aunque la relación 1/1 entre pods y containers es muy frecuente, existen los llamados pods multi-conainer.

Un `multi-container pod` es un pod que contiene más de un contenedor. Es importante tener claro que los contenedores dentro de un pod no son nunca iguales (el escalado siempre se hace creando nuevos pods). 

En muchas ocasiones, estos contenedores "extra" del pod son `helper containers` o contenedores de ayuda, que prestan algun tipo de soporte al contenedor "principal" del pod. 

Los contenedores de un mismo pod:

- Nacen y mueren juntos.
- Se comunican directamente referenciándose como local host.
- Comparten el mismo espacio de red (network space).
- Tienen la misma dirección IP.
- Pueden compartir también el mismo espacio de almacenamiento.

### ¿Por qué son útiles los pods?

Estamos desarrollando un proceso o script que despliega una aplicación en un Docker host. El siguiente comando levanta un contendor a partir de una imagen de la aplicación:

```bash
docker run python-app
```

Para escalar la aplicación, desplegamos nuevas intancias de la aplicación levantando más contenedores:

```bash
docker run python-app
docker run python-app
docker run python-app
docker run python-app
```

Ahora vamoms a imaginar un escenario de futuro en el que la aplicación cambia e incrementa su complejidad. Ahora tenemos un `helper container` que da soporte a la aplicación y se encarga de procesar o extraer datos de otros sitios. Estos nuevos contenedores mantienen una relación 1/1 con los contenedores de la aplicación y además necesitan comunicarse directamente con ellos. 

```bash
docker run helper -link app1
docker run helper -link app2
docker run helper -link app3
docker run helper -link app4
```

Se necesita todo lo siguiente:

- Un mapa de conexión `appContainer: helpContainer` .
- Establecer una conexión de red (network connectivity).
- Crear volúmenes compartidos.
- Monitorizar el estado de los `appContainer` y, cuando muera, matar manualmente su `helpContainer` .
- Desplegar de manera conjunta el `appContainer` junto a su `helpContainer` .

El poder de Kubernetes es que hace todo lo anterior de manera automática.

## kubectl

El siguiente comando

```bash
kubnectl run nginx --image nginx
```

despliega un contenedor de docker creando un pod a partir de la imagen indicada. Se puede configurar el repositorio del que se extraen las imágenes.

Mediante el comando `kubectl get pods` , se lista los pods de un cluster. 

El ejemplo de `nginx` sirve para plantear algunos temas importante, que se verán más adelante:

- ¿Cómo se puede acceder al nginx web server?
- Acceso al web server para usuarios externos.
- Acceso interno desde el nodo.
- Networking & Services.
- Exponer un servicio.

## Demo. Pods

Con el cluster iniciado, se utilizan los siguiente comandos:

```bash
kubnectl run nginx --image nginx
kubnectl get pods
kubectl run nginx --image=nginx
kubectl run nginx --image=nginx
kubnectl describe pods
kubnectl get pods -o wide
```

# Introducción a los YAML

## Archivos YAML frente a otros formatos

Los archivos `YAML`, al igual que los `xml` y `json`, se utilizan para representar datos. En este ccontexto, datos de configuración.

Veamos la forma de representar un mismo conjunto de datos en los formatos mencionados:

```xml
<Servers>
    <Server>
        <name>Server1</name>
        <owner>John</owner>
        <created>12232012</created>
        <status>active</status>
    </Server>
</Servers>
```

```json
{
    Servers: [
        {
        name: Server1,
        owner: John,
        created: 12232012,
        status: active,
         }
    ]
}
```

```yaml
  Servers: 
    - name: Server1
      owner: John
      created: 12232012
      status: active
```

## Representación de tipos

En general, en muchos lenguajes de programación:

- Las llaves `{ }` encierran diccionarios. El orden no importa.
- Los corchete `[ ]` encierran listas. El orden importa.

### Key Value Pair

Los datos representados en un YAML siguen la estructura `clave` - `valor` de los mapas o diccionarios. Sin embargo, pueden tener anidados

Por ejemplo:

```yaml
Fruit: Apple
Vegetable: Carrot
Liquid: Water
Meat: Chiken
```

### Array/Lists

Los guiones `-` anteceden los elementos que forman parte de una lista. Las listas son objetos ordenados.

Una lista de frutas y una lista de verduras. La manzana es el primer elemento de la lista de frutas:

```yaml
Fruits: 
  - Apple
  - Orange
  - Banana

Vegetables: 
  - Carrot
  - Cauliflower
  - Tomato
```

### Dictionary/Map

Un archivo YAML suele tener varios niveles o anidaciones. El valor de una clave puede ser un diccionario.

El valor de `Banana` es el diccionario `{}`

```yaml
Banana:
  Calories: 105
  Fat: 0.4 g
  Carbs: 27 g

Grapes:
  Calories: 62
  Fat: 0.3 g
  Carbs: 16 g
```

## Espacios en un YAML

Los espacios son muy importantes en un `YAML` porque indican el nivel jerarquico de la propiedad. Por ejemplo, en el siguiente YAML, `Fat` y `Carbs` son propiedades de `Calories` que a su vez es una propiedad de `Banana`.

```yaml
Banana:
  Calories: 105
    Fat: 0.4 g
    Carbs: 27 g
```

## Diccionarios, listas y listas de diccionarios

























