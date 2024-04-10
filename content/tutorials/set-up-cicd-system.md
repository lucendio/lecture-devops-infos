--- 
title: 'Set up CI/CD system'
---


Install and set up a CI/CD system
=================================


## Objectives

* install Jenkins server in an automated fashion
* Set up and configure Jenkins as needed
* Jenkins web console is accessible, you can login successfully and a pipeline could be configured 


## Prerequisites

* a Configuration Management Tool or Container Runtime is installed
* *[optional]* a running virtual machine (min. 2 GB memory), accessible via SSH


## Tasks

1. Leverage the work others already did
    a) Does a 3rd-party container image already exist? One that might even be officially supported?
    b) Are there any packages supported by the CM tool you use, that can ease with the installation? 
2. Make yourself familiar with the documentation according to your choice and eventually install
   Jenkins.
    {{< hint >}}
You may want to disable the so called 'Setup/Upgrade Wizard' and instead go with 3. 
    {{< /hint >}}
3. *[optional]* Utilize the
   [*Jenkins Configuration as Code (a.k.a. JCasC)*](https://github.com/jenkinsci/configuration-as-code-plugin)
   approach to make your setup reproducible   
4. Expose the web console (if you went through the 'Setup Wizard' this might already be the case) and
   try to login. 

{{< hint info >}}
You may find some of the [official tutorials](https://www.jenkins.io/doc/tutorials) helpful, or the one
on [Docker & JcaC](https://www.digitalocean.com/community/tutorials/how-to-automate-jenkins-setup-with-docker-and-jenkins-configuration-as-code).
{{< /hint >}}


## Solution

*Please note that this solution is based on a local approach utilizing Vagrant and
reusing an Ansible Role. Source code can be found
[here](https://github.com/lucendio/lecture-devops-code/tree/master/tutorials/set-up-cicd-system)*


### (0) Preparations

We need a machine to install Jenkins on. Take a look at one of the previous
tutorials and take your pick - local (Vagrant) or remote (AWS).

Please note, that the
[`Vagrantfile`](https://github.com/lucendio/lecture-devops-code/blob/master/tutorials/06_set-up-cicd-system/Vagrantfile)
in [this folder](https://github.com/lucendio/lecture-devops-code/tree/master/tutorials/set-up-cicd-system)
requires an additional plugin to be installed:

```bash
vagrant plugin install vagrant-hostmanager
```

{{< hint info >}}
The plugin maintains entries in your host's `/etc/hosts` and thus may ask
for a local administrator password local during initial start up as well as when
destroying the machine again.
{{< /hint >}}


### (1) Which kind of work do you want to reuse?

* __A:__ [Ansible role](https://github.com/geerlingguy/ansible-role-jenkins)
* __B:__ [Container image](https://github.com/jenkinsci/docker/blob/master/README.md)

Choose and dive into it's documentation.


### (2) Write Ansible code to automate the installation

* __A:__ Role 

    Aside from writing the Ansible playbook, one would first need to install the Ansible
    *role* that contains all the heavy lifting:
    
    ```bash
    ansible-galaxy role install -r ./requirements.yml --roles-path ./roles
    ```
    
    {{< hint info >}}
`requirements.yml` works as a dependency manifest and the `roles` folder
is configured in `ansible.cfg`.
    {{< /hint >}}

* __B:__ Container image

    Recommended instructions:
    
    * install dependencies: container runtime
    * (pull Jenkins container image)
    * either directly use an Ansible task
      (e.g. [podman](https://docs.ansible.com/ansible/latest/collections/containers/podman/podman_container_module.html)
      or [docker](https://docs.ansible.com/ansible/latest/collections/community/docker/docker_container_module.html))
      or configure the host's process manager (e.g. systemd) with a
      [template](http://docs.ansible.com/ansible/latest/collections/ansible/builtin/template_module.html)
      to start the container

    {{< hint info >}}
Separate parameters and logic by utilizing *inventory* and *playbook*
    {{< /hint >}}
