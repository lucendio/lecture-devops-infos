Grading
=======


This document explains which parts of the assignments that are being taken during this course will be part of
your overall score.

In order to pass the course, one is required to obtain at least _**50%**_ of the overall achievable score.


#### Research

_Weight: **0%**_

*Purpose:* be permitted to hand in the [project work](./assignments/project-work.md) assignment 

*Criteria:*

* actively participated and shared findings during discussion 
* research notes are published


#### Concept

_Weight: **0%**_

*Purpose:* be entitled to receive a score for the [implementation](./assignments/deliverables/project_implementation.md)
 
*Notes:* meant as a tool to ...

* help designing the architecture
* lay down a roadmap
* discuss possible solutions
* document this development process

*Criteria:*

* it exists
* reflects actual implementation


#### Implementation

_Weight: **100%**_

*Purpose:* to pass the course

*Notes:*

* the total score may vary depending on the set of requirement defined for a specific semester (please
  refer to the *Onboarding* slides provided at the beginning of the semester)
* a maximum of *one (1)* point can be received for every criterion, unless defined otherwise (e.g. `[n]`)  
* criteria may depend on one another
* unmet criterion does *not* cause point deduction
* only written content is taken into account in the grading process, not what is demonstrated during review
  session
* if the overall score received for this assignment reaches below 50%, one can take a
  [retry exam](./assignments/retry-exam.md) to still pass the course

*Criteria:* 

* convincingly participate in review session
* allocation of all infrastructure pieces is reproducible
* allocation of all infrastructure pieces is automated
* installation & configuration of all services are reproducible
* installation & configuration of all services are automated
* idempotent provisioning
* each service exists and is operational
* each service can be reached via HTTPS
* FQDNs are configured for all services, resolve to each of them and serve them properly 
* *application* runs with a replication factor of at least *2*
* zero-downtime deployment strategy
* overall setup implements all environments
* the *application* in each environment uses a different instance of its backing service (e.g. database)
* CI/CD pipeline functions properly
* CI/CD pipeline consists of at least 3 stages
* CI/CD pipeline is triggered via VCS commit, and - in case of *production* environment - in an
  explicit/controlled manner
* concept/documentation matches implementation
* all formal requirements ([concept](./assignments/deliverables/project_concept.md#formal) & 
  [implementation](./assignments/deliverables/project_implementation.md#formal)) are met and the
  [process](./assignments/project.md#process) has been followed
* at least one service - aside from the *application* - was provisioned by yourself
* [3] overall quality and impression, e.g.:
  - engagement
  - applied new knowledge
  - reasonable documentation
  - meaningful commit messages
  - ...


#### Retry exam

_Weight: **100%**_

*Purpose:* to pass the course in case of a failed project assignment

*Notes:*

* it's only possible to reach a maximum score of __60%__ (grade: `3,3`) even if all questions have been
  answered correctly

*Criteria:* 

* reach at least a score of 50%
