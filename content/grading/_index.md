---
bookFlatSection: true
bookCollapseSection: false

title: 'Grading'
weight: 3
---


Grading
=======

This document explains which parts of the assignments taken during this course will be part of your overall score.

{{< hint danger >}}
In order to pass the course, one is required to obtain at least _**50%**_ of the overall achievable score.
{{< /hint >}}


## Topic Recaps

_Weight: **2% each**_

*Purpose:*

* earn bonus score

*Criteria:*

* all questions of a test are answered correctly

More information can be found in the [assignments section]({{< ref "/assignments/topic-recaps" >}}).


## Concept

_Weight: **0%**_

*Purpose:* initiate the grading procedure (attempt to pass the course) and be entitled to receive a score for the
[implementation]({{< ref "/assignments/deliverables/project-implementation" >}})
 
*Notes:* meant as a tool to ...

* help designing the architecture
* lay down a roadmap
* discuss possible solutions
* document this development process

*Criteria:*

* it exists
* reflects actual implementation


## Implementation

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
  [retry exam]({{< ref "/assignments/retry-exam" >}}) to still pass the course

*Criteria:*

* convincingly participate in review session
* allocation of all infrastructure pieces is reproducible
* allocation of all infrastructure pieces is implemented through code
* installation & configuration of all services are reproducible
* installation & configuration of all services are implemented through code
* idempotent provisioning
* each service exists, is operational, and - if intended - is externally accessible
* each service can be reached via HTTPS
* FQDNs are configured for all services, resolve to each of them and serve them properly 
* *application* runs high available
* zero-downtime deployment strategy
* overall setup implements all environments
* the *application* in each environment uses a different instance of its backing services (e.g. database)
* CI/CD pipeline functions properly
* CI/CD pipeline consists of at least 3 stages
* CI/CD pipeline is triggered via VCS commit, and - in case of *production* environment - in an
  explicit/controlled manner
* the *application* development life cycle is defined and matches implementation
* *application* changes carried out by CI/CD successfully propagate through all environments
* concept/documentation matches implementation
* all formal requirements ([concept]({{< ref "/assignments/deliverables/project-concept#formal" >}}) & 
  [implementation]({{< ref "/assignments/deliverables/project-implementation#formal" >}})) are met and the
  [process]({{< ref "/assignments/project-work#process" >}}) has been followed
* at least one service - aside from the *application* - was provisioned by oneself
* `[3]` overall quality and impression, e.g.:
  - engagement
  - applied new knowledge
  - reasonable documentation
  - meaningful commit messages
  - ...


## Retry exam

_Weight: **100%**_

*Purpose:* to pass the course in case of a failed project assignment

*Notes:*

* if the project assignment was omitted, it's only possible to reach a maximum score of __60%__ (grade: `3,3`) even
  when all questions have been answered correctly

*Criteria:* 

* reach at least a score of 50%
