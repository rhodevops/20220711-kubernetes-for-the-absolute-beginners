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

Los datos representados en un YAML siguen la estructura `clave` - `valor` de los mapas o diccionarios. El valor de una clave pueden ser un string, una lista u otro diccionario.

En este ejemplo, los valores son strings:

```yaml
Fruit: Apple
Vegetable: Carrot
Liquid: Water
Meat: Chiken
```

### Array/Lists

Los guiones `-` anteceden los elementos que forman parte de una lista. 

En el siguiente YAML se representa una lista de frutas y otra de verduras. El orden importa, el tomata es el tercer elemento de la lista de verduras:

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

El valor de `Banana` es un diccionario.

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

## Lista de diccionarios

Es bastante habitual que un YAML contenga una lista de diccionarios:

```yaml
- Date: 20220710
  Activity: Run
  Time: 3000
- Date: 20220712
  Activity: Swim
  Time: 2500
```

# Conceptos de kubernetes: Pods, ReplicaSets, Deployments

## Pods con YAML

Kubernetes utiliza YAMLs como inputs para crear objetos como pods, replicas, deployments services, etc.

Todos los archivos de definición de Kubernetes siguien una estructura basada en cuatro campos o propieades de nivel superior, llamado en inglés `top level` o `root level properties`

```yaml
# kubernetes-objetc.yaml
apiVersion:
kind:
metadata:

spec:
```

Tenemos:

- `apiVersion`
- `Kind` tipo de objeto de kubernetes
- `metadata`
- `spec`

Por ejemplo: 

```yaml
# pod-definition.yaml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    app: myapp
    type: front-end

spec:
  containers:
    - name: nginx-container
      image: nginx
```

Notas:

- `apiVersion` y `Kind` son strings
- `metadata` y `spec` son diccionarios
- `name` y `labels` son childrens de `metadata`
- `metadata.labels` es un diccionario
- `spec.containers` es una lista/array (de diccionarios). El primer (y único) item es un diccionario con dos propiedades, `name` e `image`.

La razón de que `spec.containers` sea un array es que los pods pueden contener más de un contenedor.

Para crear el pod utilizando los datos del YAML se ejecuta:

```bash
kubectl create -f pod-definition.yaml
```

o

```bash
kubectl apply -f pod-definition.yaml
```

Normalmente, el segundo comando se ejecuta sobre objetos que ya se han creado y se quieren aplicar los nuevos cambios de un archivo YAML modificado.

## Demo. Pods with YAML

Directorio: `./workspace/demo.pods-with-yaml`

Objetivos:

- Crear un pod de un nginx utilizando un manifiesto YAML

## Tips & Tricks. Desarrollar manifiestos de Kubernetes con VS Code 

Utilizar la extensión `YAML` de `Red Hat` con la configuración siguiente:

```json
"yaml.schemas": {
  
  "kubernetes": "*.yaml*"

}
```

## Lab. Pods with YAML

Algunos comando útiles:

```bash
kubectl get pods
kubectl describe pod <pod name> | grep -i image
kubectl get pods -o wide #muestra nodos
```

## Replication Controller y Replica Sets

### Controllers

Los `controllers` son los cerebros de kubernetes. Se encargan de monitorizar los objetos de kubernetes y dar respuestas acordes. En esta sección vamos a ver, en particular, el `ReplicationController`.

### Replicas

Antes de entrar en los detalles del ReplicationController, necesitamos hablar de un concepto fundamental en kubernetes. ¿Qué es una `replica` y para que sirve? Imaginemos que por algun motivo nuesta apicación se rompe. Para asegurarnos que los usuarios pueden acceder a la aplicación, hay que tener más de una instancia o pod ejecutandose al mismo tiempo. Estos pods son iguales, es decir, son replicas.  

### Replication Controller

El `ReplicationController` es el controlador que se encarga de que siempre exista un número deseado de pods ejecutándose al mismo tiempo.

El Replication Controller también es responsable del **balanceado de carga** y del **escalado** de la aplicación:

- Si la demanda aumenta, se encarga de desplegar nuevos pods para balancear la carga de trabajo sobre los mismos.
- El balanceo de carga también se aplica sobre los pods de otros nodos dentro del clúster.

### Replica Set vs Replication Controller

El `ReplicaSet` es la nueva tecnología recomnedada para configurar las réplicas. El objetivo que persigue es el mismo que el del Replication Controller.

Hay pequeñas diferencias en la forma de trabajo de ambos tecnologías.

