---
bookFlatSection: true
bookCollapseSection: false

title: 'Tool chain'
weight: 5
---


Tool chain
==========

Throughout this course you may need to install some software tools for the following purposes:

1. Virtualization software (aka hypervisor)
2. Container runtime
3. Tool to define virtual machines locally following the *as-code approach
4. Tool to define cloud infrastructure following the *as-code approach
5. Configuration Management Software
6. Kubernetes toolchain
7. Cloud provider command-line interface
8. Go

All the technologies referred to during this course are open source, including the operating system your
application will be deployed on - Linux to be precise. Therefore, it's recommend to also use Linux
as your work environment - or at least a POSIX-compliant system. If the host system of your workstation is not
already Linux-based continue read on to find out about possible solutions.

__Windows (Architecture: x86)__

[WSL2](https://docs.microsoft.com/en-us/windows/wsl/install) may work for some tools, but for others like *VirtualBox*
or *containers* in general (which it's based on Linux technologies) it won't work, or in case of Ansible it's
[not recommended](https://docs.ansible.com/ansible/latest/user_guide/windows_faq.html#can-ansible-run-on-windows).
See FAQ: [*My workstation runs Microsoft Windows*]({{< ref "/faq#unix-on-windows-workstation" >}}). Experience from
past semesters show that using a hypervisor like *VirtualBox* directly is the cleaner and less error-prone approach.

__macOS__

It's already POSIX-compliant, so usually most of the tools work just fine. Even if the hardware is already ARM-based
(Apple Silicon, M* SoC), pre-build binaries should be available.

__OS / Technology compatibility:__

| Technology/OS | Purpose                  | Windows  | macOS (x86) | macOS (ARM) | Linux  |
|---------------|--------------------------|:--------:|:-----------:|:-----------:|:------:|
| VirtualBox    | Virtualization           |    ✅    |      ✅     |     ✅      |   ✅   |
| QEMU          | Virtualization           |    ✅    |      ✅     |     ✅      |   ✅   |
| UTM           | Virtualization           |    🚫    |      ✅     |     ✅      |   🚫   |
| Lima          | Virtualization           |    🚫    |      ✅     |     ✅      |   ✅   |
| Ansible       | Configuration Management |    🚫    |      ✅     |     ✅      |   ✅   |


## Installation guide

In general, it's recommended to consult a software package manager supported by your operating system in order
to search for and install any software:

* Windows: [Chololatey](https://community.chocolatey.org/)
* macOS: [Homebrew](https://brew.sh/)
* Linux: [Apt](https://ubuntu.com/server/docs/package-management), [DNF](https://docs.fedoraproject.org/en-US/quick-docs/dnf/), [Nix](https://nixos.org/download#download-nix) etc.)

{{< hint info >}}
The selection below may be opinionated and takes the exercise solutions into account. If you feel adventures, you may
want to take a look at the [software list]({{< ref "/guide/software">}}) and diverge from the software mentioned below.
{{< /hint >}}


1. Virtualization software (aka hypervisor)

   a) [VirtualBox](https://www.virtualbox.org/wiki/Downloads) is recommended, because it's supported across all major
   operating systems and architectures (x86 & ARM)

   b) [QEMU](https://www.qemu.org/download/) (note that QEMU is not actually a hypervisor but an emulator,
   though, the result - a virtual machine - is the same)


2. Container runtime: [Podman](https://podman.io/)

   a) in case the operating system workspace is not Linux-based one would need to install
   the [software](https://podman.io/docs/installation) in  a virtual machine (see 1.)

   b) a more convenient but also rather black-boxy approach:
   [Podman Desktop](https://podman-desktop.io/docs/Installation)


3. Declaratively manage cloud infrastructure with Code

   a) [OpenTofu](https://opentofu.org/docs/cli/) / [Terraform](https://learn.hashicorp.com/tutorials/terraform/install-cli)

   b) [Pulumi](https://www.pulumi.com/docs/install/)


4. Declaratively manage virtual machine locally with Code: [Vagrant](https://www.vagrantup.com/docs/installation)

   Please note that *VirtualBox* is the only cross-platform __and__ open-source
   [provider](https://developer.hashicorp.com/vagrant/docs/providers) available for Vagrant that also support x86 & ARM architecture

   {{< hint warning >}}
   When choosing a box from the [catalog](https://portal.cloud.hashicorp.com/vagrant/discover?architectures=arm64&providers=virtualbox),
   make sure the required architecture (`amd64` or `arm64`) is available (e.g.
   [bento/ubuntu-24.04](https://portal.cloud.hashicorp.com/vagrant/discover/bento/ubuntu-24.04))
   {{< /hint >}}


5. Configuration Management: [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/)

   It's witten in Python, so it's recommended to use `pip` - the Python package manager (and latest Python 3 version)
   to install the latest
   [full package](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-the-ansible-community-package)


6. Kubernetes toolchain: [Client CLI](https://kubernetes.io/docs/tasks/tools/#kubectl) and [Helm CLI](https://helm.sh/docs/intro/install/)

   Since they are static binaries, there are no prerequisites, and you can pick the latest version.


7. Cloud provider command-line interface: *Amazon Web Service* and/or *Google Cloud Platform*

    * [`awscli`](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html) (v1 is also available via
      [pip](https://pypi.org/project/awscli/)); Consult the
      [FAQ section]({{< ref "/faq/cloud-and-infrastructure#how-to-get-access-to-aws-and-unlock-aws-academy" >}})
      for how to obtain credentials
      
      ```bash
      aws sts get-caller-identity
      ```

    * [`glcoud`](https://cloud.google.com/sdk/docs/install)

      ```bash
      gcloud auth login ${EMAIL_ADDRESS} --no-launch-browser 
      ```

8. Tool chain to test and build the [webservice](https://gitlab.bht-berlin.de/fb6-wp11-devops/webservice): Go

   a) again, consult a software package manager supported by the operating system

   b) [install by hand](https://go.dev/doc/install)

   {{< hint warning >}}
   Don't forget to adjust the `PATH` environment variable, and possibly set `GOROOT` and `GOPATH`
   {{< /hint >}}
