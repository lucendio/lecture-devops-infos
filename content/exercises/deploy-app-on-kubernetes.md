---
title: 'Deploy app on Kubernetes'
---

Deploy an app on Kubernetes and make it accessible to the outside
=================================================================


## Objectives

* gain basic knowledge of how to interact with `kubectl`
* write Kubernetes objects
* schedule workload
* inspect & debug workload


## Prerequisites

* [`kubectl` is installed](https://kubernetes.io/docs/tasks/tools/#kubectl) to interact with the cluster
* a reachable Kubernetes cluster (e.g. [Minikube](https://minikube.sigs.k8s.io/docs/start/),
  [university cluster]({{< ref "/faq/cloud-and-infrastructure#kubeconfig-of-edu-cluster">}}),
  [cloud provider]({{< ref "/faq/cloud-and-infrastructure#which-cloud-provider-should-i-use" >}}))
* working credentials to authenticate against the cluster


## Tasks

0. Gain access to a Kubernetes cluster or install one
1. Get to know `kubectl` (➡️ [cheatsheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/))
    * create a *webservice* *Pod*
    * inspect the pod    
2. Write a `Deployment` manifest and deploy a stateless application
    * create a dedicated namespace
    * write the Deployment configuration file
    * apply the configuration
3. Expose the application
    * write a Service and Ingress configuration
    * apply the configurations
    * inspect the service
    * confirm that the application is accessible from the *"outside"* via CLI

__For more exercises, please refer to the [official documentation](https://kubernetes.io/docs/tasks/).__


## Deliverables

* source code
* `terminal.log` file showing the commands and their output used to carry out the tasks


## Solution: Edu cluster {id="solution-edu-cluster"}

*Please note that this solution is loosely based on a set of tasks written in 
the [official documentation](https://kubernetes.io/docs/tasks/). Source code
can be found
[here](https://github.com/lucendio/lecture-devops-code/tree/master/exercises/deploy-app-on-kubernetes/edu-cluster).*


### (0) Preparations {id="0-prep-edu-cluster"}

{{< hint info >}}
📖 [__Upstream documentation__ (german)](https://gitlab.bht-berlin.de/ris/doku/-/wikis/educluster#nutzung-des-clusters-mit-kubectl)
{{< /hint >}}

1. [login and obtain the *Kubeconfig File*]({{< ref "/faq/cloud-and-infrastructure#kubeconfig-of-edu-cluster">}})
2. navigate to *Projects/Namespaces*, click on *Add Namespace* (right) in the area where the project is named after
   this module; give the namespace a meaningful & unique name (e.g. university/student account ID)
3. [install `kubectl`](https://kubernetes.io/docs/tasks/tools/#kubectl) on your workstation, if it doesn't
   exist already

{{< hint warning >}}
Moving forward, either use `-n {{ NS_NAME }}` whenever namespaced objects are involved, or configure it as the
default value in your local *KubeConfig* - edit the file directly by adding `contexts[*].context.namespace: {{ NS_NAME }}`
or run:

```bash
kubectl config set-context --current --namespace={{ NS_NAME }}
```
{{< /hint >}}


Verify that `kubectl` is configured correctly. The following command should list all the different
Kubernetes objects that are available via *kube-api*.

```bash
kubectl api-resources
```


### (1) Familiarize yourself with `kubectl` and the cluster

No pods should exist yet:

```bash
kubectl get pods
```
Result:
```
No resources found in {{ NS_NAME }} namespace.
```

Start a pod (*`POD_NAME` must be unique*):

```sh
kubectl run {{ POD_NAME }} --image=registry.bht-berlin.de:443/fb6-wp11-devops/webservice
```

The new pod should be listed now:

```bash
kubectl get pods
```
Result:
```
NAME             READY   STATUS    RESTARTS   AGE
{{ POD_NAME }}   1/1     Running   0          15s
```

Inspect the pod:

```bash
kubectl describe pods {{ POD_NAME }}
```

Connect to the running container inside the pod:

__⚡ Context: *workstation*__
```bash
kubectl exec --stdin --tty {{ POD_NAME }} -- bash
```

... and inspect it:

__⚡ Context: *pod*__
```bash
apt update
apt install psmisc
pstree
```
Result:
```
webservice---7*[{webservice}]
```


### (2) Define and deploy an application

Write a `Deployment` configuration for `registry.bht-berlin.de:443/fb6-wp11-devops/webservice` with the 
following specifications and use `kubectl` to deploy it on the cluster.

* replication of __3__
* expose on port `8080`
* deployment strategy: rolling update
* resource requirements (CPU, memory)
* run non-privileged (security context)
* health checks (aka probes)

{{< hint info >}}
Consult the [source code](https://gitlab.bht-berlin.de/fb6-wp11-devops/webservice) in order to find a suitable
endpoint in case you choose the `HTTP` probe type
{{< /hint >}}

```bash
kubectl apply -n {{ NS_NAME }} --filename ./deployment.yaml
```

{{< hint warning >}}
It's also possible to define the namespace in the configuration file:

`metadata.namespace: '{{ NS_NAME }}'`
{{< /hint >}}


Confirm whether the deployment was successful and the `Pods` are healthy:

```bash
kubectl get pods -n {{ NS_NAME } --output wide
```
Result:
```
NAME                                    READY   STATUS    RESTARTS   AGE   IP           NODE           NOMINATED NODE   READINESS GATES
webservice-deployment-d8f8c56ff-2v959   1/1     Running   0          42m   10.42.5.16   edu-worker02   <none>           <none>
webservice-deployment-d8f8c56ff-v9rdg   1/1     Running   0          42m   10.42.6.18   edu-worker03   <none>           <none>
webservice-deployment-d8f8c56ff-dfvg4   1/1     Running   0          42m   10.42.6.15   edu-worker01   <none>           <none>
```

Delete one of the `Pods` to see the *replication controller* kicking in as part of the `Deployment`

Terminal 1:
```bash
kubectl get pods -n {{ NS_NAME }} --output wide --watch
```
Terminal 2:
```bash
kubectl delete pods {{ DEPLOYMENT_POD_NAME }} --grace-period=0 -n {{ NS_NAME }}
```

{{< hint info >}}
After Kubernetes notices that one pod is gone, it creates a new one to match the replication count.
{{< /hint >}}


### (3) Expose the application

Write a `Service` configuration for the `Deployment` with the following specifications and
use `kubectl` to deploy it on the cluster.

* maps port `80` to port `8080`
* TCP as protocol
* from type *Cluster IP*

```bash
kubectl apply -n {{ NS_NAME }} --filename ./service.yaml
```

Verify whether the configuration of the `Service` was successful:
 
```bash
kubectl get services -n {{ NS_NAME }} --output wide
```
Result:
```
NAME                 TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)   AGE   SELECTOR
webservice-service   ClusterIP   10.43.205.119   <none>        80/TCP    41s   app=webservice
```

Connect to the stand-alone pod from section [(1)]({{< relref "#0-prep-edu-cluster" >}})
and verify that the `Service` resolves across the cluster:

__⚡ Context: *pod*__
```bash
curl http://{{ SERVICE_NAME }}.{{ NS_NAME }}.svc.cluster.local/health
```

{{< hint info >}}
The response is supposed to be a JSON.
{{< /hint >}}

To eventually make the application available from outside the cluster, write an `Ingress` configuration 
pointing to the `Service` that you just created.

{{< hint warning >}}
The ingress controller of the education cluster runs on a single node (`141.64.89.201`)
directly on the cluster. The respective DNS entry is an A record pointing to `*.lehre.ris.bht-berlin.de`, 
which is why the ingress host must be set to `{{ SUB_DOMAIN }}.lehre.ris.bht-berlin.de`, where `SUB_DOMAIN`
is a virtual but globally unique host name with the format: `{{ SERVICE_NAME }}-{{ NS_NAME }}`, e.g.
`myservice-987456.lehre.ris.bht-berlin.de` (see [*Preparations* section]({{< relref "#0-prep-edu-cluster" >}}))
{{< /hint >}}

```bash
kubectl apply -n {{ NS_NAME }} --filename ./ingress.yaml
```

Finally, verify that the application is indeed accessible from your browser (or `curl`).


### (4) Cleaning up

Use the [cluster management web console](https://rancher.ris.beuth-hochschule.de) to delete your namespace,
which will then delete all associated objects.


## Solution: Minikube {id="solution-minikube"}

*Please note that this solution is loosely based on a set of tasks written in 
the [official documentation](https://kubernetes.io/docs/tasks/). Source code
can be found
[here](https://github.com/lucendio/lecture-devops-code/tree/master/exercises/deploy-app-on-kubernetes/minikube).*


### (0) Preparations {id="0-prep-minikube"}

Install [minikube](https://minikube.sigs.k8s.io/docs/start/) (Kubernetes on a local virtual machine) and
create a cluster, if you don't have any available. ([see requirements](https://minikube.sigs.k8s.io/docs/start/#what-youll-need))

{{< hint warning >}}
If your workstation runs on ARM, you may choose `qemu` as *driver*. Please note that additional adjustments
might be required.
{{< /hint >}}

```bash
minikube start \
    --cpus=2 --memory=4096m \
    --container-runtime=cri-o \
    --driver=virtualbox \
    --addons=ingress \
    --profile=devops-exercise
```

Verify that `kubectl` is configured correctly. The following command should list all the different
Kubernetes objects that are available via *kube-api*.

```bash
kubectl api-resources
```


### (1) Familiarize yourself with `kubectl` and the cluster

No pods should exist yet:

```bash
kubectl get pods
```
Result:
```
No resources found in {{ NS_NAME }} namespace.
```

Start a pod (*`POD_NAME` must be unique*):

```bash
kubectl run {{ POD_NAME }} --image=registry.bht-berlin.de:443/fb6-wp11-devops/webservice
```

The new pod should be listed now:

```bash
kubectl get pods
```
Result:
```
NAME             READY   STATUS    RESTARTS   AGE
{{ POD_NAME }}   1/1     Running   0          15s
```

Inspect the pod:

```bash
kubectl describe pods {{ POD_NAME }}
```

Connect to the running container inside that pod:

__⚡ Context: *workstation*__
```bash
kubectl exec --stdin --tty {{ POD_NAME }} -- bash
```

... and inspect it:

__⚡ Context: *pod*__
```bash
apt update
apt install psmisc
pstree
```
Result:
```
webservice---7*[{webservice}]
```


### (2) Define and deploy an application

Create a namespace:

```bash
kubectl create namespace {{ NS_NAME }}
```

... and either use `-n {{ NS_NAME }}` whenever namespaced objects are involved, or set:

```bash
kubectl config set-context --current --namespace={{ NS_NAME }}
```

Write a `Deployment` configuration for `registry.bht-berlin.de:443/fb6-wp11-devops/webservice` with the 
following specifications and use `kubectl` to deploy it on the cluster.

* replication of __3__
* expose to port `8080`
* deployment strategy: rolling update
* resource requirements (CPU, memory)
* run non-privileged (security context)
* health checks (aka probes)

{{< hint info >}}
Consult the [source code](https://gitlab.bht-berlin.de/fb6-wp11-devops/webservice) in order to find a suitable
endpoint in case you choose the `HTTP` probe type
{{< /hint >}}

```bash
kubectl apply -n {{ NS_NAME }} --filename ./deployment.yaml
```

{{< hint warning >}}
It's also possible to define the namespace in the configuration file:

`metadata.namespace: '{{ NS_NAME }}'`
{{< /hint >}}

Confirm whether the deployment was successful and the `Pods` are healthy:

```bash
kubectl get pods -n {{ NS_NAME }} --output wide
```
Result:
```
NAME                                     READY   STATUS    RESTARTS   AGE     IP            NODE       NOMINATED NODE   READINESS GATES
webservice-deployment-65cd77f4fc-98gq8   1/1     Running   0          70m     10.244.0.36   minikube   <none>           <none>
webservice-deployment-65cd77f4fc-dmfjv   1/1     Running   0          82m     10.244.0.33   minikube   <none>           <none>
webservice-deployment-65cd77f4fc-edfrp   1/1     Running   0          82m     10.244.0.35   minikube   <none>           <none>
```

Delete one of the `Pods` to see the *replication controller* kicking in as part of the `Deployment`

Terminal 1:
```bash
kubectl get pods -n {{ NS_NAME }} --output wide --watch
```
Terminal 2:
```bash
kubectl delete pods {{ DEPLOYMENT_POD_NAME }} --grace-period=0 -n {{ NS_NAME }}
```

{{< hint info >}}
After Kubernetes notices that one pod is gone, it creates a new one to match the replication count.
{{< /hint >}}


### (3) Expose the application

Write a `Service` configuration for the `Deployment` with the following specifications and
use `kubectl` to deploy it on the cluster.

* maps port `80` to port `8080`
* TCP as protocol
* from type *load balancer*

```bash
kubectl apply -n {{ NS_NAME }} --filename ./service.yaml
```

Verify whether the configuration of the `Service` was successful:
 
```bash
kubectl get services -n {{ SERVICE_NAME }} --output wide
```
Result:
```
NAME                 TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE     SELECTOR
webservice-service   LoadBalancer   10.100.122.224   <pending>     80:32044/TCP   6h35m   app=etherpad
```

Connect to the nginx pod from section [(1)]({{< relref "#0-prep-minikube" >}}) and verify that the `Service` resolves
across the cluster:

__⚡ Context: *pod*__
```bash
curl http://{{ SERVICE_NAME }}.{{ NS_NAME }}.svc.cluster.local/health
```

{{< hint info >}}
The response is supposed to be a JSON.
{{< /hint >}}

To eventually make the application available from outside the cluster, write an `Ingress` configuration 
pointing to the `Service` that you just created. Enable TLS termination.

{{< hint warning >}}
`Ingress` requires FQDNs. To get IP-based FQNDs one can either adjust `/etc/hosts` or utilize
[nip.io](https://nip.io). The IP of you Kubernetes single node cluster can be determined with the
command `minikube ip`
{{< /hint >}}

```bash
kubectl apply -n {{ NS_NAME }} --filename ./ingress.yaml
```

Finally, verify that the application is indeed accessible from your browser (or `curl`).


### (4) Cleaning up

```bash
kubectl delete "{{ NS_NAME }}"
kubectl delete pod {{ POD_NAME }} -n default
minikube stop
minikube delete
```
