---
title: 'Update version as instance group'
---


Deploy a new version as an instance group behind a load balancer
================================================================


## Objectives

* implement a deployment strategy
* at least 3 instances of the service are running
* the instance group is proxied by a load balancer
* service is updated w/o downtime


## Prerequisites

* Tutorial: [Automate service installation and apply configuration to a machine]({{< relref "./automate-system-configuration" >}})


## Remarks

Decide on one of the following designs:
 
* __Design A - everything in one machine, processes as instances:__ a running machine (min. 2 GB memory),
  accessible via SSH
* __Design B - machines as instances:__ 4-5 running machines (min. 512B MB memory, 0.5 CPU), accessible
  via SSH


## Tasks

0. Ensure that all machines exist and are accessible via Configuration Management tool
1. Prepare the target systems by installing and configuring dependencies for the load balancer (e.g. Nginx)
   and, if needed, any backing service(s) (e.g. database)
2. Deploy the service as an instance group containing 3 instances, and make the load balancer aware of
   these instances
3. Implement one of the known deployment strategies by utilizing CM functionality
4. Choose another version of the service and re-deploy to verify that the chosen strategy behaves as expected

{{< hint info >}}
Depending on the type of artifact, dependencies may vary. If the artifact is a container image for example,
then a container runtime (e.g. Podman) would need to be installed as well.
{{< /hint >}}

{{< hint info >}}
In case of __Design B__, load balancer and other backing services can be installed on the same
machine, but make sure that only the LB is exposed to the "outside world".
{{< /hint >}}


## Deliverables

* source code
* `terminal.log` file showing the commands and their output used to carry out the tasks


## Solution: local {id="solution-local"}

*Please note that this solution is based on a local approach utilizing Vagrant and
builds on top of the tutorial about
[automating system configuration]({{< relref "./automate-system-configuration" >}}).
Source code can be found
[here](https://github.com/lucendio/lecture-devops-code/tree/master/tutorials/update-version-as-instance-group/local).*


### (0) Preparations

Refer to the solution of [this]({{< relref "./automate-system-configuration" >}}) tutorial to learn
more about how to set up one or more machines locally with *Vagrant*.

{{< hint info >}}
You may need to adjust the `ansible_host` variable throughout the `./inventory` to match the
hostname(s)/IP(s) of the machine(s).
{{< /hint >}}

Re-using the *Ansible role* from the tutorial mentioned in [Prerequisites](#prerequisites) can be accomplished
in different ways:

* copy & paste the role into a `./roles` subdirectory and add `roles_path = ./roles` to `ansible.cfg`
* symlink `./roles/webservice` to the respective role directory of the tutorial solution mentioned
   in [Prerequisites](#prerequisites)
* append the destination path mentioned in (b) to the `roles_path` value, e.g.
  ```
  roles_path = ./roles:./../other/roles
  ``` 


### (1) Configure machine(s) with Ansible

Define and configure an instance group in the *Ansible inventory* and prepare them, e.g. by
installing the static binary of the *webservice* or a container runtime.

Install and set up the load balancer (Nginx). Install and start the database backing service (Redis).

{{< hint info >}}
To make it more visible what's going on. In this case, there is no dependency on any 3rd-party
code. Feel free to use other people's code instead, e.g.
[DavidWittman/ansible-redis](https://github.com/DavidWittman/ansible-redis)
or
[nginxinc/ansible-role-nginx](https://github.com/nginxinc/ansible-role-nginx).
{{< /hint >}}

Write a playbook to manage the system via code and apply the configuration to reach a state
compliant with the specification mentioned above.

```bash
ansible-playbook --inventory ./inventory ./playbooks/prepare.yaml
```

### (2) Deploy the service as an instance group

Multiple instances of the application must be started as a service, which means a
configuration file representing the service must be created. It must be ensured that
each service started successfully.

The load balancer must be informed about the instances, so that it can distribute
incoming request across all available instances. Eventually,dDisabled/flagged
instances should be retired.

{{< hint info >}}
In this case, all instances are deploys on the same machine, therefore each service needs a
different name, even though each one invokes the same static binary. The service name is
derived from the application name combined with the *Ansible host alias*.

This is not necessary when each instance runs on its own dedicated machine, because on
a machine, service names must be unique.
{{< /hint >}}

Write a playbook to manage the system via code and apply the configuration to reach a state
compliant with the specification mentioned above.

```bash
ansible-playbook --inventory ./inventory ./playbooks/deploy.yaml
```


### (3) Implement one of the deployment strategies to update the application

Utilize Ansible's [asynchronous](https://docs.ansible.com/ansible/latest/user_guide/playbooks_strategies.html).
In this case, the *play* keyword `serial` is used to apply configuration onto `1` *Ansible host* at a time.
This results in an implementation of the *Rolling Restart* strategy of a single instance group. The existing
instances are being updated and restarted without getting removed from the load balancer and eventually
decommissioned.

{{< hint info >}}
Completely replacing instances is usually the easier approach instead of reusing the instances, but it's significant
more costly to do this with machines compared to e.g. containers.
{{< /hint >}}

Copy `./playbooks/deploy.yaml` and adjust the content accordingly in order to implement a suitable strategy.
Then, run:

```bash
ansible-playbook --inventory ./inventory ./playbooks/update.yaml
```


### (4) Change the version and re-deploy the service

In this case, the *Ansible playbooks*, `deploy` and `update`, only recognize changes in the service
configuration file. So, to see the deployment strategy in action, set/change the `font_color` for the
entire instance group in the *Ansible inventory*

```yaml
instance_group:
  # ...
  vars:
    font_color: 'red'
```

and run

```bash
ansible-playbook --inventory ./inventory ./playbooks/update.yaml
```

again, while frequently reloading `http://webservice.vagrant.local` in the browser.

{{< hint >}}
One question remains: what needs to be changed, so that the service also restarts if the
static binary has changed in the filesystem? Left as an optional exercise. 
{{< /hint >}}