### Definir y crear un Replication Controller

Crear el archivo `rc-definition.yaml`:

```yaml
# rc-definition.yaml
apiVersion: v1
kind: ReplicationController
metadata:
  name: myapp-rc
  labels:
    app: myapp
    type: front-end

spec:
  template:
    metadata:
      name: myapp-pod
      labels:
        app: myapp
        type: front-end
    spec:
      containers:
        - name: nginx-container
          image: nginx

  replicas: 3
```

Observar que `spec.template` es un diccionario que contiene las propiedades que definen el objeto de k8s `Pod` (salvo las propiedades `kind` y `apiVersion`)

Para crear el objeto de k8s, se ejecuta

```bash
kubectl create -f rc-definition.yaml
```

Otros comandos:

```bash
kubectl get replicationcontroller
kubectl get pods
```

### Definir y crear un Replica Set

Crear el archivo `rc-definition.yaml`:

```yaml
# replicaset-definition.yaml
apiVersion: apps/v1
kind: ReplicationSet
metadata:
  name: myapp-replicaset
  labels:
    app: myapp
    type: front-end

spec:
  template:
    metadata:
      name: myapp-pod
      labels:
        app: myapp
        type: front-end
    spec:
      containers:
        - name: nginx-container
          image: nginx

  replicas: 3
  selector: 
    matchLabels:
      type: front-end
```

Observar que la **gran diferencia** con el ReplicationController es que el ReplicaSet requiere la definición de la propiedad `spec.selector`, que se encarga de indentificar los pods que están bajo el "paraguas" del ReplicaSet. 

Un momento, ¿por qué es necesario hacer esta identificación si ya hemos definido el pod en el YAml? El motivo es que un ReplicaSet puede gestionar pods que no han sido creados en el archivo de definición del ReplicaSet.

Por cierto, la propiedad `spec.selector` también está disponible en un ReplicationController, la diferencia es que en este caso es opcional. 

La propiedad `selector.matchLabels` se encarga de vincular el label especificado con el label del pod.

```bash
kubectl create -f replicaset-definition.yaml
```

Otros comandos:

```bash
kubectl get replicaset
kubectl get pods
```

Para borrar el ReplicaSet (también borra todos los pods subyacentes) se ejecuta

```bash
kubectl delete replicaset myapp-replicaset
```

### Labels y Selectors

El ReplicaSet es un proceso que monitoriza los pods y los vuelve a crear es caso de que alguno de ellos se rompa.

El `selector` de un ReplicaSet es la propiedad que permite identificar los pods que van a ser monitorizados. 

Aqui un ejemplo reducido:

```yaml
# replicaset-definition.yaml
[...]
spec:
  selector:
    matchLabels:
      tier: front-end
```

```yaml
# pod-definition.yaml
[...]
metadata:
  labels:
    tier: front-end
```

Este mismo concepto se emplea en otros sitios de Kubernetes.

### Escenario de ejemplo

Tenemos tres replicas de un pod (`tier: front-end`) ejecutándose en un cluster de k8s y queremos crear un ReplicaSet para que monitorice esos pods y los vuelva a crear en caso de caídas.

¿Es necesario en este escenario añadir el `template` de definición de los pods que ya están creados? Si, es necesario. La razón es que cuando alguno de ellos caíga, el ReplicaSet necesita crear el pod caído.

### Escalado del ReplicaSet

Tenemos el siguiente archivo de definición de un ReplicaSet. 

```yaml
# replicaset-definition.yaml
apiVersion: apps/v1
kind: ReplicationSet
metadata:
  name: myapp-replicaset
  labels:
    app: myapp
    type: front-end

spec:
  template:
    metadata:
      name: myapp-pod
      labels:
        app: myapp
        type: front-end
    spec:
      containers:
        - name: nginx-container
          image: nginx

  replicas: 3
  selector: 
    matchLabels:
      type: front-end
```

Queremos escalar el número de replicas de 3 a 6. Hay varias opciones para ello:

1. Editar el YAML y ejecutar `kubectl replace -f replicaset-definition.yaml`
2. Ejecutar `kubectl scale --replica=6 -f replicaset-definition.yaml`
3. Ejecutar `kubectl scale --replica=6 replicaset myapp-replicaset` donde se ha pasado el `metadata.name` del ReplicaSet

El problema de la opción 3 es que el valor no se modifica de forma automática en el YAML.

Hay otras opciones avanzadas de escalado automático basado en la carga.

