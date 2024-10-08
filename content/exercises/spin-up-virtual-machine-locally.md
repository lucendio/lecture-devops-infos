---
title: 'Spin up virtual machine locally'
---


Spin up a virtual machine locally
=================================


## Objectives

* successfully launch a virtual machine on your local system
* connect to the virtual machine via SSH


## Prerequisites

* *Vagrant* is available on the local system
* a [supported hypervisor](https://www.vagrantup.com/docs/providers) (e.g. VirtualBox) is installed

{{< hint warning >}}
Virtualbox is not available on ARM-based macOS (e.g. M1 or M2). So, if Vagrant doesn't support another provider on
mentioned platforms, try [QEMU](https://www.qemu.org/download/) or [UTM](https://mac.getutm.app/) 
{{< /hint >}}


## Tasks

1. Choose a reasonably up-to-date *box* (machine image) from the
   [Vagrant Cloud](https://app.vagrantup.com/boxes/search?order=desc&page=1&provider=virtualbox&sort=downloads)
   or [upstream](https://cloud-images.ubuntu.com/), and launch a virtual machine based on that image
2. After the instance has booted successfully, establish an SSH connection to that machine
3. Install the [webservice](https://gitlab.bht-berlin.de/fb6-wp11-devops/webservice) example app into a virtual machine,
   expose it and confirm with your browser that the landing page is being served


## Deliverables

* `Vagrantfile` or equivalent code showing the configuration of the virtual machine 
* `terminal.log` file showing the commands and their output used to carry out the tasks


## Solution: Vagrant

*Source code can be found [here](https://github.com/lucendio/lecture-devops-code/tree/master/exercises/spin-up-virtual-machine-locally/vagrant)*


### (1) Start a virtual machine with `vagrant`

__⚡ Context: *host/workstation*__
```bash
mkdir -p my-virtual-machine && cd "${_}"
vagrant init ubuntu/focal64 && vagrant up --provider virtualbox
```

{{< hint >}}
This way, the chosen image is downloaded implicitly, if it's not already available
locally. To fetch the image manually: `vagrant box add ubuntu/jammy64`.
{{< /hint >}}


### (2) Connect via SSH to the virtual machine 

__⚡ Context: *host/workstation*__
```bash
vagrant ssh
```

You may want to stop and delete the virtual machine, after successfully logging out again:

__⚡ Context: *host/workstation*__
```bash
vagrant halt
vagrant destroy
```

{{< hint >}}
A more comprehensive example of a `Vagrantfile` can be found
[here](https://github.com/lucendio/lecture-devops-code/blob/master/scenarios/ansible/environments/local/Vagrantfile).
Or take a look into the one you just generated (`./Vagrantfile`) with the `init` sub-command.
{{< /hint >}}


### (3) Create a virtual machine that runs the *webservice* inside

Configure port forwarding for `8080` (host) to `3000` (virtual machine) and start the machine. For more details,
please refer to the [documentation](https://developer.hashicorp.com/vagrant/docs/networking/forwarded_ports).

__⚡ Context: *host/workstation*__
```bash
vagrant up
```

Build the *webservice* for Linux.

__⚡ Context: *host/workstation*__
```bash
GOOS=linux GOARCH=amd64 go build -o ./webservice ./*.go
```

Copy the artifact into the virtual machine, either by (a) using the `synced_folder` mechanism (see `Vagrantfile`), or
(b) by using SSH (see `private_network` in `Vagrantfile` to find out the IP address of the machine).

__⚡ Context: *host/workstation*__
```bash
scp -i .vagrant/machines/default/virtualbox/private_key ./webservice vagrant@10.0.42.23:~/webservice
```

{{< hint warning >}}
Vagrant generates a new SSH key pair when creating a new machine. The `scp` command utilizes the same key-based
authentication mechanism already known from `ssh`.
{{< /hint >}}

Start the webservice from within the virtual machine after connecting to it via SSH.

__⚡ Context: *host/workstation*__
```bash
vagrant ssh
# OR
ssh -i .vagrant/machines/default/virtualbox/private_key vagrant@10.0.42.23:~/webservice
```

__⚡ Context: *guest/vm*__
```bash
chmod +x ./webservice
./webservice
```

Verify that the *webservice* is indeed accessible form the *outside*.

__⚡ Context: *host/workstation*__
```bash
curl -s http://localhost:8080
# OR
curl -s http://10.0.42.23:3000
```

Result:
```
Hello, World!
```

{{< hint warning >}}
Depending on the default values of the configuration built into the `webservice` binary,
environment variables like `HOST` or `PORT` may need to be set in order to fix e.g. the 
*Connection reset by peer* error.
{{< /hint >}}


## Solution: QEMU

*Source code can be found [here](https://github.com/lucendio/lecture-devops-code/tree/master/exercises/spin-up-virtual-machine-locally/qemu)*


### (1) Start a virtual machine with `qemu` and connect via SSH

Download a disk image containing an installed OS, e.g. [Ubuntu](https://cloud-images.ubuntu.com/releases/)
and look for a file like `ubuntu-${VERSION-server-cloudimg-amd64.img`.

__⚡ Context: *host/workstation*__
```bash
curl --remote-name https://cloud-images.ubuntu.com/releases/jammy/release/ubuntu-22.04-server-cloudimg-amd64.img
```

Create two files that will be used to configure the machine via
[cloud-init](https://cloudinit.readthedocs.io/en/latest/) during
initial boot.

__⚡ Context: *host/workstation*__
```bash
mkdir -p ./cloud-init
cat <<EOF > ./cloud-init/meta-data
instance-id: my-instance
local-hostname: my-hostname
EOF
cat <<EOF > ./cloud-init/user-data
#cloud-config

users:
  - name: '{{ CHOOSE_A_USERNAME }}'
    shell: '/usr/bin/bash'
    lock_passwd: false
    sudo: 'ALL=(ALL) NOPASSWD:ALL'
    ssh_authorized_keys:
      - '{{ YOUR_SSH_PUBLIC_KEY }}'
EOF
```

Create a disk to bundle the files that are later used as cloud-init datasource:
[NoCloud](https://cloudinit.readthedocs.io/en/latest/topics/datasources/nocloud.html#nocloud)

{{< hint info >}}
Please refer to the
[datasource docs](https://cloudinit.readthedocs.io/en/latest/topics/datasources/nocloud.html#nocloud)
for information on supported filesystems, and to the internet search engine
of your least distrust for other tooling to create such disk image.
{{< /hint >}}

__⚡ Context: *host/workstation (macOS)*__
```bash
hdiutil makehybrid \
    -o ./datasource.iso ./cloud-init \
    -joliet -joliet-volume-name cidata \
    -iso -iso-volume-name cidata
```

__OR__

__⚡ Context: *host/workstation (Linux)*__
```bash
(cd ./cloud-init && genisoimage -output ./../datasource.iso -volid cidata -joliet -rock ./user-data ./meta-data)
```

Start the machine with QEMU

__⚡ Context: *host/workstation*__
```bash
qemu-system-x86_64 \
  -machine type=q35 \
  -smp 2 \
  -m 4G \
  -nic user,hostfwd=tcp::2222-:22 \
  -hda ./ubuntu-22.04-server-cloudimg-amd64.img \
  -cdrom ./datasource.iso
```

{{< hint info >}}
To stop the machine press `Ctrl+C`
{{< /hint >}}


### (2) Connect via SSH to the virtual machine 

Open up another terminal session and connect via SSH to the machine

__⚡ Context: *host/workstation*__
```bash
ssh -p 2222 {{ CHOOSE_A_USERNAME }}@127.0.0.1
```

{{< hint warning >}}
If you can't establish an SSH connect, you may need to __delete the disk image and the datasource ISO__, before
beginning from the start again.
{{< /hint >}}


### (3) Install and run the *webservice* inside a virtual machine

Stop and start the machine again, but this time with slightly different parameters (notice the additional port
forwarding?).

__⚡ Context: *host/workstation*__
```bash
qemu-system-x86_64 \
  -machine type=q35 \
  -smp 2 \
  -m 4G \
  -nic user,hostfwd=tcp::2222-:22,hostfwd=tcp::8080-:3000 \
  -hda ./ubuntu-22.04-server-cloudimg-amd64.img \
  -cdrom ./datasource.iso
```

Build the *webservice* for Linux.

__⚡ Context: *host/workstation*__
```bash
GOOS=linux GOARCH=amd64 go build -o ./webservice ./*.go
```

Copy the artifact into the virtual machine via SSH.

__⚡ Context: *host/workstation*__
```bash
scp -P 2222 ./webservice {{ CHOOSE_A_USERNAME }}@127.0.0.1:~/webservice
```

Start the *webservice* from within the virtual machine after connecting to it via SSH

__⚡ Context: *host/workstation*__
```bash
ssh -p 2222 {{ CHOOSE_A_USERNAME }}@127.0.0.1
```

__⚡ Context: *guest/vm*__
```bash
chmod +x ./webservice
./webservice
```

Verify that the *webservice* is indeed accessible form the *outside*.

__⚡ Context: *host/workstation*__
```bash
curl -s http://localhost:8080
```

Result:
```
Hello, World!
```

{{< hint warning >}}
Depending on the default values of the configuration built into the `webservice` binary,
environment variables like `HOST` or `PORT` may need to be set in order to fix e.g. the 
*Connection reset by peer* error.
{{< /hint >}}
