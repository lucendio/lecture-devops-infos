07 - Deploy a new version to an instance group
==============================================


### Objective(s)

* at least 3 instances of the service are running
* the instance group is proxied by a load balancer
* service is updated w/o downtime


### Prerequisite(s)

* Configuration Management Tool installed on the local development machine
* a stateful or stateless service (containerized or not) for the instance group (e.g. [CodiMD](https://github.com/hackmdio/codimd)) 
* (optional) Container Runtime installed on the target system

__Approach A - everything in one machine, containers as instances:__
* (optional) a running machine (min. 2 GB memory), accessible via SSH

__Approach B - machines as instances:__
* 3-4 running machines (min. 512B MB memory, 0.5 CPU), accessible via SSH


### Task(s)

0. Ensure that all machines exist and are accessible by CM

1. Prepare the target systems by installing and configuring the following dependencies:
  * load balancer (e.g. Nginx)
  * backing service(s) (e.g. database) if needed
  * container runtime (e.g. Podman) if needed

*__NOTE (B):__ Load balancer and other backing services could be installed on the same machine, but make sure that
only the LB is exposed to the outside world.*

2. Deploy the service as an instance group containing 3 instances

*__NOTE:__ Don't forget to make the load balancer aware of these instances, and, depending on whether the service
is stateful or not, enable session persistence.*

3. Implement one of the known deployment strategies by utilizing CM functionality

4. Choose another version of the service and re-deploy to verify that the chosen strategy behaves as expected


### Solution(s)

Can be found over in [lecture-devops-code](https://github.com/lucendio/lecture-devops-code/tree/master/tutorials-solutions/07_deploy-to-instance-group).