## Demo. Replica Sets

Directorio: `./workspace/demo.replicasets`

Objetivos:

- Levantar tres pods de un nginx a través de un ReplicaSet definido en un YAML.
- Eliminar un pod y observar que ocurre.
- Intentar levantar una cuarto pod (replica de los anteriores) utilizando  un manifiesto YAML de tipo `kind: Pod` (debe definirse con el label del ReplicaSet) y observar que ocurre.
- Escalar el número de replicas de 3 a 4 (con `kubectl edit replicaset <name>`)
- Escalar el número de replicas de 4 a 2 (con `kubectl scale replicaset <name> --replicas=<number>`)

Observar cuando se haga, que el comando `kubectl scale replicaset <name> --replicas=<number>` modifica el valor `spec.replicas` del archivo temporal correspondiente.

### Editar un ReplicaSet

El siguiente comando permite editar un ReplicaSet:

```bash
kubectl edit replicaset myapp-replicaset
```

donde `myapp-replicaset` es el nombre del ReplicaSet.

Se abrirá en el editor por defecto un archivo temporal que k8s crea en la memoría para permitir la edición de un objeto de k8s existente. En este archivo es donde sustuímos el valor 3 de `spec.replicas` por el valor 4 para hacer el escalado.

Tras editar el archivo:

- Si hemos escalado replicas, no hay que hacer nada más
- Si hemos hecho otros cambios, p.e. modificar la imagen de los contenedores, hay que borrar los pods antiguos para que se generen los nuevos pods actualizados.

Puede ser util cuando:

- No conocemos la ruta de YAML
- YAML creado por otra persona


## Lab. Replica Sets

Algunos comando útiles:

```bash
kubectl get na
kubectl get rs
kubectl get po
kubectl get rs -o wide #muestra containers, images, selector
kubectl describe rs
kubectl edit replicaset <name>
kubectl scale replicaset <name> --replicas=2
```

donde 

- `na` es `namespaces`
- `rs` es `replicasets`
- `po` es `pods`

## Deployments

### ¿Qué es un Deployment?

Hasta ahora hemos visto los conceptos de contenedor, Pod y ReplicaSet. Los contenedores están encapsulados en Pods, la unidad mínima de despliegue de k8s, y a su vez los ReplicaSet permiten desplegar el número deseado de pods como instancias/replicas de una misma aplicación.

En un nivel jerarquico superior a los elementos anteriores se sitúan los Deployments. En otras cosas, este objeto de k8s viene a resolver las siguientes funciones:

- Hacer un upgrade sólamente de algunas de las instancias desplegadas. Este tipo de upgrade se denomina `rolling updates` y busca un menor impacto en el acceso de los usuarios.
- Hacer `rolling back` de los cambios de una nueva versión.
- Hacer varios cambios a la vez, "haciendo una pausa" en el entorno.

Un `Deployment` no da la capacidad de actualizar sin problemas las instancias subyacentes de la aplicación mediante `rolling updates` (actualizaciones continuas), deshaciendo cambios y deteniendo y volviendo a reanudar cambios cuando se requiera.

### Definir y crear un Deployment

El manifiesto YAML de un Deployment es muy parecido al manifiesto de un ReplicaSet. Crear el archivo `deployment-definition.yaml`:

```yaml
# deployment-definition.yaml
apiVersion: apps/v1
kind: ReplicationSet
metadata:
  name: myapp-replicaset
  labels:
    app: myapp
    type: front-end

spec:
  template:
    metadata:
      name: myapp-pod
      labels:
        app: myapp
        type: front-end
    spec:
      containers:
        - name: nginx-container
          image: nginx

  replicas: 3
  selector: 
    matchLabels:
      type: front-end
```

y ejecutar

```bash
kubectl create -f deployment-definition.yaml
```

¿Qué ocurre?

- El Deployment crea automáticamente un ReplicaSet
- El ReplicaSet crea los pods

Otros comandos:

```bash
kubectl get deployments
kubectl get replicasets
kubectl get pods
kubectl get all
```

La diferencia fundamental es que el Deployment, además de crear un ReplicaSet, crear el objeto de k8s del mismo nombre.

## Demo. Deployments

Directorio: `./workspace/demo.deployments`

Objetivos:

- Levantar tres pods de un nginx a través de un Deployment definido en un YAML.

## Lab. Deployments

Muy similar al laboratorio de Replica Sets.

## Deployments. Update y Rollback

### Rollout. Control de versiones de los deployments

