07 - Deploy a new version to an instance group
==============================================


### Goal(s)

* at least 3 instances of the service are running
* the instance group is proxied by a load balancer
* service is updated w/o downtime


### Prerequisite(s)

* (optional) a running virtual machine (min. 2 GB memory), accessible via SSH
* a Configuration Management Tool installed on the local development machine
* (optional) a Container Runtime installed on the target system


### Task(s)

0. Ensure that the target system exists and is accessible via CM (Allocation & SSH)

1. Prepare the target system by installing and configuring the following dependencies:
  * container runtime (e.g. Podman)
  * load balancer (e.g. Nginx)
  * backing service(s) (e.g. database) if needed

2. Deploy an application (e.g. [CodiMD](https://github.com/hackmdio/codimd)) as an instance group containing 3 instances

*__NOTE:__Don't forget to make the load balancer aware of these instances, and, depending on whether the application
is stateful or not, enable session persistence.*

3. Implement one of the known deployment strategies by utilizing CM functionality 

4. Choose another version of the application and re-deploy to verify that the chosen strategy behaves as expected


### Solution(s)

Can be found over in [lecture-devops-code](https://github.com/lucendio/lecture-devops-code/tree/master/tutorials-solutions/07_deploy-to-instance-group).
