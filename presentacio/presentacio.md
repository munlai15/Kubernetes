# Kubernetes
## Project HISX2 2020-2021

![](../aux/logo.png)

Adrián Narváez

#
## Introducció
### Què és Kubernetes?
Kubernetes és una plataforma portable i extensible de codi obert per a administrar càrregues de treball i serveis en contenidors.
Google va alliberar el projecte Kubernetes l'any 2014

#
### Contenidors
Kubernetes ofereix un entorn d'administració centrat en contenidors.
![.](../aux/containers.png)

#
### Beneficis
En resum, els beneficis d'usar contenidors inclouen:
+ Àgil creació i desplegament d'aplicacions
+ Desenvolupament, integració i desplegament continu
+ Separació de tasques entre Dev i Ops
+ Observabilitat
+ Consistència entre els entorns de desenvolupament, proves i producció
+ Portabilitat entre núvols i distribucions
+ Administració centrada en l'aplicació
+ Microserveis distribuïts, elàstics, alliberats i feblement acoblats
+ Aïllament de recursos
+ Utilització de recursos

#
## Arquitectura
Kubernetes distribueix els contenidors en pods, així aquests poden estar en diversos nodes. Al seu torn, aquests nodes formen un clúster, completant així l'estructura que té Kubernetes.
+ Pod
+ Node
+ Clúster

#
## Components 
Perquè l'arquitectura descrita abans funcioni correctament, Kubernetes utilitza diversos components en els diferents elements del seu sistema.
### Components del node máster
+ Servidor API
+ Etcd
+ Planificador (Scheduler)
+ Gestor de controladors (Controller-manager)

#
### Components dels nodes workers
+ Kubelet
+ Kube-proxy
+ cAdvisor

#
![.](../aux/cluster.png)

#
## Espai de noms (namespaces)
Serveix per a dividir el clúster físic de manera virtual, de manera que podria dir-se que crea clústers virtuals.
Quan es crea un clúster de Kubernetes, es creen per defecte tres namespaces en el sistema:
* Default
* Kube-system
* Kube-public  

#
## Objectes controladors
Existeixen objectes dels controladors encarregats que el clúster funcioni correctament perquè pugui gestionar els pods i, per tant, la orquestració de contenidors.
### Deployment
És el controlador de desplegaments de contenidors que necessitem per a la nostra aplicació. S'encarrega que aquesta s'executi sobre la base d'unes característiques específiques. Per exemple, el número de pods que volem que s'executin.  

#
### ReplicaSet
És un controlador de rèpliques de pods de l'aplicació desplegada. Aquestes quantitats específiques de rèpliques es vigilen i, en cas que no es compleixin, ReplicaSet s'encarrega de recuperar l'estat desitjat del nombre de rèpliques.  
