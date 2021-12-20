Deliverable: Project Implementation
===================================


__SUBMISSION DEADLINE:__ before the end of the day prior to the day scheduled for the review session.
    Default; may deviate and superseded by information stated in other lecture material.


### Requirements

#### Formal

* language: German or English
* every line of code and documentation is stored and tracked in an accessible Git repository
* hand in (a) link(s) to the repository/repositories
* each code base contains a `README.md` which includes an onboarding section
* every relevant (terminal) command has to be made transparent & verifiable, e.g. in (script) file(s) a/o documentation
* each step taken towards the final setup must be comprehensible (to ensure fair evaluation/grading)


#### Technical

* follow the *Infrastructure-as-Code* paradigm
* cloud-, self- or locally hosted
* the host system for the application and any other service must be at least
  [POSIX-compliant](https://en.wikipedia.org/wiki/POSIX#POSIX-oriented_operating_systems)
  (e.g. Linux, BSD, macOS)
* all relevant services (VCS, CI/CD, App, Monitoring) need to be accessible via FQDN
* the application and each service provisioned by yourself must be served via _HTTP**S**_ (HTTP over TLS)
* CI is only triggered through a change in the VCS
* CD to the *production* environment must involve a manual approval/release process
* application must run 100% redundant (scaled horizontally with a replication factor of at least *2*)
* when deploying a new version, the app must continue to be reachable; this depends on the deployment
  strategy that has been chosen
* for the sake of simplicity, the persistence layer of the application can be considered ephemeral, but
  the backing services representing that layer still have to be taken into account


#### Content

* the application is put through at least 3 stages (build, test, deploy) by at least 1 pipeline (CI/CD)
* at least one service (e.g. VCS, Monitoring) other than the application has to be provisioned by yourself (no
  third-party as-a-service solution)
* automate allocation and configuration of all required infrastructure resources and services
