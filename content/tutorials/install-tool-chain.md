---
title: 'Install tool chain'
---


Install tool chain
==================


## Objectives

* successfully install the tools used in the upcoming tutorials
* verify that your credentials of your AWS Academy account work


## Remarks

* you may want consult your favourite package manager to search and install any of the tools
* for Windows workstations it is recommended to use [WSL](https://docs.microsoft.com/en-us/windows/wsl/install)
  (please refer to the [FAQ]({{< ref "/faq#unix-on-windows-workstation">}})
  for further details)


## Tasks

1. Install the *Amazon Web Service* and/or *Google Cloud Platform* CLI and verify that you can log in

    * [`awscli`](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html) (v1 is also available via [pip](https://pypi.org/project/awscli/))
      Consult the [FAQ section]({{< ref "/faq/cloud-and-infrastructure#how-to-get-access-to-aws-and-unlock-aws-academy" >}})
      for how to obtain credentials

      ```bash
      aws sts get-caller-identity
      ```

    * [`glcoud`](https://cloud.google.com/sdk/docs/install)

      ```bash
      gcloud auth login ${BHT_EMAIL} --no-launch-browser 
      ```

2. Install a container runtime of your choice (e.g. Podman, Docker)

    * depending on your host system, you might want to run containers in a virtual machine


3. Install a virtualization software (aka Hypervisor) [compatible with Vagrant and that is not named Docker)](https://www.vagrantup.com/docs/providers)

    * [VirtualBox](https://www.virtualbox.org/wiki/Downloads) is recommended, because it is supported across all major
      operating systems (on x86)


4. Install the [Terraform CLI](https://learn.hashicorp.com/tutorials/terraform/install-cli)

    * since it's a static binary, there are no prerequisites and you can pick the latest version


5. Install a tool that allows declarative definition of virtual machines locally on your workstation

    * [Vagrant](https://www.vagrantup.com/docs/installation)
    * [Virtualbox Provider for Terraform](https://registry.terraform.io/providers/terra-farm/virtualbox/latest/docs)


6. Install Ansible

    * choose the latest [community package](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-the-ansible-community-package)
      (not `ansible-[core,base]`)
    * it's recommended to use `pip` - the Python package manager (and latest Python 3 version)


7. Install the Kubernetes CLI [`kubectl`](https://kubernetes.io/docs/tasks/tools/#kubectl)

    * since it's a static binary, there are no prerequisites, and you can pick the latest version


8. Install [Helm CLI](https://helm.sh/docs/intro/install/)

    * since it's a static binary, there are no prerequisites, and you can pick the latest version
