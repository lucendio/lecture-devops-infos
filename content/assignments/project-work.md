---
title: 'Project Work'
weight: 2
---


Project Work 
============


__Description:__ Deploy and run an application in a cloud or cloud-simulated environment. Allocate the underlying
infrastructure resources. Provision all the services required for the application to function properly and to be
successfully deployed as example workload in multiple environments. Automate these processes in a reasonable manner.

__Deliverables:__ [concept]({{< ref "/assignments/deliverables/project-concept" >}}) &
                  [implementation]({{< ref "/assignments/deliverables/project-implementation" >}})

__Requirements:__ complete the [Tutorials]({{< ref "/assignments/tutorials" >}}) assignment

{{< hint warning >}}
Please note, that it's not required to successfully & completely finish the implementation in order to pass
(grade: 4.0) the course.
{{< /hint >}}


## Process

1. write a [concept]({{< ref "/assignments/deliverables/project-concept" >}}) and hand in an initial version before the given
   deadline (sometime within the first half of the semester, for more specifics refer to the onboarding slides)
2. receive initial feedback and sanity-check in written form via email
3. check in last commit of the [implementation]({{< ref "/assignments/deliverables/project-implementation" >}}) before the end
   of day at the date of *code freeze*
4. present your implementation in a review session at the end of the semester

Eventually, you'll end up with two repositories:

(1) a fork of the application that you [chose]({{< ref "/faq/assignments#app-for-project" >}})
    to deploy in this assignment (NOTE: *fork* as in __`git clone`__, don't copy content of another repository;
    __rebase regularly__ to upstream)

(2) the source code that defines your infrastructure, its documentation, and the
    [concept]({{< ref "/assignments/deliverables/project-concept" >}})


## Specifications

### Components

* Application (+ backing service)
* Version control system
* Automation system
* Target environments (test, production)
* Monitoring

You are free to choose any available technology to implement the project, as long as all requirements are being met.  

{{< hint info >}}
For more details and specifications of the deliverable(s) and their requirements, please refer the
corresponding document(s) [here]({{< ref "/assignments/deliverables" >}})
{{< /hint >}}


### Application conditions

* the host system or runtime environment of the application or any other service must be 
  [POSIX-compliant](https://en.wikipedia.org/wiki/POSIX#POSIX-oriented_operating_systems)
  (e.g. Linux, BSD, macOS)
* the source code of the application is part of the deliverable, and thus must not be proprietary or
  confidential in any way 
* the application must depend on at least one backing service (e.g. database to persist data, an email server
  to send out notifications, etc.)


## Review

* it's going to be a remote 1:1 session
* time slots are assigned in advance through a self-service online poll  
* demonstrate the entire deployment process by introducing an actual change to the application source code
* verify the existence of every service


## Teams

* it's allowed to work in a team (≤ 2 people), but not required
* register the team before the deadline mentioned in the onboarding slides has exceeded 
* the workload of a team is expected to be the pre-defined workload of a single person
  multiplied by the amount of team members


## Additional information

The following links may provide some guidance on how to approach the assigment. They can be helpful to structure your
work, or even lead to one or two useful hints for the [implementation]({{< ref "/assignments/deliverables/project-implementation" >}}).
Whether you are new to this whole field of DevOps or already have some experience, chances are high you'll get something
out of it.

* [App suggestions]({{< ref "/faq/assignments#app-for-project" >}})
* [A guided path]({{< ref "/guide/path" >}})
* [Making decisions]({{< ref "/guide/decisions" >}})
* [Technical tips]({{< ref "/guide/tips" >}})
* [Example stacks]({{< ref "/guide/examples" >}})
