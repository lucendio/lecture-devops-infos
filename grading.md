Grading
=======


This document explains which parts of the assignments taken during this course end up in your grading.

In order to pass the course, one is required to obtain at least _**50%**_ of the overall achievable score.


#### Research

_Weight: **0%**_

*Purpose:* be permitted to hand in the [project work](./assignments/project-work.md) assignment 

*Criteria:*

* actively participated and shared findings during discussion 
* research notes are published


#### Concept

_Weight: **0%**_

*Purpose:* be entitled to receive a score for the [implementation](./deliverables/project_implementation.md)
 
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
  refer to the onboarding slides provided at the beginning of the semester)
* for every criterion a maximum of *one (1)* point can be received, unless defined otherwise (e.g. `[n]`)  
* criteria may depend on one another
* unmet criterion does *not* cause point deduction

*Criteria:* 

* resource allocation is automated
* resource allocation is reproducible
* installation & configuration of all services is automated
* installation & configuration of all services is reproducible
* idempotent provisioning
* each service exists and is operational
* every service can be reached via HTTPS
* FQDNs are configured for all services, resolve to each of them and serve them properly 
* *application* runs with a replication factor of *2* (at least)
* zero/minimal-downtime deployment
* the *application* in each environment uses a different backing service (database) instance
* CI/CD pipeline functions properly
* CI/CD pipeline consists of at least 3 stages
* CI/CD pipeline is triggered via VCS commit, and - in case of *production* environment - in an
  explicitly/controlled manner
* overall setup implements all environments
* concept matches implementation
* all formal requirements ([concept](./deliverables/project_concept.md#formal) & 
  [implementation](./deliverables/project_implementation.md#formal)) are met and the
  [process](./assignments/project-work.md#process) has been followed
* at least one service - aside from the *application* - was provisioned by yourself
* a [Pull Request](https://github.com/lucendio/lecture-devops-app/pulls) was submitted, that adds another *meaningful*
  test to the application
* [3] overall quality and impression, e.g.:
  - engagement
  - applied new knowledge
  - reasonable documentation
  - readable commit messages
  - ...
