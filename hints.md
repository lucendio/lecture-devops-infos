Hints & Guidance
================

This documents gives some advice on how to approach the exercise and how to deal with certain problems. The content
might only be applicable to a certain extent, depending on your approach. It is based on personal experience and
therefore may be subjective.

*NOTE: the content of this document is not finite but also not updated regularly*



### Approach and workflow

* there is no universal best practise only the most reasonable and suitable solution for your specific problem that you
  are trying to solve 
* infrastructure code is subject to development just as it is with application code
  - you may establish a development life cycle starting where you write code on your local computer, but instead of
    building and running your application locally, you allocate and bootstrap either on your computer or just
    triggered locally  
* tear down and recreate your infrastructure on a regular basis
  - shows reproducibility
  - might safe some credits
* automate every step of the way right from the beginning
  - not necessarily by starting to write jobs for your automation tool, but rather by scripting those steps  
  - this can speed up your work and may serve as the foundation for the CI/CD integration happening later down the road



### Technical Tips

* a change in VCS can be, for example, a modification of the code or a new tag
* FQDNs are typically provided through DNS
    * simulated locally: (a) by editing the `/etc/hosts` file or (b) by running a local DNS service (e.g. `dnsmasq`)
    * public: third-party DNS provider that runs *name servers* (requires to own a *domain*)
    * service: [nip.io](https://nip.io/) (and alike) provide *wildcard DNS for any IP address*
* to access the same application deployed in different environments, they typically get assign distinct FQDNs, instead
  of, for example, different ports (e.g. `dev.myapp.tld`, `prod.myapp.tld`)
* even though the application requires a persistence layer, it can be considered *ephemeral*
* to prevent side effects and increase reproducibility (necessary for fair evaluation/grading), it is recommended to not
  install any of the services to the local host system (e.g. your computer)
* in order to distribute incoming traffic across multiple instances of an application a load balancer can be
  placed *in front* of them
* to implement TLS for local (non-public) services, self-signed certificates are a valid solution (keep in mind, that
  other services might need to have the certificate installed, when communication between each other)
* [Let's Encrypt](https://letsencrypt.org/docs/) issues certificates free of charge for public domains (e.g. used by
  [*cert-manager*](https://github.com/jetstack/cert-manager) on K8s)
* on public clouds, being able to allocate and bootstrap your infrastructure from scratch in an automated fashion on
  demand, can help saving some credits (e.g. tear down everything after a day of development work and next day bringing
  everything back up)
* the [application repository](https://github.com/lucendio/lecture-devops-app) defines all its dependencies. For more
  details, please refer to the [README](https://github.com/lucendio/lecture-devops-app/blob/master/app/README.md)
* infrastructure provisioning can be triggered manually and doesn't have to be integrated into the automation service



### Example stacks

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
