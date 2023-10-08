---
title: 'Allocate a virtual machine in the cloud'
---


Allocate a virtual machine in the cloud
=======================================


## Objectives

* allocate a virtual machine in the *cloud*
* successfully connect to tha virtual machine via SSH


## Prerequisites

* [OpenTofu](https://opentofu.org/docs/cli/) / [Terraform](https://learn.hashicorp.com/tutorials/terraform/install-cli) installed locally
* credentials for a [supported](https://registry.terraform.io/browse/providers)
  cloud platform
* *[optional]* additional requirements depending on the chosen *cloud provider*


## Tasks

1. Create an account for a cloud provider of your choice
   (see [FAQ]({{< ref "/faq/cloud-and-infrastructure#which-cloud-provider-should-i-use">}}))
2. Configure the credentials locally (process is different for each cloud provider)
3. Generate an SSH key-pair & (optionally) Define a *Terraform* resource to manage the SSH key
4. Define a *Terraform* resource to allocate a virtual machine - according to your provider, e.g.
    * AWS: [`aws_instance`](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/instance)
    * DigitalOcean: [`digitalocean_droplet`](https://registry.terraform.io/providers/digitalocean/digitalocean/latest/docs/resources/droplet)
    * Hetzner: [`hetzner_server`](https://registry.terraform.io/providers/hetznercloud/hcloud/latest/docs/resources/server)
    * GCP: [`google_compute_instance`](https://registry.terraform.io/providers/hashicorp/google/latest/docs/resources/compute_image)
    * Azure: [`azurerm_virtual_machine`](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/virtual_machine)
5. Ensure that the machine has a public IP assigned and incoming traffic to the SSH & (optionally) HTTP port are
   allowed (may require additional Terraform resource)
6. Use the private part of the previously generated SSH key and connect to the allocated machine
7. *[optional]* Install *Nginx* into the instance and confirm that the default page is being served


## Solution: AWS

*Source code can be found
[here](https://github.com/lucendio/lecture-devops-code/tree/master/tutorials/03_allocate-machine-in-cloud/aws).*

### (1) Generate an SSH key pair

```bash
mkdir -p ./.ssh
ssh-keygen -t rsa -b 4096 -C "operator" -N "" -f ./.ssh/operator
chmod 600 ./.ssh/operator*
```

### (2) Set up cloud provider and initialize the Terraform root module

{{< hint info >}}
At this point, the necessary cloud provider credentials (e.g. [AWS](https://registry.terraform.io/providers/hashicorp/aws/latest/docs))
must have already been configured. For AWS,

* set a *region* - in `provider.tf` or via environment `AWS_DEFAULT_REGION` (note that if AWS Academy Program account
  is being used, `region` must be set to `"us-east-1"`)
* obtain credentials:
  * if eligible see FAQ on
    [how to get credentials provided by AWS Academy]({{< ref "/faq/cloud-and-infrastructure#how-to-get-access-to-aws-and-unlock-aws-academy" >}}) 
  * you may want to
    [use environment variables or stick with the default location `~/.aws/[config, credentials]`](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html)*  
{{< /hint >}}

```bash
terraform init
```

### (3) Write configuration code in order to create a machine

* determine the cheapest [instance type](https://aws.amazon.com/ec2/pricing/on-demand/)
* find the right image: [`aws ec2 describe-images`](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-images.html)
  (in case of AWS Academy, check the *EC2* part in the section *Service usage and other restrictions* of the Readme
  mentioned [here]({{< ref "/faq/cloud-and-infrastructure#how-to-get-access-to-aws-and-unlock-aws-academy" >}}))

```bash
terraform apply \
  -var 'sshPublicKeyPath=./.ssh/operator.pub'
```

You may want to *output* the public IP of that machine:

```bash
terraform output 'instanceIPv4'
```

### (4) Connect to the machine via SSH

```bash
ssh -i ./.ssh/operator -l ubuntu $(terraform output -raw 'instanceIPv4')
```

### (5) *[optional]* Install Nginx and verify that it's running 

__⚡ Context: *virtual machine*__
```bash
sudo apt update
sudo apt install -y nginx
```

__⚡ Context: *workstation*__
```bash
curl -s http://$(terraform output -raw 'instanceIPv4'):80 | grep nginx
```

Result:
```html
<title>Welcome to nginx!</title>
...
```

### (6) Clean up afterwards (!)

__⚡ Context: *workstation*__
```bash
terraform destroy
```


## Solution: GCP

*Source code can be found
[here](https://github.com/lucendio/lecture-devops-code/tree/master/tutorials/03_allocate-machine-in-cloud/gcp).*

### (1) Generate an SSH key pair

```bash
mkdir -p ./.ssh
ssh-keygen -t rsa -b 4096 -C "operator" -N "" -f ./.ssh/operator
chmod 600 ./.ssh/operator*
```

### (2) Set up cloud provider and initialize the Terraform root module

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
terraform init \
  -var 'projectID={{ YOUR_PROJECT_ID }}' \
  -var 'gcpCredentialsFilePath={{ REPLACE_WITH_KEYFILE_JSON_PATH }}'
```

### (3) Write configuration code in order to create a machine

* determine the cheapest [machine type](https://cloud.google.com/compute/docs/machine-types)
* find the right image: `gcloud compute images list` (log in if needed: `gcloud auth login --no-launch-browser`)

```bash
terraform apply \
  -var 'projectID={{ YOUR_PROJECT_ID }}' \
  -var 'gcpCredentialsFilePath={{ REPLACE_WITH_KEYFILE_JSON_PATH }}' \
  -var 'sshPublicKeyPath=./.ssh/operator.pub'
```

You may want to *output* the public IP of that machine:

```bash
terraform output 'instanceIPv4'
```

### (4) Connect to the machine via SSH

```bash
ssh -i ./.ssh/operator -l ubuntu $(terraform output -raw 'instanceIPv4')
```

### (5) *[optional]* Install Nginx and verify that it's running 

__⚡ Context: *virtual machine*__
```bash
sudo apt update
sudo apt install -y nginx
```

__⚡ Context: *workstation*__
```bash
curl -s http://$(terraform output -raw 'instanceIPv4'):80 | grep nginx
```

Result:
```html
<title>Welcome to nginx!</title>
...
```

### (6) Clean up afterwards (!)

__⚡ Context: *workstation*__
```bash
terraform destroy \
  -var 'projectID={{ YOUR_PROJECT_ID }}' \
  -var 'gcpCredentialsFilePath={{ REPLACE_WITH_KEYFILE_JSON_PATH }}' \
  -var 'sshPublicKeyPath=./.ssh/operator.pub'
```
