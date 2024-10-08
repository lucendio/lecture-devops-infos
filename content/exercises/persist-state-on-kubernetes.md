---
title: 'Persist state on Kubernetes'
---


Enable and utilize persistence on Kubernetes to store state
===========================================================

{{< hint danger >}}
*under construction*
{{< /hint >}}

## Objectives

* TODO
* use Helm to deploy and update an application


## Prerequisites

* Exercise: [{{< page-title "./deploy-app-on-kubernetes" >}}]({{< relref "./deploy-app-on-kubernetes" >}}) 
* [Helm](https://helm.sh/docs/intro/install/) is installed on your workstation


## Remarks

* TODO


## Tasks

1. Ensure database state persists across redeployment


## Deliverables

* source code
* `terminal.log` file showing the commands and their output used to carry out the tasks


## Solution: Edu cluster {id="solution-edu-cluster"}

*Source code can be found
[here](https://github.com/lucendio/lecture-devops-code/tree/master/exercises/persist-state-on-kubernetes/edu-cluster).*

TODO


### (1) Enable TLS and Data persistence

Both features are covered in the upstream documentation
([TLS](https://gitlab.bht-berlin.de/ris/doku/-/wikis/educluster#ressourcen-service-ingress),
[Storage](https://gitlab.bht-berlin.de/ris/doku/-/wikis/educluster#ressourcen-volumes-persistener-speicher-f%C3%BCr-deployments-und-pods)).
You just have to integrate the respective attributes and objects into you chart(s) and make them
configurable via Helm values if needed.



## Solution: Minikube {id="solution-minikube"}

*Source code can be found
[here](https://github.com/lucendio/lecture-devops-code/tree/master/exercises/persist-state-on-kubernetes/minikube).*

TODO


### (1) Enable TLS and Data persistence

Minikube may already have [*Persistent Volumes*](https://minikube.sigs.k8s.io/docs/handbook/persistent_volumes/) 
enabled.
