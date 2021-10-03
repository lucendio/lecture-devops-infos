Example technology stacks
=========================

The subsequent collection of examples describes - in a very vague manner - possible technology stacks and even relations
between some of them. It's just meant to be an inspiration and starting point of research when designing and developing
you *project*.

*Please note, that none of the examples are mutual exclusive. Certain items across these examples may yield other
reasonable or even more suitable combination(s).*

__*Setup categories:*__
a) *local* (your own computer)
b) *remote* (any other computer; aka 'cloud')
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


##### 1d – Orchestration *[remote]* (university)

* Gitlab                                    *[VCS,CI/CD,Registry]*
* Kubernetes (+Docker) -> *application*     *[runtime environment]*
* Helm                                      *[automation]*
* EFK                                       *[logging]*


##### 2a – Cloud services + Monitoring VM *[remote]*

* Bitbucket                                *[VCS, CI/CD]*
* Heroku (+Terraform) -> *application*     *[runtime environment,automation,proxy,load balancer]*
* EFK                                      *[logging]*
  + DigitalOcean            
  + Terraform
  * Ansible


##### 2b – Multi-Cloud: managed *[remote]*

* Github                                *[VCS]*
* Github actions                        *[CI/CD]*
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
