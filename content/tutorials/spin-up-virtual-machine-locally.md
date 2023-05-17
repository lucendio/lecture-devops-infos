---
title: 'Spin up a virtual machine locally'
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
Virtualbox is not available on ARM-based macos (e.g. M1 or M2). So, if Vagrant doesn't support another provider on
mentioned platforms, try [QEMU](https://www.qemu.org/download/) or [UTM](https://mac.getutm.app/) 
{{< /hint >}}


## Tasks

1. Choose a reasonably up-to-date *box* (machine image) from the
   [Vagrant Cloud](https://app.vagrantup.com/boxes/search?order=desc&page=1&provider=virtualbox&sort=downloads)
   or [upstream](https://cloud-images.ubuntu.com/), and launch a virtual machine based on that image. After the
   instance has booted successfully, establish an SSH connection to that machine.   
2. Install *Nginx* into a virtual machine, expose it to the host and confirm with your browser that
   the default page is being served.


## Solution: Vagrant

*Source code can be found [here](https://github.com/lucendio/lecture-devops-code/tree/master/tutorials/02_spin-up-virtual-mcachine-locally/vagrant)*

### (1) Start a virtual machine with `vagrant` and connect via SSH

__⚡ Context: *host/workstation*__
```bash
mkdir -p my-virtual-machine && cd "${_}"
vagrant init ubuntu/focal64 && vagrant up --provider virtualbox
```

{{< hint >}}
This way, the chosen image is downloaded implicitly, if it's not already available
locally. To fetch the image manually: `vagrant box add ubuntu/focal64`.
{{< /hint >}}

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

*A more comprehensive example of a `Vagrantfile` can be found
[here](https://github.com/lucendio/lecture-devops-code/blob/master/scenarios/ansible/environments/local/Vagrantfile).
Or take a look into the one you just generated (`./Vagrantfile`) with the `init` sub-command for more
information.


### (2) Create a virtual machine that runs Nginx inside

Provision a virtual machine, install Nginx and configure port forwarding for `8080` (host) to `80` (virtual machine).
For more details, please refer to the [source](https://github.com/lucendio/lecture-devops-code/blob/master/tutorials/02_spin-up-virtual-mcachine-locally/vagrant/Vagrantfile).

__⚡ Context: *host/workstation*__
```bash
cd ./webserver
vagrant up
```

Verify that Nginx is running and returns its default page

__⚡ Context: *host/workstation*__
```bash
curl -s http://localhost:8080 | grep nginx
```

Result:
```
<title>Welcome to nginx!</title>
...
```

## Solution: QEMU

*Source code can be found [here](https://github.com/lucendio/lecture-devops-code/tree/master/tutorials/02_spin-up-virtual-mcachine-locally/qemu)*

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

Open up another terminal session and connect via SSH to the machine

__⚡ Context: *host/workstation*__
```bash
ssh -p 2222 {{ CHOOSE_A_USERNAME }}@0.0.0.0
```

{{< hint warning >}}
If you can't establish an SSH connect, you may need to __delete the disk image and the datasource ISO__, before
beginning from the start again.
{{< /hint >}}


### (2) Create a virtual machine that runs Nginx inside

{{< hint info >}}
Download the disk image again to reset the state of the machine.
{{< /hint >}}

Then, extend the cloud-init configuration file

__⚡ Context: *host/workstation*__
```bash
cat <<EOF >> ./cloud-init/user-data

write_files:
  - path: '/etc/systemd/resolved.conf.d/qemu-enable-dns.conf'
    content: |
      [Resolve]
      DNS=9.9.9.9
      FallbackDNS=8.8.8.8

runcmd:
  - [ systemctl, restart, systemd-resolved ]
EOF
```

Delete the existing datasource image (see 3.1) and rebuild it again to include the adjusted configuration file.

Start the machine with QEMU

__⚡ Context: *host/workstation*__
```bash
qemu-system-x86_64 \
  -machine type=q35 \
  -smp 2 \
  -m 4G \
  -nic user,hostfwd=tcp::2222-:22,hostfwd=tcp::8080-:80 \
  -hda ./ubuntu-22.04-server-cloudimg-amd64.img \
  -cdrom ./datasource.iso
```

Connect via SSH to the machine like before. Then, install Nginx

__⚡ Context: *guest/vm*__
```bash
sudo apt update
sudo apt install -y nginx
```

Verify that Nginx runs and is returning its default page

__⚡ Context: *host/workstation*__
```bash
curl -s http://localhost:8080 | grep nginx
```

Result:
```html
<title>Welcome to nginx!</title>
...
```
