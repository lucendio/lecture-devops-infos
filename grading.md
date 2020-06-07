Grading
=======


This document explains which parts of the work delivered in the context of this lecture result into certain gradings.

In order to pass this module, one is required to obtain at least _**50%**_ of the overall achievable amount of points.


#### Research

_Weight: **0%**_

*Purpose:* being permitted to hand in the [exercise](./exercise.md). 

*Criteria:*

* actively participated and shared findings during discussion 
* research notes are published


#### Concept

_Weight: **0%**_

*Purpose:* to receive a score for the implementation
 
*Notes:*  meant as a tool to ...

* help designing the architecture
* lay down a roadmap
* discuss possible solutions
* document that process

*Criteria:*

* it exists
* reflects actual implementation


#### Implementation

_Weight: **100%**_

*Purpose:* to pass the module

*Notes:*

* the total score may vary depending on the set of requirement defined for a specific semester (please refer to the
  onboarding slides provided at the beginning of the semester)
* for every criterion a maximum of *one (1)* point can be received, unless defined otherwise (e.g. `[n]`)  
* criteria may depend on one another
* unmet criterion will *not* cause point deduction

*Criteria:* 

* resource allocation is automated
* resource allocation is reproducible
* component bootstrapping is automated
* component bootstrapping is reproducible
* every component exists and is operational
* every component can be reached via HTTPS
* FQDNs are configured for all components, resolve to each of them and serve them properly 
* *application* runs with a replication factor of *2*
* in each environment the *applications* uses a different instances of its backing service (database)
* CI/CD pipeline functions properly
* CI/CD pipeline is triggered through VCS commit, or manually, in case of *production* environment  
* CI/CD pipeline consists of at least 3 stages
* overall setup implements all environments
* concept matches implementation
* implementation meets all formal requirements
* at least one component - aside from the *application* - was bootstrapped by yourself
* submitted a Pull Request, that adds another *meaningful* test to the application
* [3] quality and impression, e.g.:
  - reasonable documentation
  - readable commit messages
  - engagement
