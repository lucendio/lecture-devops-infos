09 - Manage objects and deploy with Helm
========================================


### Objective(s)

* write your own Helm chart
* use Helm to deploy and update an application
* encrypt Helm secrets with `sops` 


### Prerequisite(s)

* a reachable Kubernetes cluster
* working credentials to authenticate against the cluster
* `kubectl` is installed on your workstation to interact with the cluster
* `helm` is installed on your workstation
* `sops` is installed on your workstation (and optionally [GPG](https://www.gnupg.org/download/))
* [Sops/Helm plugin](https://github.com/jkroepke/helm-secrets) is installed 


### Task(s)

0. Combine the effort of previous tutorials and create a Helm chart
    
    * manage and update a containerized application (07)
    * write Kubernetes Objects (08)

1. Write a chart from scratch including

    * meta files such as `Chart.yaml`
    * `./templates` based on static Kubernetes object files
    * `./templates/_helpers.tpl` (functions, available in templates)
    * `values.yaml` and `secrets.yaml`
    
    __NOTE:__ [The Chart Template Developer's Guide](https://helm.sh/docs/chart_template_guide/) 

2. Use Sops to encrypt sensitive values and re-deploy using Helm to verify that the decryption during runtime works 


### Solution(s)

Can be found over in [lecture-devops-code](https://github.com/lucendio/lecture-devops-code/tree/master/tutorials-solutions/09_deploy-workload-with-helm-with-kubernetes).
