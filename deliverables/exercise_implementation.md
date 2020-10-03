Deliverable: Exercise implementation
====================================


__SUBMISSION DEADLINE:__ before the end of that day on which the result has been presented during the 1:1 review
    session. 


### Requirements

#### Formal

* language: German or English
* every line of code and documentation is stored and tracked in an accessible Git repository
* hand in (a) link(s) to the repository/repositories
* every code base contains a `README.md` which includes an onboarding section
* every relevant (terminal) command has to be made transparent & verifiable, e.g. in script file(s) a/o documentation
* implementation and execution of each step needs to be comprehensible (to ensure fair evaluation/grading)


#### Technical

* follow the *Infrastructure-as-Code* paradigm
* cloud-, self- or locally hosted
* the host system for the application and any other component must be at least
  [mostly POSIX-compliant](https://en.wikipedia.org/wiki/POSIX#POSIX-oriented_operating_systems)
  (e.g. Linux, BSD, macOS)
* all relevant components (VCS, CI/CD, App, Monitoring) need to be accessible via FQDN
* the application and all implemented components must be served via _HTTP**S**_ (HTTP over TLS)
* CI is only triggered by a change in the VCS
* CD to the *production* environment must involve a manual approval/release process
* application must run 100% redundant (scaled horizontally with a replication factor of *2*)
* when deploying a new version, the app must continue to be reachable; this depends on the deployment strategy that has
  been chosen


#### Content

* the application is put through at least 3 stages (build, test, deploy) by at least 1 pipeline (CI/CD)
* at least one component (e.g. VCS, Monitoring) other than the application has to be bootstrapped by yourself (no
  third-party as-a-service solution)
* automate allocation and bootstrapping of all required infrastructure resources and components