Cuando se crea un Deployment, se genera un `rollout`. Y cada nuevo rollout crea una nueva revisión del Deployment. Es decir, los rollout mantienen un control de versiones del Deployment. Un rollout vienen dado por algun cambio:

- Nueva verión de la aplicación.
- Nueva verión del contenedor Docker.
- Actualizar labels.
- Actualizar número de réplicas.

Los rollout permiten hacer updates con la posibilidad de revertir los cambios mediante un rollback.

El siguiente comando

```bash
kubectl rollout status <deployment name>
kubectl rollout history <deployment name>
```

permite consultar el estado e historial de rollouts de un Deployment.

### Deployment Strategy

Hay dos tipos de estrategía:

- `Recreate` Destruir todo y crear todo
- `Rolling Update` (default) Destruir y crear uno a uno

### Upgrades

Una opción para hacer el upgrade es cambiar el manifiesto YAMl del Deployment y ejecutar:

```bash
kubectl apply -f <deployment name>
```

El proceso de upgrade es el siguiente:

- Tenemos un Deployment que ha creado el ReplicaSet `rs1`.
- Al hacer el `apply`, k8s crear un nuevo ReplicaSet `rs2`.
- Cuando se levanta un pod de `rs2`, se elimina un pod de `rs1`.

El proceso anterior se puede observar con el comando `kubectl ger replicasets`.

Se puede utilizar el siguiente comando para actualizar la versión de la imagen del contenedor:

```bash
kubectl set image <deployment name> <image name>=<image name>:<nuevo tag>
```

Investigar si es buena opcion actualizar utilizando el comando:

```bash
kubectl edit deployments.apps <deployment name>
```

`¡Cuidado!` El yaml original del deployment no se actualiza. Si cambiamos el valor del número de replicas de 3 a 6 en el archivo temporal del deployment, el yaml del manifiesto original sigue teniendo el valor 3 en el campo `spec.replicas`. 

### Rollbacks

Para hacer un rollback se ejecuta

```bash
kubectl rollout undo <deployment name>
```

El proceso anterior se puede observar con el comando `kubectl ger replicasets`.

## Demo. Deployments. Update y Rollback

[PENDIENTE](todo)

Directorio: `./workspace/demo.deployments.update-and-rollbak`

Objetivos:

- Editar el manifiesto YAML del deployment de la demo anterior para escalar el número de réplicas de 3 a 6, haciendo un `kubectl apply`.
- Cambiar versiones de la imagen del deployment con el comando `kubectl set image` y tambíen con `kubectl edit`.
- Utilizar el flag `--record` en comandos de `kubectl`.
- Utilizar `kubectl edit` para cambiar la versión de la imagen a una que no exista y luego hacer un `kubectl rollout undo`. Observar que gracias a utilizar la strategía de despliegue `<rolling update>`, el usuario no se queda sin acceso a la aplicación.

## Lab. Rolling Updates and Rollbacks

### Algunos scripts

Mandar múltiples peticiones a la WebApp:

```bash
cat curl-test.sh 
for i in {1..35}; do
   kubectl exec --namespace=kube-public curl -- sh -c 'test=`wget -qO- -T 2  http://webapp-service.default.svc.cluster.local:8080/info 2>&1` && echo "$test OK" || echo "Failed"';
   echo ""
