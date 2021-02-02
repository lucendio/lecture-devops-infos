10 - Provision Kubernetes
=========================


### Objective(s)

* allocate infrastructure (automated)
* export information from the process and feed it into ...
* an automated Kubernetes deployment


### Prerequisite(s)

* [Terraform](https://learn.hashicorp.com/tutorials/terraform/install-cli) is installed locally
* a Configuration Management Tool is installed locally
* `kubectl` is installed on your workstation to interact with the cluster


### Task(s)

1. Allocate infrastructure

    * at least two machines (1x *control-plane* and 1x *node*)
    * accessible via SSH

2. Install Kubernetes on top

3. Verify whether Kubernetes has been installed successfully

    * configure `KUBECONFIG` and run `kubectl` to verify
    * [write and apply a simple *Deployment*](https://kubernetes.io/docs/tasks/run-application/run-stateless-application-deployment/#creating-and-exploring-an-nginx-deployment)
      associated to a *Service* that is configured with the `type: NodePort`
    * find out on which node (in case of multiple ones) the workload is scheduled
    * find out the port to which the *Service* is bound to
    * put it all together and open the URL in a browser


### Solution(s)

Can be found over in [lecture-devops-code](https://github.com/lucendio/lecture-devops-code/tree/master/tutorials-solutions/10_provision-kubernetes).
