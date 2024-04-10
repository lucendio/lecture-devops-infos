---
title: 'Automate system configuration'
---


Automate system configuration and software installation
=======================================================


## Objectives

* install an application and a supported backing service on a virtual machine
  in an automated fashion
* configure the application to use the backing service


## Prerequisites

* Configuration Management tool is installed (e.g. Ansible, but note [on Windows, it's only supported to run in WSL](https://docs.ansible.com/ansible/latest/user_guide/windows_faq.html#can-ansible-run-on-windows))
* virtual machine with SSH access - either locally or in the cloud (see tutorials 
  [Spin up a virtual machine locally]({{< relref "./spin-up-virtual-machine-locally" >}})
  or [Allocate a virtual machine in the cloud]({{< relref "./allocate-virtual-machine-in-cloud" >}}))


## Tasks

1. Write code for the chosen Configuration Management tool to automate the installation and
   configuration of the [webservice](https://gitlab.bht-berlin.de/fb6-wp11-devops/webservice)
2. Apply the configuration to the virtual machine
3. Verify that the service works and is successfully using the backing service  


## Deliverables

* source code
* `terminal.log` file showing the commands and their output used to carry out the tasks


## Solution

*Please note that this solution uses [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/)
and a [local machine]({{< relref "./spin-up-virtual-machine-locally" >}}). Source code can be found
[here](https://github.com/lucendio/lecture-devops-code/tree/master/tutorials/automate-system-configuration).*


### (0) Preparations

* create an SSH key pair, if necessary (see [this]({{< relref "./allocate-virtual-machine-in-cloud" >}}) tutorial for more
  details)
* spin up a virtual machine, if not already running
* create and customize `ansible.cfg` if needed, e.g.:

```ini
[defaults]
# ...
transport = ssh
private_key_file = ./.ssh/operator
host_key_checking = False
remote_user = operator
# ...
```

{{< hint info >}}
Make sure, the user that you are accessing the machine via SSH with also has sudo capabilities.
{{< /hint >}}


### (1) Write Ansible code to install & configure the application and its backing service

{{< hint info >}}
You may choose the *webservice* artifact for yourself: static binary vs container image.
Depending on where the artifact was published, credentials might be required to download it.
Either would be a common approach.

Please note, that in order to yield a self-contained code base, this solution deviates from said 
approaches by building from source locally (see playbook `build-artifact.yaml`).
{{< /hint >}} 

The following steps may be taken:

* install the application: transfer the artifact to the target system
* configure the application 
* utilize the system's process manager to start the application
* install and - if needed - configure a supported backing service (e.g. Redis)
* *[optional]* placing webservice-specific code into a
  [role](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html)

Write the [inventory](https://docs.ansible.com/ansible/latest/inventory_guide/intro_inventory.html)
containing the respective host information and variables. Use a
[playbook](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_intro.html) to assemble
*tasks* and *roles* in order to apply the desired state to a machine.


### (2) Apply the configuration by using the Ansible tool chain

Build the static binary, if it doesn't already exist in `./artifact.bin`

```bash
ansible-playbook ./build-artifact.yaml
```

Apply system configuration to bring um webservice and its backing service

```bash
ansible-playbook -i ./inventory ./playbook.yaml
```

{{< hint info >}}
If you want Vagrant to take care of the inventory, you may [invoke Ansible
directly from Vagrant](https://www.vagrantup.com/docs/provisioning/ansible) 
{{< /hint >}}


### (3) Verify whether service works and successfully uses backing service

Send some data to the webservice and then try  retrieve it again

__⚡ Context: *host/workstation*__
```bash
curl \
  -X PUT \
  --header 'Content-Type: text/plain; charset=utf-8' \
  --data 'foo' \
  http://10.0.42.23:8080/state/bar
```

__⚡ Context: *host/workstation*__
```bash
curl \
  -X GET http://10.0.42.23:8080/state/bar
```

Result:
```
foo
```

Change some of the webservice configuration (e.g. service port) and apply the changes.
The *handler* attached to some tasks will eventually trigger a restart of the webservice,
if those tasks were finished with a *changed* state.

The `GET` request should still yield the same result.