done
```

### Algunos comandos

Algunos comandos útiles:

```bash
kubectl rollout status <deployment name>
kubectl rollout history <deployment name>
```

# Networking en Kubernetes

## Cluster de un solo nodo

Punto de partida. Tenemos un cluster de un solo nodo. En un principio, existe un único pod con un contenedor:

- nodo <-- ip `192.168.1.2`
  - PNet  <-- ip `10.244.0.0`
  - pod 1 (aloja 1 contenedor) <-- ip `10.244.0.2`

Es fundamental tener claro lo siguiente. A diferencia que en Docker, donde la `IP` se asigna al contendor, en kubernetes la `IP` se asigna al pod, independientemente del número de contendores que contenga. 

En el caso de `minikube` (1 solo nodo, local), la `IP` del nodo se corresponde con la `IP` de la vm dentro del hypervisor. Como solo hay un nodo, se le llama `minikube node`. El ordenador del sistema tendra otra dirección `IP`.

Cuando se inicia Kubernetes, se crea una Private Network interna con la dirección IP `10.244.0.0` y todos los pods se conectan a ella. 

Cuando se despliegan múltiples pods, todos ellos tienen una `IP` diferente dentro de esta red:

- pod 1 <-- `10.244.0.2`
- pod 2 <-- `10.244.0.3`
- pod 3 <-- `10.244.0.4`

Aunque los pods anteriores pueden comunicarse a través de estas direcciones ip, esto no es una buena idea. Más adelante veremos otra forma más apropiada.

## Cluster de varios nodos

Punto de partida. Tenemos dos nodos. En un principio están aíslado y no forman parte de un clúster:

- nodo A <-- ip `192.168.1.2`
  - PNet  <-- ip `10.244.0.0`
  - pod 1 (aloja 1 contenedor) <-- ip `10.244.0.2`

- nodo B <-- ip `192.168.1.3`
  - PNet  <-- ip `10.244.0.0`
  - pod 1 (aloja 1 contenedor) <-- ip `10.244.0.2`

El primer problema a resolver es que las PNet internas de los nodos tienen las mismas direcciones. Respecto a la red de k8s, existen unos requisitos fundamentales. Algunos de estos son:

- Todos los pods y contenedores del clúster tienen que poder comunicarse entre ellos sin la configuración de un NAT.
- Todos los nodos del clúster tienen que poder comunicarse con los contenedores y todos los contenedores tienen que poder comunicarse con los nodos sin la configuración de un NAT.

`CNI` (Container Network Interface) es un proyecto de la `Cloud Native Computing Foundation`. Consiste en un conjunto de específicaciones y librerías para escribir plugins de configuración de interfaces de red en contenedores Linux.

Hay múltiples soluciones pre-construídas para Kubernetes:

- Cisco ACI (Application Centric Infrastructure)
- Cilium
- Flannel
- VMWare NSX-T
- Calico

Estas soluciones crean una Virtual Network que aloja a todos los nodos y pods y asigna direcciones IP únicas dentro del cluster, mediante técnicas básicas de `routing`:

- nodo A <-- ip `192.168.1.2`
  - PNet  <-- ip `10.244.0.0`
  - pod 1 (aloja 1 contenedor) <-- ip `10.244.0.2`

- nodo B <-- ip `192.168.1.3`
  - PNet  <-- ip `10.244.1.0`
  - pod 1 (aloja 1 contenedor) <-- ip `10.244.1.2`

# Services

Los `Services` de k8s permiten establecer la comunicación entre componentes internos y externos de la aplicación y conectar con los usuarios u otras aplicaciones.

## Services. Node Port

### Contexto y necesidad

Podemos pensar en una aplicación que tiene grupos de pods ejecutándose en varias partes:

- Un primer grupo de pods hace el servicio de carga del frontend a los usuarios.
- Un segundo grupo de pods ejecuta los procesos del backend.
- Un tercer grupo de pods hace la conexión con una base de datos externa.

Todas estas comunicaciones y acoplamientos se llevan a cabo mediante los `services`.

Vamos a imaginarnos el siguiente escenario. Tenemos un clúster de un nodo donde se ha desplegado un pod que ejecuta una página web:

```yaml
net: 
  ip: 192.168.1.0
  cluster:
    - nodo: 192.168.1.2
      pnet: 10.244.0.0
        - pod: 10.244.0.2
  laptop: 192.168.1.10
```

Hay que tener claro que:

- Desde el ordenador, no puedo hacer ping o acceder al puerto `10.244.0.2` (porque está en una red separada).
- Desde el nodo (ssh desde el ordenador), puedo hacer un curl a la página web.

```bash
# from laptop
> ssh 192.168.1.2
> curl http://10.244.0.2
hello world
```

Pero lo que deseamos es poder acceder a la página web desde el ordenador sin necesidad de hacer ssh al nodo del cluster. Para ello se necesita tener algo en el medio que nos ayude a mapear las peticiones desde el ordenador a través del nodo hasta el pod que ejecuta el contenedor web. Y aquí es donde entra en juego el `Service` de kubernetes.

Un `Service` es un objeto de k8s que permite servir la aplicación al exterior del clúster. Una forma de hacerlo es escuchar a través de un puerto (`30008`) situado en el nodo (`192.168.1.2`) y reenviar solicitudes en ese puerto al pod donde se ejecuta la aplicación web.

```bash
# from laptop
> curl http://192.168.1.2:30008
hello world
```

Este tipo de Service se denomina `NodePort` ya que el servicio escucha en un puerto del nodo y reenvía las peticiones a los pods. Pero no es el único tipo, existen otro tipos de Services:

- `NodePort`
- `ClusterIP`
- `LoadBalancer`

### Node Port

Tenemos el siguiente cluster donde se ejecuta una WebApp. Los términos `port` y `targetPort` se emplean desde el punto de vista del Service:

```yaml
cluster:
  service:
    clusterIP: 10.162.1.12
    port: 80
  nodos:
    - nodo: 192.168.1.2
      nodePort: 30008   
      pnet: 10.244.0.0
        - pod: 10.244.0.2
          targetPort: 80
