---
title: 'Deploy a new version as an instance group'
---


Deploy a new version as an instance group
=========================================


## Objectives

* at least 3 instances of the service are running
* the instance group is proxied by a load balancer
* service is updated w/o downtime


## Prerequisites

* Configuration Management Tool installed on the local development machine
* a stateful or stateless service (containerized or not) for the instance group (e.g. [HedgeDoc](https://github.com/hedgedoc/hedgedoc/)) 
* *[optional]* Container Runtime installed on the target system

__Approach A - everything in one machine, containers as instances:__
* *[optional]* a running machine (min. 2 GB memory), accessible via SSH

__Approach B - machines as instances:__
* 3-4 running machines (min. 512B MB memory, 0.5 CPU), accessible via SSH


## Tasks

0. Ensure that all machines exist and are accessible by Configuration Management tool
1. Prepare the target systems by installing and configuring the following dependencies:
    * load balancer (e.g. Nginx)
    * backing service(s) (e.g. database) if needed
    * container runtime (e.g. Podman) if needed
    
    {{< hint >}}
In case of __Approach B__: Load balancer and other backing services could be installed on the same
machine, but make sure that only the LB is exposed to the outside world.
    {{< /hint >}}

2. Deploy the service as an instance group containing 3 instances

    {{< hint info >}}
Don't forget to make the load balancer aware of these instances, and enable session persistence if the
service is stateful and requires sticky sessions.
    {{< /hint >}}

3. Implement one of the known deployment strategies by utilizing CM functionality
4. Choose another version of the service and re-deploy to verify that the chosen strategy behaves as expected


## Solution: local {id="solution-local"}

*Please note that this solution is based on a local approach utilizing Vagrant and
managing containerized process with the system's process manager. Source code can
be found
[here](https://github.com/lucendio/lecture-devops-code/tree/master/tutorials/07_update-version-as-instance-group/local).*


### (0) Preparations

Utilize a combination of previous tutorials to allocate a virtual machine locally with Vagrant.

* Vagrant HostManager
* pre-provisioned dedicated user (`operator`)

```bash
mkdir -p ./.ssh
ssh-keygen -t rsa -b 4096 -C "operator" -N "" -f ./.ssh/operator
chmod 600 ./.ssh/operator*
```

```bash
vagrant up
```

### (1) Configure machine with Ansible

* install container runtime
* set up load balancer (Nginx) and configure instance group based on the list of
  instances defined in the
  [inventory](https://github.com/lucendio/lecture-devops-code/tree/master/tutorials/07_update-version-as-instance-group/local/inventory/instance_group.yaml)
* start a database container

__⚡ Current working directory:__ [here](https://github.com/lucendio/lecture-devops-code/tree/master/tutorials/07_update-version-as-instance-group/local)
```bash
ansible-playbook --inventory ./inventory ./play_configure.yaml
```

### (2) Deploy the service as an instance group

Choose a service - in this case: [HedgeDoc](https://docs.hedgedoc.org/) and
deploy it in multiple containers as an instance group of 3 instances.

__⚡ Current working directory:__ [here](https://github.com/lucendio/lecture-devops-code/tree/master/tutorials/07_update-version-as-instance-group/local)
```bash
ansible-playbook --inventory ./inventory ./play_deploy.yaml
```

### (3) Implement one of the deployment strategies

Utilize Ansible's [asynchronous](https://docs.ansible.com/ansible/latest/user_guide/playbooks_strategies.html).
In this case, the *play* keyword `serial` is used to apply configuration onto `1` host at a time.


### (4) Change the version and re-deploy the service

Adjust `service_version` in  `./inventory/instance_group.yaml`

__⚡ Current working directory:__ [here](https://github.com/lucendio/lecture-devops-code/tree/master/tutorials/07_update-version-as-instance-group/local)
```bash
ansible-playbook --inventory ./inventory ./play_deploy.yaml
```


## Solution: remote

*Source code can be found
[here](https://github.com/lucendio/lecture-devops-code/tree/master/tutorials/07_update-version-as-instance-group/remote).*

*TODO*


## Solution: Docker Compose

*Source code can be found
[here](https://github.com/lucendio/lecture-devops-code/tree/master/tutorials/07_update-version-as-instance-group/docker-compose).*

*TODO*
