---
title: 'Automate web-server installation & configuration'
---


Automate web-server installation and apply configuration to a machine
=====================================================================


## Objectives

* install and configure Nginx on a virtual machine in an automated fashion
* tell Nginx to serve a custom landing page


## Prerequisites

* Ansible is installed ([on Windows, it's only supported to run in WSL](https://docs.ansible.com/ansible/latest/user_guide/windows_faq.html#can-ansible-run-on-windows))
* a running virtual machine with SSH access - either locally or in the cloud


## Tasks

1. Write Ansible code (*playbook*, *inventory*, possibly a *role*) to automate the installation and
   configuration of Nginx. Required steps:
    * install Nginx (probably via OS package manager)
    * create a custom landing page (static file somewhere in the filesystem)
    * configure Nginx so that it knows where that file resides and how to serve it (you can either
      create your own *vhost* by defining a
      [`server` directive](https://docs.nginx.com/nginx/admin-guide/web-server/web-server/#setting-up-virtual-servers)
      to serve your custom landing page, or you override the existing default page)
    * restart Nginx or reload its configuration
2. Apply the configuration to the virtual machine
3. Verify if your own custom landing page shows up in the browser  


## Solution

*Source code can be found
[here](https://github.com/lucendio/lecture-devops-code/tree/master/tutorials/04_automate-webserver-configuration).*

### (1) Generate an SSH key pair

```bash
mkdir -p ./.ssh
ssh-keygen -t rsa -b 4096 -C "operator" -N "" -f ./.ssh/operator
chmod 600 ./.ssh/operator*
```

### (2) Bring up a new virtual machine

{{< hint info >}}
You may choose to allocate a [local machine]({{< ref "/tutorials/spin-up-virtual-machine-locally" >}})
or [one in the cloud]({{< ref "/tutorials/allocate-machine-in-cloud" >}}). Make sure, you have SSH
access with a user that has sudo capabilities. If needed, create a user and add the public part of the
SSH key (`./.ssh/operator.pub`) to the list of `.ssh/authorized_keys`. This solution uses the local
approach.  
{{< /hint >}}

```bash
vagrant up
```

### (3) Write Ansible code

* [inventory](https://github.com/lucendio/lecture-devops-code/tree/master/tutorials/04_automate-webserver-configuration/inventory),
  defining host information and variables
* [playbook](https://github.com/lucendio/lecture-devops-code/blob/master/tutorials/04_automate-webserver-configuration/playbook.yaml),
  installing Nginx and adjusting its configuration
* *[optional]* placing Nginx-specific code in a [role](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html)


### (4) Apply the configuration by using the Ansible tool chain

```bash
ansible-playbook -i ./inventory ./playbook.yml
```

{{< hint info >}}
If you want Vagrant to take care of the inventory, you may [invoke Ansible
directly from Vagrant](https://www.vagrantup.com/docs/provisioning/ansible) 
{{< /hint >}}

{{< hint warning >}}
If you are on Windows, which is not supported as Ansible control host and
[WSL](https://docs.microsoft.com/windows/wsl/install) is not an option, you may
[invoke Ansible locally within the virtual machine via Vagrant](https://www.vagrantup.com/docs/provisioning/ansible_local) 
{{< /hint >}}


### (5) Open your browser and confirm whether your changes where successful

```
http://10.0.42.23
```
