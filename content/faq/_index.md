---
bookFlatSection: false
bookCollapseSection: true

title: 'FAQ'
weight: 8
---


Frequently Asked Questions
==========================


## Where do I even start? {id="where-do-i-start"}

The first step would be to get an overview of what [this website]({{< ref "/#content-overview" >}}) and the
[external resources]({{< ref "/#external-resourcesrepositories" >}}) provide you with. As a second step, you might want to grab
a coffee and click your way through the [lists of link collections on *DevOps*]({{< ref "/links#devops" >}}) to grasp
the scope of this subject and therewith of this course. Most of the [link collections]({{< ref "/links#devops" >}}) not
only provide a comprehensive lists of technologies but also refer to books, blogs and other knowledgeable resources,
too. Then, a next step would be to search and decide on an application that you are going to deploy as part of the
[*project*]({{< ref "/assignments/project-work" >}}) assignment, and try to get it running on your workstation.

You might also find [this roadmap](https://roadmap.sh/devops) useful. It shows you a possible learning path. Towards
the end, you may postpone the monitoring bits until the end and instead have a look at cloud-related things first.
Another learning path can be found in the [guide]({{< ref "/guide/path" >}}).


## I have no clue about DevOps. How am I supposed to write this concept?

Don't worry. It's just an initial step to get you start digging into things related to DevOps. After handing in the
(first version of the) concept you'll receive individual feedback, which you can then use to iterate over your design
and implementation. So, nothing is set in store. You can change things along the way - after some consultation first.


## My workstation runs Microsoft Windows. From where do I get a Unix-like context to work in? {id="unix-on-windows-workstation"}

You may either want to try the [*Windows Subsystem for Linux*](https://docs.microsoft.com/en-us/windows/wsl/install)
or get yourself a virtual machine running Linux on your workstation. One way to archive this is to install
[VirtualBox](https://www.virtualbox.org/wiki/Downloads) and [Vagrant](https://www.vagrantup.com/downloads)
(directly from their websites or through [Chocolatey](https://chocolatey.org/) - the Windows package manager).


## My workstation runs on ARM architecture (e.g. Apple Silicon). So, Virtualbox is not supported. What other options do I have?

When looking at [providers](https://www.vagrantup.com/docs/providers) that are officially supported by Vagrant, then 
there would only be one option: [VMware](https://www.vmware.com/products/fusion.html). But it's proprietary and not free
of charge. Aside of Vagrant, another option would be [QEMU](https://formulae.brew.sh/formula/qemu), or the GUI-based
app called [UTM](https://mac.getutm.app/).
