Project HISX2 2020-2021 Kubernetes

# Kubernetes

## Introducción

### ¿Qué es Kubernetes?

Kubernetes es una plataforma portable y extensible de código abierto para administrar cargas de trabajo y servicios. Kubernetes facilita la automatización y la configuración declarativa. Tiene un ecosistema grande y en rápido crecimiento. El soporte, las herramientas y los servicios para Kubernetes están ampliamente disponibles.

Google liberó el proyecto Kubernetes en el año 2014. Kubernetes se basa en la experiencia de Google corriendo aplicaciones en producción a gran escalapor década y media, junto a las mejores ideas y prácticas de la comunidad.

![](https://cdn.icon-icons.com/icons2/2699/PNG/512/kubernetes_logo_icon_168360.png)

### ¿Por qué utilizar contenedores?

Kubernetes ofrce un entorno de administración centrado en contenedores.

![](https://d33wubrfki0l68.cloudfront.net/e7b766e0175f30ae37f7e0e349b87cfe2034a1ae/3e391/images/docs/why_containers.svg)

La *Manera Antigua* de desplegar aplicaciones era instalarlas en un servidor usando el administrador de paquetes del sistema operativo. La desventaja era que los ejecutables, la configuración, las librerías y el ciclo de vida de todos estos componentes se entretejían unos a otros. Podíamos construir imágenes de máquina virtual inmutables para tener rollouts y rollbacks predecibles, pero las máquinas virtuales son pesadas y poco portables.

La *Manera Nueva* es desplegar contenedores basados en virtualización a nivel del sistema operativo, en vez del hardware. Estos contenedores están aislados entre ellos y con el servidor anfitrión: tienen sus propios sistemas de archivos, no ven los procesos de los demás y el uso de recursos puede ser limitado. Son más fáciles de construir que una máquina virtual, y porque no están acoplados a la infraestructura y sistema de archivos del anfitrión, pueden llevarse entre nubes y distribuciones de sistema operativo.

En resumen, los beneficios de usar contenedores incluyen:

- **Ágil creación y despliegue de aplicaciones:** Mayor facilidad y eficiencia al crear imágenes de contenedor en vez de máquinas virtuales.
- **Desarrollo, integración y despliegue continuo:** Permite que la imagen de contenedor se construya y despliegue de forma frecuente y confiable, facilitando los rollbacks pues la imagen es inmutable.
- **Separación de tareas entre Dev y Ops:** Puedes crear imágenes de contenedor al momento de compilar y no al desplegar, desacoplando la aplicación de la infraestructura.
- **Observabilidad:** No solamente se presenta la información y métricas del sistema operativo, sino la salud de la aplicación y otras señales.
- **Consistencia entre los entornos de desarrollo, pruebas y producción:** La aplicación funciona igual en un laptop y en la nube.
- **Portabilidad entre nubes y distribuciones:** Funciona en Ubuntu, RHEL, CoreOS, tu datacenter físico, Google Kubernetes Engine y todo lo demás.
- **Administración centrada en la aplicación:** Eleva el nivel de abstracción del sistema operativo y el hardware virtualizado a la aplicación que funciona en un sistema con recursos lógicos.
- **Microservicios distribuidos, elásticos, liberados y débilmente acoplados:** Las aplicaciones se separan en piezas pequeñas e independientes que pueden ser desplegadas y administradas de forma dinámica, y no como una aplicación monolítica que opera en una sola máquina de gran capacidad.
- **Aislamiento de recursos:** Hace el rendimiento de la aplicación más predecible.
- **Utilización de recursos:** Permite mayor eficiencia y densidad.

## Arquitectura de Kubernetes
Kubernetes distribuye los contenedores en **pods**, así estos pueden estar en varios **nodos**. A su vez, estos nodos forman un **clúster**, completando así la estructura que tiene Kubernetes.

### Pods
En Kubernetes los contenedores se agrupan en pods, por lo que todos los contenedores que se ejecuten en un pod lo harán en la misma máquina o host, ya que no se pueden separar.

Se agrupan en el mismo pod contenedores que usan y necesitan los mismos recursos. De esta forma da la sensación de que forman un host lógico dentro del clúster, por lo que es más fácil entender que el pod tendrá una dirección IP compartida por los contenedores que lo formarán. Además, todos los pods se alcanzarán entre ellos, dado que compartirán la misma red privada.

### Nodos
Las aplicaciones que se ejecuten en el clúster (conjunto de nodos), realmente se están ejecutando en los nodos, siendo los contenedores distribuidos por Kubernetes de manera automática.

Estos nodos pueden ser tanto un ordenador, como una máquina virtual o incluso una máquina en la nube. Estos están administrados por un componente llamado Kubelet, que veremos más adelante.

Los nodos pueden ser de dos tipos: los nodos trabajadores  y los nodos maestros. La diferencia que existe es que los nodos maestros Kubernetes los utiliza para labores de administración y planificación de los pods que se ejecutan en los nodos trabajadores del clúster. Esta administración y control se realiza a través de diferentes controladores que veremos más adelante.

Normalmente se dispone de un único nodo máster, pero en función de la carga de trabajo, podríamos disponer de varios nodos máster, haciendo así el sistema más resistente ante fallos.

### Clúster
Un clúster se forma por los dos elementos explicados anteriormente. Kubernetes coordina contenedores en un sistema formado por varios nodos, haciendo que todo funcione como una sola unidad regida por el nodo máster, logrando que las aplicaciones no estén vinculadas a una sola máquina o host. Esto evita que tengamos que instalar una aplicación directamente en una sola máquina, ya que la realizamos sobre el clúster.

Por último, la comunicación de los nodos con el máster y del usuario con el clúster, se hace a través de la API de Kubernetes.

## Componentes de Kubernetes
Para que la arquitectura descrita antes funcione correctamente, Kubernetes utiliza varios componentes en los diferentes elementos de su sistema.

### Componentes del máster

**Servidor de API**

Sirve para acceder a la API de Kubernetes en nuestro clúster. Es el front-end del plano de control de Kubernetes, es decir, es donde llegan las peticiones al clúster. Estas pueden llegar desde un nodo o desde una petición por parte del administrador del máster, y lo redirige a los componentes que correspondan.

Básicamente se encarga de preparar, validando y configurando, los datos de los objetos api que necesitamos para el clúster. También prepara la interfaz a través la cual los componentes del clúster interactúan con los demás componentes.

**Etcd**

Es una base de datos que guarda y almacena la configuraciñón del clúster, almacenando también información de los servicios que están disponibles.

Etcd estará replicando en todos los nodos máster del clúster para asegurar una alta disponibilidad de la información. Esta información es también usada por el API serer para que los servicios desplegados en los contenedores mantengan sus características

**Planificador (Scheduler)**

Asigna a los nuevos pods un nodo en el que ejecutarse, es decir, se encarga de repartir los recursos disponibles en el clúster, para la ejecución de los pods tras valorar los requisitos que necesitan para desarrollar su trabajo.

También es el responsable de monitorizar la utilización de recursos de cada host para asegurar que los pods no sobrepasen los recursos disponibles una vez ya estén en funcionamiento.

**Gestor de controladores (Controller-manager)**

Es un componente que, como su nombre indica, se encarga de gestionar los controladores de los nodos. Estos controladores son los siguientes:

* Controlador de réplicas (replication controller): Se encarga de controlar la ejecución correcta de réplicas deseadas para una aplicación.

* Controlador del nodo (node controller): Se encarga de controlar que los nodos pertenecientes a un clúster funcionan de manera correcta y detectar si un nodo ha dejado de fucionar.

* Controlador de puntos finales (end-points controller): Gestiona los puntos finales (end-points) de los servicios desplegados.

* Controlador de la nube: Es un controlador de las últimas versiones de Kubernetes que gestiona la conexión con el proveedor de servicios en la nube que se contrata para nuestro clúster.

### Componentes de los nodos

**Kubelet**

Se ejecuta en cada nodo gestionando los pods y su contenido a través de los ficheros que describen cada pod, y juntos forman las especificaciones de un pod.

Hay que tener en cuenta que Kubelet no administra contenedores que no pertenecen a Kubernetes, es decir, que no hayan sido creados por Kubernetes.

**Kube-proxy**

Se ejecuta en cada nodo y proporciona abstracción de servicios al realizar el reenvío de conexión. Así, cuando llega una petición de servicio a un nodo por parte de un usuario desde el exterior, a un nodo donde no se está ejecutando algún pod de la aplicaciñón que se encarga de ellos, kube-proxy se ocupa de hacer que la conexión se reencíe al nodo correcto.

**cAdvisor**

Se dedica a recoger información del uso de los recursos que se usan en los nodos vigilando la CPU, la memoria, sistemas de ficheros y el uso de la red. La información recogida se usa para informar al nodo maestro.

### Complementos de los nodos

Los complementos dotan a la estructura del clúster de funcionalidades adicionales, que aún no está disponibles por defecto en Kubernetes, pero que se pueden implementar a través de terceros:

* DNS: Pese a ser un complemento, su funcionalidad es necesaria para el correcto funcionamiento del clúster. Será usado por kube-proxy, para reenviar la información.

* Dashboard: Una interfície web destinada al usuario con finalidades de administración del clúster.

En la siguiente imagen se muestra la estructura completa de la arquitectura con los componentes en cada elemento del clúster.

![](https://miro.medium.com/max/981/1*HXbT0c4Q5XaiCIp6y3VMvw.png)

## Etiquetas

Están definidas en los objetos y son una dupla de clave y valor separados por dos puntos, es decir, con la forma "clave:valor". Se usan para la gestión de los objetos por parte de los controladores del clúster.

Hay que tener en cuenta que una etiqueta debe ser única en un objeto pero puede tener varios valores, incluso estar vacío.

En el objeto las etiquetas se organizan a continuación de la etiqueta "labels" de los metadatos.

```
"metadata": {
  "labels": {
    "key1" : "value1",
    "key2" : "value2"
  }
}
```

Las etiquetas no están previamente definidas, pudiendo cada desarrollador establecer los nombres que más convengan.

### Selectores de etiquetas (labels)

Antes se ha visto que no pueden existir dos etiquetas con el mismo nombre en un objeto, pero si puede haber dos objetos con la misma etiqueta y el mismo valor. Esto hace posible agrupar un conjunto de objetos, en función de una etiqueta, usando los selectores de etiquetas y comprobando los valores que estas tienen. Por ejemplo, se utiliza para ejecutar pods para prestar un servicio en el sistema.

### Tipos de selectores

* Selectores de igualdad: Busca igualdades que cumplan la condición especificada en el operador definido de la expresión implementada.

```
1. app = server
2. app != zone1
```

El primero selecciona objetos que tienen la etiqueta "app" y el valor "server", y el segundo ejemplo excluye los objetos que tengan el valor "zone1".

* Selectores de conjunto: Este tipo de selectores busca que una etiqueta tenga un valor que satisface un rango de opciones, es decir, no hay una condición específica de selección como en los selectores de igualdad.

```
1. type in (server, client1)
2. zone notin (zone1, zone2)
```

El primer ejemplo selecciona los objetos que tienen la etiqueta "type" con los valores "server" y "cliente1". El segundo ejemplo selecciona los objetos que no tengan el valor "zone1", "zone2" y "zone3". Como se ve en ambos ejemplos, puede haber varios valores para configurar la regla del selector.

* Selectores de clave: Estos selectores solo comprueban la existencia de una etiqueta, sin importar el valor que tiene.

```
1. app
2. type
3. !zone
```

Los dos primeros ejemplos selecionan los objetos que tengan la etiqueta, mientras que el tercer ejemplo excluye a los objetos que contengan la etiqueta "zone". Como no importa el valor de las etiquetas, no se implementan en la regla, es más una condición de existencia.

## Espacio de nombres (namespaces)

El espacio de nombres sirve para dividir el clúster físico de manera virtual, de forma que podría decirse que crea clústers virtuales. Además, esta división lógica crea aislamientos de nombres entre los objetos de diferentes espacios de nombres. Por ejemplo, podríamos tener un pod en un nodo con un nombre, y en el mismo nodo, pero en un namespace diferente otro pod con el mismo nombre.

![](https://i.imgur.com/CB1epTZ.jpg)

Cuando se crea un clúster de Kubernetes, se crean por defecto tres namespaces en el sistema:

* Default: Cuando se crean objetos a los que no se le ha especificado un espacio de nombres concreto se le asigna el namespace "default".

* Kube-system: Kubernetes necesita objetos para funcionar y desempeñar su trabajo. Estos objetos se crean en este espacio de nombres.

* Kube-public: Como su nombre indica es público y por lo tanto todos los usuarios pueden acceder a su contenido. Puede ser un espacio que esté vacío o que contenga información pública que interese mostrar.

Adicionalmente, a estos tres espacios de nombres, se pueden crear nuevos espacios para dividir el sistema de manera conveniente. Por ejemplo, crear un espacio de nombres dedicado a pruebas de las aplicaciones que queramos testear previamente.

## Objetos controladores

Existen objetos de los controladores encargados de que el clúster funcione correctamente para que pueda gestionar los pods y, por lo tanto, la orquestración de contenedores.

### Deployment

Es el controlador de despliegues de contenedores que necesitamos para nuestra aplicación. Se encarga de que esta se ejecute en base a unas características específicas. Por ejemplo, el número de pods que queremos que se ejecuten.

###  ReplicaSet

Es un controlador de réplicas de pods de la aplicación desplegada. Estas cantidades específicas de réplicas se vigilan y, en caso de que no se cumplan, ReplicaSet se encarga de recuperar el estado deseado del número de réplicas. Por ejemplo, si hemos especificado que queremos tres réplicas de un pod y uno de los nodos del clúster deja de funcionar, ReplicaSet se encargará de solicitar la creación de nuevos pods que se alojarán en otros nodos para así seguir manteniendo la configuración deseada.

## Almacenamiento

Las aplicaciones que se ejecutan en contenedores pueden necesitar algún tipo de almacenamiento para su correcta ejecución, ya sea solo durante la ejecución o recibiendo información del sistema de almacenamiento. Es por eso que Kubernetes utiliza volúmenes capaces de almacenar datos.

### Volúmenes

Para este almacenamiento se crea un objeto volumen llamado "volumen", que se ejecuta en el pod y es accesible por todos los contenedores ejecutados en el mismo. Pero este almacenamiento es temporal, es decir, lo que se almacena en un pod solo permanece durante la ejecución del pod. Por lo que, si tanto un pod como un nodo que ejecuta ese pod deja de funcionar, Kubernetes puede lanzar otro pod para reponer el servicio, pero el volumen vuelve a tener la información especificada en la configuración de la aplicación que estamos desplegando, no del anterior.

* EmptyDir: Monta un volumen vacío en el pod del contenedor en la ruta especificada en la configuración del contenedor con la etiqueta "emptyDir".

### Volúmenes persistentes

En cambio, si queremos usar almacenamiento persistente en un pod, se utiliza un volumen de tipo persistente llamado *persistentVolume*. Este tipo de volúmenes cargarán una ruta de la máquina física y con una petición de volumen llamada *persistentVolumeClaim* se monta en el ordenador.

Cuando se modifica algo en este volumen también se modifica en la máquina física. Por lo que, si un nodo se reinicia sigue teniendo la información que modificó.

* HostPath: Monta una ruta del sistema de ficheros del nodo en la ruta indicada en la configuración del contenedor. Esta información se da como valor de la etquieta "path" que estará anidada debajo de la etiqueta "hostPath".

### Volúmenes de proveedores de nube

Tanto Amazon como Google y demás, disponen de sus propios sistemas de almacenamiento persistente. Estos volúmenes solo se pueden usar en los contenedores de la misma zona geográfica, por lo que es importante usar mecanismos como los selectores para que los pods que quieran usar estos volúmenes cumplan con este requisito.

* gcePersistentDisk: Monta un volumen Google Compute Engine (GCE) persistent disk en un pod.

* awsElasticBlockStore: Monta un volumen AWS EBS Volume en un pod.

* azureDisk: Monta un Microsoft Azure Data Disk en un pod.

* azureFile: Monta un Microsoft Azure File Volume en un pod.

## Servicios

Cuando la aplicación ya es desplegada en un clúster es importante saber como conectarse a la aplicación para que un cliente pueda realizar la petición de un servicio. Los servicios exponen la aplicación de todas las réplicas existentes de manera unificada, utilizando etiquetas y selectores.

### Tipos de servicios

* ClusterIP: Define el servicio asignando una IP interna de clúster, por lo que solo se puede acceder al servicio desde el propio clúster. De esta manera no hay que preocuparse de las variaciones de las direcciones IP de los nodos del clúster que se puedan dar por sustitución de nodos o reasignación de direcciones.

* NodePort: Asigna el mismo puerto en cada nodo para cada servicio, así con NodeIP (IP del nodo) y NodePort (puerto asignado para el servicio) se puede acceder al servicio desde fuera del clúster. *Kube-proxy* se encargará de enrutar el tráfico, usando el servicio DNS, al nodo correcto.

Para que el usuario que acceda a los servicios no tenga la necesidad de conocer todas las direcciones IP, se puede crear un balanceador de carga externo.

* LoadBalancer: Si usamos un proveedor de servicios en nube para nuestro clúster de Kubernetes no necesitamos crear un balanceador de carga, ya que la mayoría de los proveedores tienen plugins para esta tarea. A estos servicios se denominan de tipo *LoadBalancer* y crean una IP fija y externa al servicio àra que el balanceador se encargue de encaminar el tráfico.

## Networking
