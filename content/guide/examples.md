---
title: 'Examples'
---


Example stacks
==============


The subsequent collection of examples describes - in a very vague manner - possible technology stacks and even relations
between some of them. It's just meant to be an inspiration and starting point of research when choosing technologies for
you *project*.

For more details about specific technologies mentioned below, please refer to the
[software list]({{< ref "/guide/software" >}}).

*Please note, that none of the examples are mutual exclusive. Certain items across these examples may yield other
reasonable or even more suitable combination(s).*


{{< hint info >}}
__Categories:__

*local* : your own computer

*remote* : any other computer; aka *'The Cloud'*

*hybrid* : *local* & *remote*
{{< /hint >}}


### 1a – virtual machines *[local]*

```
|  Technology                                         |  Purpose(s)                   |
|=====================================================|===============================|
|  <application>                                      |                               |
|  Gitea                                              |  VCS                          |
|  Jenkins                                            |  CI/CD                        |
|  HAProxy                                            |  proxy, vhost, load balancer  |
|  ELK                                                |  logging                      |
|   * Virtualbox + Vagrant                            |  runtime environment          |
|   * Packer + Ansible                                |  automation                   |
+=====================================================+===============================+
```


### 1b – Containers *[local]*

```
|  Technology                                         |  Purpose(s)                   |
|=====================================================|===============================|
|  <application>                                      |                               |
|  Gitea                                              |  VCS                          |
|  Concourse                                          |  CI/CD                        |
|  Nginx                                              |  proxy, vhost, load balancer  |
|  Monit                                              |  metrics                      |
|   * Docker                                          |  runtime environment          |
|   * Ansible                                         |  automation                   |
+=====================================================+===============================+
```


### 1c – Orchestration *[local]*

```
|  Technology                                         |  Purpose(s)                   |
|=====================================================|===============================|
|  Kubernetes                                         |  proxy, vhost, load balancer  |
|   * Virtualbox + Vagrant                            |  runtime environment          |
|   * Ansible + Kubespray                             |  automation                   |
+-----------------------------------------------------+-------------------------------+
|  <application>                                      |                               |
|  GitLab                                             |  VCS, CI/CD, artifact host    |
|  Prometheus & Grafan                                |  metrics                      |
|   * Helm                                            |  automation                   |
+=====================================================+===============================+
```


### 1d – Orchestration *[remote]* (university)

```
|  Technology                    |  Purpose(s)                                        |
|================================|====================================================|
|  Kubernetes                    |  runtime environment, proxy, vhost, load balancer  |
+--------------------------------+----------------------------------------------------+
|  <application>                 |                                                    |
|  GitLab                        |  VCS, CI/CD, artifact host                         |
|  EFK                           |  logging                                           |
|   * Kustomize                  |  automation                                        |
+================================+====================================================+
```


### 2a – Cloud services + Monitoring VM *[remote]*

```
|  Technology                           |  Purpose(s)                                 |
|=======================================|=============================================|
|  Bitbucket                            |  VCS, CI/CD, artifact host                  |
+---------------------------------------+---------------------------------------------+
|  <application>                        |                                             |
|   * Heroku                            |  runtime environment, vhost, load balancer  |
|   * OpenTofu                          |  automation                                 |
+---------------------------------------+---------------------------------------------+
|  EFK                                  |  logging                                    |
|   * DigitalOcean                      |  runtime environment                        |
|   * OpenTofu                          |  automation                                 |
|   * Ansible                           |  automation                                 |
+=======================================+=============================================+
```


### 2b – Multi-Cloud: managed *[remote]*

```
|  Technology                    |  Purpose(s)                                        |
|================================|====================================================|
|  Github                        |  VCS                                               |
+--------------------------------+----------------------------------------------------+
|  Github Actions                |  CI/CD, artifact host                              |
+--------------------------------+----------------------------------------------------+
|  <application>                 |                                                    |
|   * AWS                        |  runtime environment, proxy, vhost, load balancer  |
|   * OpenTofu                   |  automation                                        |
+--------------------------------+----------------------------------------------------+
|  Splunk                        |  logging                                           |
+================================+====================================================+
```


### 2c – Local + Cloud *[hybrid]*

```
|  Technology                                          |  Purpose(s)                  |
|======================================================|==============================|
|  Github                                              |  VCS                         |
+------------------------------------------------------+------------------------------+
|  Jenkins                                             |  CI/CD                       |
|   * Virtualbox + Vagrant or Docker                   |  runtime environment         |
|   * Ansible                                          |  automation                  |
+------------------------------------------------------+------------------------------+
|  <application>                                       |                              |
|  HAProxy                                             |  proxy, vhost,load-balancer  |
|  ELK                                                 |  logging                     |
|   * DigitalOcean                                     |  runtime environment         |
|   * OpenTofu                                         |  automation                  |
|   * Ansible                                          |  automation                  |
+------------------------------------------------------+------------------------------+
```


### 3a - Cloud Orchestration: managed *[remote]*

```
|  Technology                    |  Purpose(s)                                        |
|================================|====================================================|
|  Bitbucket                     |  VCS, artifact host                                |
+--------------------------------+----------------------------------------------------+
|  GKE                           |  runtime environment, proxy, vhost, load balancer  |
|   * OpenTofu                   |  automation                                        |
+--------------------------------+----------------------------------------------------+
|  <application>                 |                                                    |
|  Jenkins + K8s Plugin          |  CI/CD                                             |
|   * Helm                       |  automation                                        |
+--------------------------------+----------------------------------------------------+
|  Stackdriver                   |  logging                                           |
+--------------------------------+----------------------------------------------------+
```


### 3b - Cloud Orchestration: DIY *[remote]*

```
|  Technology                                         |  Purpose(s)                   |
|=====================================================|===============================|
|  Github                                             |  VCS                          |
+-----------------------------------------------------+-------------------------------+
|  Kubernetes                                         |  proxy, vhost, load balancer  |
|   * DigitalOcean                                    |  runtime environment          |
|   * OpenTofo                                        |  automation                   |
|   * Ansible + Kubespray                             |  automation                   |
+-----------------------------------------------------+-------------------------------+
|  <application>                                      |                               |
|  Jenkins + K8s Plugin                               |  CI/CD                        |
|  EFK                                                |  logging                      |
|   * Helm                                            |  automation                   |
+-----------------------------------------------------+-------------------------------+
```


### 4 – All in One *[local]* or *[remote]*

```
|  Technology                                                 |  Purpose(s)           |
|=============================================================|=======================|
|  OpenShift, includes:                                       |                       |
|    Kubernetes                                               |  runtime environment  |
|    HAProxy                                                  |  proxy,load balancer  |
|    Container Registry                                       |  artifact host        |
|    Jenkins                                                  |  CI/CD                |
|    EFK                                                      |  logging              |
|    Prometheus + Grafana                                     |  metrics              |
+-------------------------------------------------------------+-----------------------+
|  <application>                                              |                       |
|  Gitea                                                      |  VCS                  |
|   * Helm                                                    |  automation           |
+-------------------------------------------------------------+-----------------------+
```


### 5 – Jenkins X 

```
|  Technology     |  Purpose(s)                                                       |
|=================|===================================================================|
|  Github         |  VCS                                                              |
+-----------------+-------------------------------------------------------------------+
|  Jenkins X      |  CI/CD, automation, proxy, load balancer, metrics, artifact host  |
|   * AWS         |  runtime environment                                              |
+-----------------+-------------------------------------------------------------------+
```
