---
title: 'Welcome'
---


Hey, there! 👋
==============


## What's this about?

This website is the main entry point for the *DevOps* course. It's supposed to help
you discover all the provided materials and explains how this course is organized.


## Prerequisites

* not afraid of the terminal or command-line interfaces
* [prior knowledge]({{< ref "/guide/prior-knowledge" >}}) of Unix-like operating systems (e.g. `$PATH`, file permissions)
* some experience with Version Control Systems (e.g. [Git](https://git-scm.com/doc))

{{< hint info >}}
Experience from previous semesters show that working on a Windows machine
might become tedious and could even result in data loss (invoking `make` in path
containing white spaces). For possible solutions, please refer to the
[FAQ]({{< ref "/faq#linux-on-windows-workstation" >}})
{{< /hint >}}

[^1]: there is a [section in the guide]({{< ref "/guide/prior-knowledge" >}}) to help you get up to speed, if you're
missing some pieces


## How to get started?

Have a look into the FAQ section: [Where do I even start?]({{< ref "/faq/#where-do-i-start" >}}).
Or maybe you are already overwhelmed? No worries, try [How to learn DevOps](https://www.youtube.com/watch?v=Cthla7KqU04) 🎬.


## Content overview

* [X] [Topics]({{< ref "/lessons" >}})
* [X] [Assignments]({{< ref "/assignments" >}})
* [X] [Description of each deliverable]({{< ref "/assignments/deliverables" >}}) 
* [X] [Grading]({{< ref "/grading" >}})
* [X] [A Guide]({{< ref "/guide" >}})
* [X] [Exercises]({{< ref "/exercises">}})
* [X] [FAQ]({{< ref "/faq" >}})
* [X] [Links]({{< ref "/links" >}})
* [X] [Glossary]({{< ref "/glossary" >}})
* [ ] Slides


## External resources/repositories

* [Education Kubernetes Cluster Documentation](https://gitlab.bht-berlin.de/ris/doku/-/wikis/educluster) 📖
* [showcase](https://gitlab.bht-berlin.de/fb6-wp11-devops/showcase)
* [Tutorial solutions](https://github.com/lucendio/lecture-devops-code/tutorials)
* [code examples](https://github.com/lucendio/lecture-devops-code)
  * comprehensive examples
    * Jenkins running in Containers (Server & Agent)
    * ...
  * snippets for different technologies, such as:
    * OpenTofu + DigitalOcean
    * OpenTofu + AWS
    * Vagrant + VirtualBox
    * Ansible
    * ...
* [*webservice* (example workload/application)](https://gitlab.bht-berlin.de/fb6-wp11-devops/webservice)
  * simple web service exposing a RESTful-ish API via HTTP
  * written in Go
  * optional backing service (database): Redis
