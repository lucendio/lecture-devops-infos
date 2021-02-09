12 - Create an encrypted backup 
===============================


### Objective(s)

* allocate Object Storage
* create a backup
* encrypt and upload the backup
* restore the backup


### Prerequisite(s)

* [Terraform](https://learn.hashicorp.com/tutorials/terraform/install-cli) installed locally
* credentials for a [supported](https://www.terraform.io/docs/providers/index.html#lists-of-terraform-providers)
  cloud platform
* tool that supports cryptographical operations (symmetric a/o asymmetric en- & decryption)
* (Optional) additional requirements depending on the chosen *cloud provider*


### Task(s)

1. Use Terraform to allocate some remote Object Storage (e.g. a bucket)

2. Create an encrypted off-site backup,

    * compress a folder of your choice (represent the backup artifact)
    * choose a crypto suite
    * encrypt the artifact
    * upload it

3. Restore the backup again

    * download the encrypted artifact from Object Storage
    * decrypt the artifact
    * uncompress the resulting archive
    * verify the data


### Solution(s)

Can be found over in [lecture-devops-code](https://github.com/lucendio/lecture-devops-code/tree/master/tutorials-solutions/12_create-encrypted-backup).
