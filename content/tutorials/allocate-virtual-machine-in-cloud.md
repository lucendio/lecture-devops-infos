---
title: 'Allocate virtual machine in cloud'

TODO:
  - solution with Pulumi
---


Allocate a virtual machine in the cloud
=======================================


## Objectives

* allocate a virtual machine in the *cloud*
* successfully connect to tha virtual machine via SSH


## Prerequisites

* [OpenTofu](https://opentofu.org/docs/intro/install/) / [Terraform](https://learn.hashicorp.com/tutorials/terraform/install-cli)
  installed locally
* credentials for a [supported](https://registry.terraform.io/browse/providers)
  cloud platform
* additional dependencies depending on the chosen *cloud provider*


## Tasks

1. Create an account for a cloud provider of your choice
   (see [FAQ]({{< ref "/faq/cloud-and-infrastructure#which-cloud-provider-should-i-use">}}))
2. Configure the credentials locally (procedure is different for each cloud provider)
3. Generate an SSH key-pair & define an *OpenTofu* resource to manage the SSH key
4. Define a *OpenTofu* resource to allocate a virtual machine - according to your provider, e.g.
    * AWS: [`aws_instance`](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/instance)
    * DigitalOcean: [`digitalocean_droplet`](https://registry.terraform.io/providers/digitalocean/digitalocean/latest/docs/resources/droplet)
    * Hetzner: [`hetzner_server`](https://registry.terraform.io/providers/hetznercloud/hcloud/latest/docs/resources/server)
    * GCP: [`google_compute_instance`](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/compute_image)
    * Azure: [`azurerm_virtual_machine`](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/virtual_machine)
5. Ensure that the machine has a public IP assigned and incoming traffic to the SSH & an HTTP port are
   allowed (may require additional OpenTofu resource)
6. Use the private part of the previously generated SSH key and connect to the allocated machine
7. Install the [webservice](https://gitlab.bht-berlin.de/fb6-wp11-devops/webservice) into the machine and confirm that
   the default page is being served
8. Delete all resource again


## Deliverables

* source code
* `terminal.log` file showing the commands and their output used to carry out the tasks


## Solution: AWS

*Source code can be found
[here](https://github.com/lucendio/lecture-devops-code/tree/master/tutorials/allocate-virtual-machine-in-cloud/aws).*


### (2) Set up cloud provider and initialize the OpenTofu root module

{{< hint info >}}
At this point, the necessary cloud provider credentials (e.g. [AWS](https://registry.terraform.io/providers/hashicorp/aws/latest/docs))
must have been already configured. For AWS,

* set a *region* - in `provider.tf` or via environment `AWS_DEFAULT_REGION` (note that if AWS Academy Program account
  is being used, `region` must be set to `"us-east-1"`)
* obtain credentials:
  * if eligible see FAQ on
    [how to get credentials provided by AWS Academy]({{< ref "/faq/cloud-and-infrastructure#how-to-get-access-to-aws-and-unlock-aws-academy" >}}) 
  * you may want to
    [use environment variables or stick with the default location `~/.aws/[config, credentials]`](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html)*  
{{< /hint >}}

```bash
tofu init
```


### (3) Generate an SSH key pair

```bash
mkdir -p ./.ssh
ssh-keygen -t ed25519 -a 230 -C "operator" -N "" -f ./.ssh/operator
chmod 600 ./.ssh/operator*
```

Use an *input* variable and a `aws_key_pair` resource to manage the public part of the SSH key
with OpenTofu to later be able adding it to the machine.


### (4) Write configuration code in order to create a machine

* in AWS terminology, a machines is called a [aws_instance](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/instance)
* determine the cheapest [instance type](https://aws.amazon.com/ec2/pricing/on-demand/)
* find the right image via:
  * CLI: [`aws ec2 describe-images`](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-images.html)
  * OpenTofu data source [aws_ami](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/ami)

```bash
tofu apply \
  -var 'sshPublicKeyPath=./.ssh/operator.pub'
```

In order to find out the public IP of the machine, define an *output* variable and use the
CLI to access the IP.

```bash
tofu output 'instanceIPv4'
```

### (5) Enable ingress and egress traffic

By default, EC2 instances are assigned to a *Virtual Private Cloud*, which can be seen as a
default private network, hence no incoming (ingress) or outgoing (egress) traffic is initially
allowed. To change that, use a resource called `aws_security_group`. Don't forget to allow SSH
(22) and some HTTP (e.g. 8080) port.


### (6) Connect to the machine via SSH

```bash
ssh -i ./.ssh/operator -l ubuntu $(tofu output -raw 'instanceIPv4')
```

{{< hint info >}}
The default user of the cloud image is `ubuntu`  
{{< /hint >}}


### (7) Install the webservice and verify that it's accessible from the internet 

__⚡ Context: *workstation*__
```bash
scp -i ./.ssh/operator ./werservice/artifact.bin ubuntu@$(tofu output -raw 'instanceIPv4'):~/webservice
```

__⚡ Context: *virtual machine*__
```bash
HOST=0.0.0.0 PORT=8080 ./webservice
```

__⚡ Context: *workstation*__
```bash
curl -s http://$(tofu output -raw 'instanceIPv4'):8080
```

Result:
```
Hello, World!
```


### (8) Clean up afterwards (!)

__⚡ Context: *workstation*__
```bash
tofu destroy
```


## Solution: GCP

*Source code can be found
[here](https://github.com/lucendio/lecture-devops-code/tree/master/tutorials/allocate-virtual-machine-in-cloud/gcp).*


### (2) Set up cloud provider and initialize the OpenTofu root module

* [choose a zone](https://cloud.google.com/compute/docs/regions-zones)
* find out the id of your default project or create one
* obtain credentials:
  * create a Google Account if you don't have one yet
  * log in and add your coupon to [Google Cloud](https://console.cloud.google.com/education)
  * enable billing for [*Compute Engine*](https://console.cloud.google.com/compute) API
  * it's recommended to [create a service account](https://cloud.google.com/docs/authentication/production#create_service_account)
    instead of authenticating your personal account through the `gcloud` CLI (for this tutorial, the role
    *Compute Engine > Compute Admin* must be assigned to the service account 
  * for authentication methods and generating a `KEYFILE_JSON` please refer to the
    [Terraform provider documentation](https://registry.terraform.io/providers/hashicorp/google/latest/docs/guides/provider_reference#authentication-configuration)

```bash
tofu init \
  -var 'projectID={{ YOUR_PROJECT_ID }}' \
  -var 'gcpCredentialsFilePath={{ REPLACE_WITH_KEYFILE_JSON_PATH }}'
```


### (3) Generate an SSH key pair

```bash
mkdir -p ./.ssh
ssh-keygen -t ed25519 -a 230 -C "operator" -N "" -f ./.ssh/operator
chmod 600 ./.ssh/operator*
```

Use *input* variables in order to later inject the public part of the SSH key into the machine.


### (4) Write configuration code in order to create a machine

* in GCP terminology, a machines is called a [google_compute_instance](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/compute_instance)
* determine the cheapest [machine type](https://cloud.google.com/compute/docs/machine-types)
* find the right image via:
  * CLI: `gcloud compute images list` (log in if needed: `gcloud auth login --no-launch-browser`)
  * OpenTofu data source [google_compute_image](https://registry.terraform.io/providers/hashicorp/google/latest/docs/data-sources/compute_image)

```bash
tofu apply \
  -var 'projectID={{ YOUR_PROJECT_ID }}' \
  -var 'gcpCredentialsFilePath={{ REPLACE_WITH_KEYFILE_JSON_PATH }}' \
  -var 'sshPublicKeyPath=./.ssh/operator.pub'
```

In order to find out the public IP of the machine, define an *output* variable and use the
CLI to access the IP.

```bash
tofu output 'instanceIPv4'
```


### (5) Enable ingress and egress traffic

By default, *compute instances* are assigned to a *Virtual Private Cloud*, which can be seen as a
default private network, hence no incoming (ingress) or outgoing (egress) traffic is initially
allowed. To change that, use resource called `google_compute_network` and `google_compute_firewall`.
Don't forget to allow SSH (22) and some HTTP (e.g. 8080) port.


### (6) Connect to the machine via SSH

```bash
ssh -i ./.ssh/operator -l ubuntu $(tofu output -raw 'instanceIPv4')
```

{{< hint info >}}
The login name `ubuntu` may vary depending on which user name you chose
when adding the public part of the SSH key to the *compute instance* resource.
{{< /hint >}}


### (7) Install the webservice and verify that it's accessible from the internet 

__⚡ Context: *workstation*__
```bash
scp -i ./.ssh/operator ./werservice/artifact.bin ubuntu@$(tofu output -raw 'instanceIPv4'):~/webservice
```

__⚡ Context: *virtual machine*__
```bash
HOST=0.0.0.0 PORT=8080 ./webservice
```

__⚡ Context: *workstation*__
```bash
curl -s http://$(tofu output -raw 'instanceIPv4'):8080
```

Result:
```
Hello, World!
```


### (8) Clean up afterwards (!)

__⚡ Context: *workstation*__
```bash
tofu destroy \
  -var 'projectID={{ YOUR_PROJECT_ID }}' \
  -var 'gcpCredentialsFilePath={{ REPLACE_WITH_KEYFILE_JSON_PATH }}' \
  -var 'sshPublicKeyPath=./.ssh/operator.pub'
```
