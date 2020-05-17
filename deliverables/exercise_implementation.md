Deliverable: Exercise implementation
====================================


__SUBMISSION DEADLINE:__ before the end of that day when the work has been presented during the 1:1 review session 


### Requirements

#### Formal

* language: German or English
* every line of code and documentation is stored and tracked in an accessible Git repository
* hand in (a) link(a) to the repository/repositories
* every code base contains a `README.md` which includes an onboarding section
* every relevant (terminal) command has to be made transparent & verifiable, e.g. in script file(s) a/o documentation
* implementation and execution of of each step needs to be comprehensible (to ensure fair evaluation/grading)


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


#### Content

* the application is put through at least 3 stages (build, test, deploy) by at least 1 pipeline (CI/CD)
* at least one component (e.g. VCS, Monitoring) other than the application has to be bootstrapped by yourself (no
  third-party as-a-service solution)
* automate allocation and bootstrapping of all required infrastructure resources and components


### Tips

* a change in VCS can be, for example, a modification of the code or a new tag
* FQDNs are typically provided through DNS
    * simulated locally: (a) by editing the `/etc/hosts` file or (b) by running a local DNS service (e.g. `dnsmasq`)
    * public: third-party DNS provider that runs *name servers* (requires to own a *domain*)
    * service: [nip.io](https://nip.io/) (and alike) provide *wildcard DNS for any IP address*
* to access the same application deployed in different environments, they typically get assign distinct FQDNs, instead
  of, for example, different ports (e.g. `dev.myapp.tld`, `prod.myapp.tld`)
* even though the application requires a persistence layer, it can considered *ephemeral*
* to prevent side effects and increase reproducibility (necessary for fair evaluation/grading), it is recommended to not
  install any component to the local host system (e.g. your computer)
* in order to distribute incoming traffic across multiple instances of an application a load balancer can be
  placed *in front* of them
* to implement TLS for local (non-public) services, self-signed certificates are a valid solution (keep in mind, that
  other services might need to have the certificate installed, when communication between each other)
* [Let's Encrypt](https://letsencrypt.org/docs/) issues certificates free of charge for public domains (e.g. used by
  [*cert-manager*](https://github.com/jetstack/cert-manager) on K8s)
* on public clouds, being able to allocate and bootstrap your infrastructure from scratch in a automated fashion on
  demand, can help saving some credits (e.g. tear down everything after a day of development work and next day bringing
  everything back up)


### Examples

The subsequent collection of examples describes, in a very vague manner, possible technology stacks and even relations
between some of them. It is just meant as an inspiration and starting point for research when designing and developing
you individual exercise solution.

*Please note, that non of the examples are mutual exclusive. Certain items across these examples may yield reasonable
and even more suitable combination(s).*


##### 1a – local virtual machines

* Virtualbox + Vagrant
* Packer (+ Ansible)
* Gitea
* *application*
* Jenkins
* HAProxy
* ELK


##### 1b – local Containers

* (Virtualbox + Vagrant + Ansible)
* Docker
* *application*
* Gitea
* Concourse
* Nginx
* Monit


##### 1c – local Orchestration

* *application*
* Virtualbox + Vagrant + Ansible/Kubespray
* Kubernetes
* Helm
* Gitlab
* Prometheus + Grafana


##### 2a.1 – Cloud + Application VM

* Bitbucket
* Heroku
* Terraform + DigitalOcean -> *application*


##### 2a.2 – Cloud + local Monitoring VM

* *application*
* Heroku
* Virtualbox + Vagrant + Ansible -> Prometheus + Grafana


##### 2b – Multi-Cloud [managed]

* Github
* TravisCI
* Terraform + AWS -> *application*
* Splunk


##### 2c – Local + Cloud [hybrid]

* Github
* (Virtualbox + Vagrant + Ansible) -> Jenkins
* Terraform + DigitalOcean -> VMs
* Ansible
* *application*
* HAProxy
* ELK


##### 3a - Cloud Orchestration [managed]

* Bitbucket
* Terraform + GKE
* Helm
* Jenkins + K8s Plugin
* *application*
* Stackdriver


##### 3b - Cloud Orchestration [DIY]

* Github
* Terraform + DigitalOcean -> VMs
* Ansible/Kubespray + Kubernetes
* Helm
* Jenkins + K8s Plugin
* *application*
* EFK


##### 4 – All in One [local or cloud]

* *application*
* OpenShift: HAProxy, Container Registry, Kubernetes, Jenkins, EFK, Prometheus + Grafana


##### 5 – Jenkins X

* Github
* *application*
* AWS -> Jenkins X
