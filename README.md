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

![](https://d33wubrfki0l68.cloudfront.net/e298a92e2454520dddefc3b4df28ad68f9b91c6f/70d52/images/docs/pre-ccm-arch.png)

En la imagen que se muestra arriba, se ve la estructura de un clúster (en este caso conectado a la nube). Como mínimo un clúster tiene que estar formado por tres nodos, un máster y dos trabajadores, a excepción de Minikube que veremos más adelante ya que usa uno para todo.

Por último, la comunicación de los nodos con el máster y del usuario con el clúster, se hace a través de la API de Kubernetes.

## Componentes de Kubernetes
