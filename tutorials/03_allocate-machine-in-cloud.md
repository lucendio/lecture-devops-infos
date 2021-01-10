03 - Allocate a virtual machine in the cloud
============================================


### Objective(s)

* allocate a virtual machine in the *cloud*
* successfully connect to tha virtual machine via SSH 


### Prerequisites

* [Terraform](https://learn.hashicorp.com/tutorials/terraform/install-cli) installed locally
* credentials for a [supported](https://www.terraform.io/docs/providers/index.html#lists-of-terraform-providers)
  cloud platform
* (Optional) additional requirements depending on the chosen *cloud provider* 


### Task(s)

1. Create an account for a cloud provider of your choice
2. Configure the credentials locally (different depending on the provider)
3. Generate an SSH key-pair
4. (optional) Define a *Terraform* resource to manage the SSH key
5. Define a *Terraform* resource to allocate a virtual machine - according to your provider, e.g.
   * AWS: [`aws_instance`](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/instance)
   * DigitalOcean: [`digitalocean_droplet`](https://registry.terraform.io/providers/digitalocean/digitalocean/latest/docs/resources/droplet)
   * Hetzner: [`hetzner_server`](https://registry.terraform.io/providers/hetznercloud/hcloud/latest/docs/resources/server)
   * GCP: [`google_compute_instance`](https://www.terraform.io/docs/providers/google/d/compute_instance.html)
   * Azure: [`azurerm_virtual_machine`](https://www.terraform.io/docs/providers/azurerm/r/virtual_machine.html)
6. (Optional) Ensure that the machine has a public IP assigned and incoming traffic to the SSH port is allowed (may
   require additional resource objects)
7. Use the private part of the previously generated SSH key and connect to the allocated machine


### Solution(s)

Can be found over in [lecture-devops-code](https://github.com/lucendio/lecture-devops-code/tree/master/tutorials-solutions/03_allocate-machine-in-cloud).
