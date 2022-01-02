Project Work 
============


__Description:__ Deploy and run the [given application](https://github.com/lucendio/lecture-devops-app) in a
cloud or cloud-simulated environment. Allocate the underlying infrastructure resources. Provision all the services 
required for the application to function properly and to deploy it as example workload in multiple environments.
Automate these processes in a reasonable manner.

__Deliverables:__ [concept](./deliverables/project_concept.md) & [implementation](./deliverables/project_implementation.md)

__Requirements:__ successful participation in the [Research & Discussion](./research-and-discussion.md) assignment


#### Process

1. hand in a [concept](./deliverables/project_concept.md) before the given deadline has ended (sometime within
   the first half of the semester)
2. receive initial feedback and sanity-check in written form via email
3. check in last commit before the end of day at the date of _code freeze_
4. present your implementation in a review session at the end of the semester

Eventually, you'll end up with two repositories:

(1) a fork of the [application](https://github.com/lucendio/lecture-devops-app) that you are going to deploy
    in this assignment (NOTE: __clone or fork__, don't copy & paste code, and __rebase regularly__ to upstream)

(2) the source code that defines your infrastructure, its documentation, and the
    [concept](./deliverables/project_concept.md)

*Please note, that it's not required to successfully & completely finish the implementation in order to pass (grade: 4.0)
the course.*


#### Rules

* teams (≤ 2 people) are allowed, but not required


#### Specifications

__Components:__

* Application (+ backing service)
* Version control system
* Automation system
* Target environments (test, production)
* Monitoring

Except for the given [application](https://github.com/lucendio/lecture-devops-app) and its dependencies, you are free
to choose any available technology to implement the project, as long as all requirements are being met.  

*NOTE: for more details and specifications of the deliverable(s), please refer the corresponding document(s) 
[here](./deliverables)*


#### The application

* fork the [source code](https://github.com/lucendio/lecture-devops-app)
* contribute at least one Pull-Request that adds a new & *meaningful* test (server or client)
* adjust the forked source code in any way you feel necessary in order to integrate with your implementation
* if you add something that might be worth sharing and could be useful for others, you are more than welcome to create
  a Pull-Request against [upstream](https://github.com/lucendio/lecture-devops-app)
* [`README`](https://github.com/lucendio/lecture-devops-app/blob/master/README.md) files are usually a good starting
  point to familiarize yourself with the code base


#### Review session

* it's going to be a 1:1 
* demonstrate the entire deployment process  by introducing an actual change to the code base of the
  [given application](https://github.com/lucendio/lecture-devops-app)
* verify the existence of every service


#### Additional information

The following links may provide some guidance on how to approach the assigment. They can be helpful to structure your
work, or even lead to one or two useful hints for the [implementation](./deliverables/project_implementation.md).
Whether you are new to this whole field of DevOps or already have some experience, chances are high you'll get something
out of it.

* [A guided path](./../guide/path.md)
* [Making decisions](./../guide/decisions.md)
* [Technical tips](./../guide/tips.md)
* [Example stacks](./../guide/examples.md)
