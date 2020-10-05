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

__*Setup categories:*__
a) *local* (your own computer)
b) *remote* (any another computer; aka 'cloud')
c) *hybrid* (local + remote)


##### 1a – virtual machines *[local]*

* Virtualbox (+Vagrant) -> *application*    *[runtime environment]*
* Packer (+Ansible)                         *[automation]*
* Gitea                                     *[VCS]*
* Jenkins                                   *[CI/CD]*
* HAProxy                                   *[proxy/vhost/load-balancer]*
* ELK                                       *[logging]*


##### 1b – Containers *[local]*

* (Virtualbox + Vagrant)    
* Ansible                   *[automation]*
* Docker -> *application*   *[runtime environment]*
* Gitea                     *[VCS]*
* Concourse                 *[CI/CD]*
* Nginx                     *[proxy/vhost/load-balancer]*
* Monit                     *[metrics]*


##### 1c – Orchestration *[local]*

* (Virtualbox + Vagrant)
* Ansible (+Kubespray)                      *[automation]*
* Kubernetes (+Docker) -> *application*     *[runtime environment]*
* Helm                                      *[automation]*
* Gitlab                                    *[VCS,CI/CD]*
* Prometheus + Grafana                      *[metrics]*


##### 2a.1 – Cloud services + Monitoring VM *[remote]*

* Bitbucket                             *[VCS]*
* Heroku (+Terraform) -> *application*  *[CI/CD,runtime environment,automation,proxy,load balancer]*


##### 2a.2 – Cloud services + Monitoring VM *[hybrid]*

* Heroku (+Terraform) -> *application*     *[VCS,CI/CD,runtime environment,automation,proxy,load balancer]*
* EFK                                       *[logging]*
  + DigitalOcean            
  + Terraform
  * Ansible


##### 2b – Multi-Cloud: managed *[remote]*

* Github                                *[VCS]*
* TravisCI (or Github actions)          *[CI/CD]*
* AWS (+Terraform) -> *application*     *[runtime environment,automation,proxy,load balancer]*
* Splunk                                *[logging]*


##### 2c – Local + Cloud *[hybrid]*

* Github                            *[VCS]*
* Jenkins                           *[CI/CD]*
  + Virtualbox + Vagrant or Docker
  + Ansible
* DigitalOcean                      *[runtime environment]*
    * *application*
    * HAProxy                       *[proxy/load-balancer]*
    * ELK                           *[logging]*
+ Terraform                         *[automation]*
* Ansible                           *[automation]*


##### 3a - Cloud Orchestration: managed *[remote]*

* Bitbucket                 *[VCS]*
* Terraform                 *[automation]*
* GKE -> *application*      *[runtime environment,proxy,load balancer]*
* Helm                      *[automation]*
* Jenkins + K8s Plugin      *[CI/CD]*
* Stackdriver               *[logging]*


##### 3b - Cloud Orchestration: DIY *[remote]*

* Github                            *[VCS]*
* Terraform                         *[automation]*
* DigitalOcean
    * Ansible (+Kubespray)          *[automation]*
* Kubernetes -> *application*       *[runtime environment,proxy,load balancer]*
* Helm                              *[automation]*
* Jenkins + K8s Plugin              *[CI/CD]*
* EFK                               *[logging]*


##### 4 – All in One *[local or remote]*

* OpenShift:
    * HAProxy                   *[proxy,load balancer]*
    * Container Registry
    * Kubernetes                *[runtime environment]*
      + Gitea                   *[VCS]*
    * Jenkins                   *[CI/CD]*
    * EFK                       *[logging]*
    * Prometheus + Grafana      *[metrics]*


##### 5 – Jenkins X 

* Github                                *[VCS]*
* AWS -> Jenkins X -> *application*     *[CI/CD,runtime environment,proxy,load balancer]*
