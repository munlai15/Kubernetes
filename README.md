Project HISX2 2020-2021 Kubernetes

# Kubernetes

## Introducción

### ¿Qué es Kubernetes?

Kubernetes es una plataforma portable y extensible de código abierto para administrar cargas de trabajo y servicios. Kubernetes facilita la automatización y la configuración declarativa. Tiene un ecosistema grande y en rápido crecimiento. El soporte, las herramientas y los servicios para Kubernetes están ampliamente disponibles.

Google liberó el proyecto Kubernetes en el año 2014. Kubernetes se basa en la experiencia de Google corriendo aplicaciones en producción a gran escalapor década y media, junto a las mejores ideas y prácticas de la comunidad.

![](https://d33wubrfki0l68.cloudfront.net/e298a92e2454520dddefc3b4df28ad68f9b91c6f/70d52/images/docs/pre-ccm-arch.png)

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

## Componentes de Kubernetes
