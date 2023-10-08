---
title: 'Provision Kubernetes'
---


Provision Kubernetes
====================


## Objectives

* allocate infrastructure (automated)
* export information from the process and feed it into ...
* an automated Kubernetes deployment


## Prerequisites

* [OpenTofu](https://opentofu.org/docs/cli/) / [Terraform](https://learn.hashicorp.com/tutorials/terraform/install-cli) installed locally
* a Configuration Management Tool is installed locally
* `kubectl` is installed on your workstation to interact with the cluster


## Tasks

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


## Solution

*Please note that this solution makes use of [Kubespray](https://github.com/kubernetes-sigs/kubespray)
and AWS. Source code can be found
[here](https://github.com/lucendio/lecture-devops-code/tree/master/tutorials/10_provision-kubernetes).*


### (0) Preparations

Make sure you have installed the following dependencies on your workstation:

* Python >= 3.7
* Terraform >= 0.14
* kubectl
* *[optional]* [`jq`](https://stedolan.github.io/jq/download/) & [`yq`](https://kislyuk.github.io/yq/)

Then, clone the Kubspray repository

```bash
git clone https://github.com/kubernetes-sigs/kubespray
```

and install its dependencies.

```bash
pip install -r ./kubespray/requirements.txt
```


### (1) Allocate the necessary infrastructure

Use Terraform to allocate some resources on AWS. Either by reusing some of the code from
*Kubespray* ([`kubespray/contrib/terraform/aws`](https://github.com/kubernetes-sigs/kubespray/tree/master/contrib/terraform/aws)
or use the code residing in
[this folder](https://docs.ansible.com/ansible/latest/collections/community/general/docker_container_module.html).

0. Generate an SSH key-pair

    ```bash
    mkdir -p ./.ssh
    ssh-keygen -t rsa -b 4096 -C "operator" -N "" -f ./.ssh/operator
    chmod 600 ./.ssh/operator*
    ```

1. Initialize and run Terraform

    ```bash
    terraform init
    ```
    {{< hint >}}
Please refer to tutorials like [*Allocate a virtual machine in the cloud*]({{< relref "./allocate-machine-in-cloud" >}})
for details on how to manage infrastructure resources with Terraform.
    {{< /hint >}}

    ```bash
    terraform apply -var 'sshPublicKeyPath=./.ssh/operator.pub'
    ```

    {{< hint danger >}}
All of this Terraform code for AWS is by __NO__ means production-ready. Normally, the Kubernetes cluster
would be in a private subnet with restrictive network policies, a public load balancer in front and a bastion/jump
host to access the machines via SSH.
    {{< /hint >}}


### (2) Install Kubernetes on top it

0. Get familiar with Kubespray

    * [Getting started](https://github.com/kubernetes-sigs/kubespray/tree/a923f4e7c0692229c442b07a531bfb5fc41a23f9/docs/getting-started.md#building-your-own-inventory)
    * [Check out available variables](https://github.com/kubernetes-sigs/kubespray/tree/a923f4e7c0692229c442b07a531bfb5fc41a23f9/inventory/sample)

1. Assemble an inventory

    This is a tricky part. The inventory can be seen as an "interface" between Terraform and Ansible. In this
    solution, an inventory is (partially)
   [generated from a Terraform `output` resource](https://docs.ansible.com/ansible/latest/collections/community/docker/docker_container_module.html).
    ```bash
    terraform output -json 'hosts' > ./inventory/hosts.yaml
    ```
    {{< hint info >}}
This only works, because JSON is a sub-set of YAML.
    {{< /hint >}}   

    Whereas, the [example in Kubespray](https://github.com/kubernetes-sigs/kubespray/blob/a923f4e7c0692229c442b07a531bfb5fc41a23f9/contrib/terraform/aws/templates/inventory.tpl),
    instead, uses a Terraform template resource. And, it [provides even more assistance](https://github.com/kubernetes-sigs/kubespray/blob/master/docs/getting-started.md#building-your-own-inventory)
    with creating an inventory.

2. Configure the machines

    Run the playbook together with the inventory you have put together.
    ```bash
    ansible-playbook --inventory ./inventory ./kubespray/cluster.yml
    ```
 

### (3) Verify installation

1. Set the `kubectl` credentials, that were downloaded by Kubespray  
    
    ```bash
    export KUBECONFIG=$(pwd)/admin.conf
    ```

2. If necessary, replace the IP/host of the server URL in the kubeconfig file   

    ```bash
    export KUBE_API_HOST=$(jq -r '.all.hosts.instance1.ansible_host' ./inventory/hosts.yaml)
    yq --in-place -y --yaml-output \
      ".clusters[0].cluster.server = \"https://{{ KUBE_API_HOST }}:6443\"" \
      ./admin.conf
    ```

3. Try and connect to the kube-api server

    ```bash
    kubectl get pods -o wide --all-namespaces
    ```

4. Gather information and view the dashboard in your browser. (check the
   [docs](https://github.com/kubernetes-sigs/kubespray/blob/18efdc2c51c5881c8647c06d02f8b505c5712876/docs/getting-started.md#accessing-kubernetes-dashboard)
   for some alternatives)

    a) Adjust the dashboard configuration by replacing `type: ClusterIP` with `type: NodePort`    
    ```shell
    kubectl edit service kubernetes-dashboard -n kube-system
    ```
   
    b) Find out on which machine the dashboard *Pod* is scheduled

    ```shell
    kubectl get pods -n kube-system -o wide
    ```
    ```
    NAMESPACE     NAME                                          READY   STATUS    RESTARTS   AGE    IP               NODE        NOMINATED NODE   READINESS GATES
    ...
    kube-system   kubernetes-dashboard-5f87bdc77d-mv7hq         1/1     Running   0          169m   10.233.90.3      node1       <none>           <none>
    ...
    ```
   
    c) Find out which *NodePort* is mapped to the *Service*

    ```shell
    kubectl get services -n kube-system -o wide
    ```
    ```
    NAMESPACE     NAME                        TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)                  AGE    SELECTOR
    ...
    kube-system   kubernetes-dashboard        NodePort    10.233.23.235   <none>        443:32029/TCP            167m   k8s-app=kubernetes-dashboard
    ```

5. Log in to the dashboard

    a) Obtain the token
    ```shell
    kubectl describe secret kubernetes-dashboard-token -n kube-system | grep 'token:' | grep -o '[^ ]\+$'
    ```

    b) Open `https://{{ PUBLIC_MACHINE_IP }}:32029` in your browser and paste the token.


6. Don't forget to clean up at the end

    ```shell
    terraform destroy -auto-approve
    ```