```

El camino de la petición de un usuario `NodePort - Service - Port/TargetPort - Pod`.

El `Service` puede verse como un `virtual server` dentro del nodo, dentro del cluster. Tiene su propia dirección `IP` que se denomina `ClusterIP of the Service`.

El rango válido del `NodePort` va desde 30000 hasta 32767.

### Definición yaml del servicio NodePort

Partimos de un pod creado y definido con el siguiente yaml. Es importante tener en cuenta los `labels` ya que el manifiesto del NodePort utiliza un `spec.selector` para hacer el link al pod que corresponda:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels: 
    app: myapp
    type: front-end

spec:
  containers: 
    - name: nginx-container
      image: nginx
```

Para el NodePort, se crea el siguiente archivo `service-definition.ymal`. 

```yaml
apiVersion: v1
kind: Service
metadata:
  name: myapp-service

spec:
  type: NodePort
  ports:
    - targetPort: 80
      port: 80
      nodePort: 30008
  selector:
    app: myapp
    type: front-end
```

Se pueden tener múltiples mapeos de puertos utilizando un único Service.

Es importante aclarar que no importa que existan cientos de pods con otras WebApps ejecutándose en el puerto `80` ya que la técnica que se utiliza para hacer el link con los pods es la del `selector`.

El único puerto requerido es `port`. Los valores por defecto si no se especifican son los siguientes:

- `targetPort` el mismo que `port`
- `nodePort` "aleatorio" en el rango adecuado

Una vez que se ha codificado el archivo yaml, se utiliza la siguiente orden para crear el servivio:

```bash
kubectl create -f service-definition.yaml
```

Otros comandos:

```bash
kubectl get services
```

Ahora se puedo utilizar este puerto para acceder al servicio web usando un curl o un navegador web:

```bash
curl http://<nodo IP>:<nodePort>
```

### NodePort con múltiples pods en un solo nodo

En un entorno de producción, se tienen múltiples instancias de la aplicación web ejecutándose para tener alta disponibilidad y balancear la cargas. En este caso se tienen varios pods réplicas que ejecutan la WebApp.

Dado que el servicio `NodePort` utiliza la técnica del `selector` para seleccionar los pods, basta con utilizar un mismo label en todos los pods para que el servicio haga el link de los pods considerados. Además, el Service crea automáticamente un balanceo de carga que no tenemos que configurar.

Como curiosidad, el algoritmo utilizado en el balanceo es el siguiente:

 - Algorithm `Random`
 - SessionAffinity `Yes`

 ### NodePort con pods distribuidos en varios nodos

 En este caso tampoco hay que hacer ninguna configuración adicional, sino que k8s lo hacer por nosotros automáticamente.

 Podemos tener el siguiente escenario:

 ```yaml
cluster:
  service:
    clusterIP: 10.162.1.12
    port: 80
  nodos:
    - nodo: 192.168.1.2
      nodePort: 30008   
      pods:
        - pod: 10.244.0.2
          targetPort: 80
    - nodo: 192.168.1.3
      nodePort: 30008   
      pods:
        - pod: 10.244.0.3
          targetPort: 80
    - nodo: 192.168.1.4
      nodePort: 30008   
      pods:
        - pod: 10.244.0.4
          targetPort: 80
```

Para hacer el curl a la aplicación web se puede utilizar cualquier de la siguientes variantes:

```bash
curl http://192.168.1.2:30008
curl http://192.168.1.3:30008
curl http://192.168.1.4:30008
```

## Demo. Services. NodePort

Directorio: `./workspace/demo.services.nodeport`

Objetivos:

- Crear un servicio NodePort para los 6 pods de nginx que se levantaron con el deployment de la demo anterior.

Recordar utilizar correctamente la técnica del `selector` para hacer link a los pods del deployment.

Utilizando minikube, el siguiente comando nos devuelve la url del servicio que hemos creado:

```bash
minikube service myapp -service --url
```

donde `myapp` es el label utilizado en el `selector`