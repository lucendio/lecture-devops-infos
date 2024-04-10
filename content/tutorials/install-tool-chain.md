---
title: 'Install tool chain'
---


Install tool chain
==================


## Objectives

* successfully install the tools used in the upcoming tutorials
* verify that your credentials of your AWS Academy account (or Google Cloud Platform) work


## Remarks

### In General

Consult any software package manager supported by your operating system to search and install any of the tools
(e.g. Windows: [Chololatey](https://community.chocolatey.org/), macOS: [Homebrew](https://brew.sh/), Linux:
[Apt](https://ubuntu.com/server/docs/package-management), [DNF](https://docs.fedoraproject.org/en-US/quick-docs/dnf/),
[Nix](https://nixos.org/download#download-nix))
 
All the technology referred to during this course is open source, which also includes the operating system your 
application will be deployed on - Linux is going to be that platform. Therefore, it's recommend to also use Linux
as your work environment - or at least a POSIX-compliant system. If the host system of your workstation is not
already Linux-based continue reading to find out about possible solutions. 

#### OS / Technology compatibility:

| Technology/OS   | Windows  | macOS (x86) | macOS (ARM) | Linux  |
|-----------------|:--------:|:-----------:|:-----------:|:------:|
| VirtualBox      |    ✅    |      ✅     |     🚫      |   ✅   |
| QEMU            |    🚫    |      ✅     |     ✅      |   ✅   |
| UTM             |    🚫    |      🚫     |     ✅      |   🚫   |
| Lima            |    🚫    |      ✅     |     ✅      |   ✅   |
| Vagrant         |    ✅    |      ✅     |     🚫      |   ✅   |
| OpenTofu        |    ✅    |      ✅     |     ✅      |   ✅   |
| Podman          |    ✅    |      ✅     |     ✅      |   ✅   |
| Ansible         |    🚫    |      ✅     |     ✅      |   ✅   |
| Minikube        |    ✅    |      ✅     |     ✅      |   ✅   |
| `kubectl`       |    ✅    |      ✅     |     ✅      |   ✅   |
| Helm            |    ✅    |      ✅     |     ✅      |   ✅   |
| `gcloud`, `aws` |    ✅    |      ✅     |     ✅      |   ✅   |
| Go (webservice) |    ✅    |      ✅     |     ✅      |   ✅   |


### Windows (Architecture: x86)

[WSL2](https://docs.microsoft.com/en-us/windows/wsl/install) may work for some tools, but for others like *VirtualBox* or
*containers* in general (it's based on Linux technologies) it won't work, or in case of Ansible it's
[not recommended](https://docs.ansible.com/ansible/latest/user_guide/windows_faq.html#can-ansible-run-on-windows).
See FAQ: [*My workstation runs Microsoft Windows*]({{< ref "/faq#linux-on-windows-workstation" >}}). Experience from
past semesters show that using a hypervisor like *VirtualBox* directly is the cleaner and less error-prone approach.

### macOS

It's already POSIX-compliant, so usually most of the tools work just fine. Even if the hardware is already ARM-based
(Apple Silicon, M* SoC), pre-build binaries should be available. See FAQ:
[My workstation runs on ARM architecture]({{< ref "/faq#linux-on-macos-workstation">}}) for more details.



## Tasks

Install the following

1. Virtualization software (aka hypervisor)
2. Container runtime
3. Tool to define cloud infrastructure following the *as-code approach
4. Tool to define virtual machines locally following the *as-code approach
5. Configuration Management Software
6. Kubernetes toolchain
7. Cloud provider command-line interface & verify that your can successfully log in


## Deliverables

*none*


## Solution

{{< hint info >}}
If you feel adventures, you may want to take a look at the [software list](({{< ref "/guide/software">}})
and diverge from the software mentioned below.
{{< /hint >}}

1. Virtualization software (aka hypervisor)

    a) [VirtualBox](https://www.virtualbox.org/wiki/Downloads) is recommended, because it is supported across all major
      operating systems (on x86)

    b) [QEMU](https://www.qemu.org/download/) or [UTM](https://mac.getutm.app/) (on Apple Silicon)


2. Container runtime: Podman
   
    a) in case the operating system workspace is not Linux-based one would need to start a virtual machine
       and install the [software](https://podman.io/docs/installation) in there (see 1.) 

    b) a more convenient but also rather black-boxy approach:
       [Podman Desktop](https://podman-desktop.io/docs/Installation)


3. Declaratively manage cloud infrastructure with Code

    a) [OpenTofu](https://opentofu.org/docs/cli/) / [Terraform](https://learn.hashicorp.com/tutorials/terraform/install-cli)

    b) [Pulumi](https://www.pulumi.com/docs/install/)


4. Declaratively manage virtual machine locally with Code: [Vagrant](https://www.vagrantup.com/docs/installation)

    Since *VirtualBox* is the only cross-platform and open-source
    [provider](https://developer.hashicorp.com/vagrant/docs/providers) supported, this tool is only
    available on x86 platforms


5. Configuration Management: [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/)

    It's witten in Python, so it's recommended to use `pip` - the Python package manager (and latest Python 3 version)
    to install the latest
    [full package](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-the-ansible-community-package).


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
      gcloud auth login ${BHT_EMAIL} --no-launch-browser 
      ```
