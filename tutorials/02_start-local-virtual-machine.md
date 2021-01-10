02 - Spin up a virtual machine locally
======================================


### Objective(s)

* successfully launch a virtual machine on your local system
* connect to the virtual machine via SSH


### Prerequisite(s)

* *Vagrant* is available on the local system
* a [supported hypervisor](https://www.vagrantup.com/docs/providers) is installed (e.g. VirtualBox)


### Task(s)

1. Choose a reasonably up-to-date *box* (machine image) form the
   [Vagrant Cloud](https://app.vagrantup.com/boxes/search?order=desc&page=1&provider=virtualbox&sort=downloads)
   and launch a virtual machine with `vagrant` based on that image. After the instance has booted successfully,
   establish an SSH connection to that machine.
   
2. Install *Nginx* into a virtual machine, expose it to the host and confirm with your browser that
   the default page is being served.


### Solution(s)

Can be found over in [lecture-devops-code](https://github.com/lucendio/lecture-devops-code/tree/master/tutorials-solutions/02_start-local-virtual-machine).
