---
title: 'Project Implementation'
weight: 3
---


Deliverable: Project Implementation
===================================


__SUBMISSION DEADLINE:__ before the end of the day prior to the day scheduled for the review session.


## Requirements

### Formal

* language: German or English
* every line of code and documentation is stored and tracked in an accessible Git repository
* hand in (a) link(s) to the repository/repositories
* each code base contains a `README.md` which includes an onboarding section
* starting from scratch each step taken towards the final state of the overall infrastructure setup must be
  comprehensible (to ensure fair evaluation/grading)
* every relevant (terminal) command has to be made transparent & verifiable, e.g. in documentation a/o dedicated files
  (e.g. pipeline code, shell script, `Makefile`)

{{< hint warning >}}
Please note, that the minimal permissions required to view source code of *internal* or *private* projects on
GitLab is the [__reporter__ role](https://docs.gitlab.com/ee/user/permissions.html#project-members-permissions).
{{< /hint >}}


### Technical

* follow the *Infrastructure-as-Code* paradigm, meaning: write code rather than words that explain your steps
* cloud-, self- or locally hosted
* all relevant services (VCS, CI/CD, App, Monitoring) need to be accessible via FQDN
* the application and each service provisioned by yourself must be served via _HTTP**S**_ (HTTP over TLS)
* CI is only triggered through a change in the VCS
* deploying to the *production* environment must involve a manual review/approval/release process
* application must run 100% redundant (scaled horizontally with a replication factor of at least *2*)
* when deploying a new version, the app must continue to be reachable (zero-downtime deployment); this
  depends on the deployment strategy that has been chosen
* for the sake of simplicity, the state of the application's persistence layer can be considered
  ephemeral/transient, which means data may get lost when services representing said layer get restarted,
  nevertheless they still have to be part of the overall setup, and thus of the infrastructure and
  automation code


### Content

* the application is put through at least 3 stages (build, test, deploy) by at least 1 pipeline (CI/CD)
* at least one service (e.g. VCS, Monitoring) other than the application has to be provisioned by yourself (no
  third-party as-a-service solution)
* automate allocation and configuration of all required infrastructure resources and services


### Additional notes

* making the deliverable comprehensive means, one must be able to reproduce the entire setup solely by having the
  repositories content available, so that at the end the entire setup is functional as intended
* it's not necessary to implement any exporter logic into the application code in order to get some monitoring 
  working; there are other aspects of your infrastructure to collect metrics on
