Glossary
========


*Format: `TERM(*) [ABBREVIATION]: DEFINITION`*

*NOTE: either abbreviation or definition are optional; terms, marked with a __`*`__, tent to be more opinionated rather
       than generally accepted*



### General

__environment*__ [env]: a *single installation* (or instance) of an application/service, all its *backing services*
    and, at least to some extend, the infrastructure that is hosting all of it. Some resources/services might me shared
    across environments. The purpose(s), requirement(s) and thus the accessibility of an environment may vary.
    (Examples: production, staging, test, development, local) 

__infrastructure*__ [infra]: (A) the physical/virtual resources (machines, network) one or more environments are running
    on and, to some extend, (B) components/services that are necessary to facility environment(s), such as load balancer,
    CI/CD, or VCS 

__stack/toolchain*__: a set of technologies (software a/o hardware), that describes the composition/architecture of, for
    example, the infrastructure (i.e. infrastructure stack) and maybe how it's set up. Each item represents a dependency
    to the overall setup

__local environment*__: refers of a toolchain that facilitates developing software a/o infrastructure code on a local
    computer. The locally instantiated environment is called the same. For example, it consists of a virtual machine
    simulating (a portion of) the actual remote setup.

__allocate*__: bring up/instantiate/configure infrastructure resources/components (e.g. machines, network)

__bootstrap*__: configure infrastructure resources or install software on top to give purpose

__provision*__: refers to allocation & bootstrapping altogether. While allocation is more related to (emulated) hardware
    resource (e.g. virtual machines), bootstrapping is covering the software installation and configuration on top of
    that  

[__Infrastructure-as-Code__](https://en.wikipedia.org/wiki/Infrastructure_as_code) [IaC]: define the desired state of
    your infrastructure in source code. Common best practices in *Software Engineering* can, thereby, be applied.

[__Greenfield project__](https://en.wikipedia.org/wiki/Greenfield_project): A project started from scratch that doesn't
    have any dependencies yet, and hence no limitation regarding these decisions.

__Version Control System__ [VCS]: software to manage source code versioning, that enables collaborative editing and
    helps to integrate changes into the code base 

__Source Control Management__ [SCM]: see *VCS*; not to be confused with *Software Configuration Management*

[__system/application development life cycle__](https://en.wikipedia.org/wiki/Systems_development_life_cycle) [SDLC]:
    represents the process, and therefore describes every aspect, of software creation, delivery, updating, and even
    transition or retirement

[__Software Configuration Management__](https://en.wikipedia.org/wiki/Software_configuration_management) [SCM]: refers
    to the process of organizing and controlling software changes as part of the *SDLC*, e.g. prioritizing & ordering, 
    bug tracking, versioning, releasing & rollout, environment settings

[__microservice__](): A design pattern for software architecture and the counterpart to a 
    [monolithic approach](https://en.wikipedia.org/wiki/Monolithic_application). Following this, turns components of an
    application into services, in order to archive a loose coupling (e.g. communicate through web APIs) 

[__12-Factor-App__](https://12factor.net) [12-Factor]: a set of best practices to build software-as-a-service solutions
    for cloud-native environments, but also recognized as *good application design principles* in general

[__backing service__](https://12factor.net/backing-services): a service, that is consumed (a.k.a. dependent on) by
    another service/process/application

__scale vertically__: modify the available resources (processor, memory, storage, uplink) of an instance (a.k.a. scale up)

__scale horizontally__: modify the amount of instances and distribute load (a.k.a. scale up)

__ephemeral__: a process/service/instance has no persistent state (e.g. sessions), thus no need to be petted or make it
    last; this characteristic, i.a., makes it effortless to scale horizontally

__persistent__: the state of a process/service/instance remains (outlives) its shutdown/restart (a.k.a. *stateful*);
    opposite of *ephemeral*

__idempotent__: repeated execution or application results in the same state. Typically, referred to in the context of 
    *Configuration Management*

__workload__: any kind of program, non-native of it surrounding context, but executed within it; referred to, e.g, the
    context of *Kubernetes*

__Automation tool/system__ [CI/CD]: helps to facility & automate certain steps in a *SDLC*, e.g. build, test, or deploy.
    It runs as a service that is usually accessing but also accessed by other component within an infrastructure setup.   

[__Continuous Integration__](https://en.wikipedia.org/wiki/Continuous_integration) [CI]: as part of an automation tool,
    it runs [integration tasks](https://en.wikipedia.org/wiki/Systems_integrator) unattended, e.g. merge, build, test  

__Continuous Delivery/Deployment__ [CD]: as part of an automation tool, it runs tasks either to ensure that the software
    is in a deployable state (Continuous Delivery), or to actually release and deploy the software (Continuous Deployment) 

__exporter__: makes data (metrics, logs) available or even sends it proactively to one ore more collectors (push pattern)

__collector__: place, where all monitoring data comes together. Consists of storage and maybe logic (in case of polling)

[__Fully qualified domain name__](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) [FQDN]: consists of at least
    the *hostname* and one higher level (or top-level) domain (short: TLD) separated by a '.' (dot);
    e.g. `my-domain-name.tld`

__virtual machine__ [VM]: emulation of a physical machine hosting a full-featured (own kernel) operating system 

__Kubernetes__ [K8s]: open source container orchestration platform; managed versions are offered by public cloud provider,
    such as [DigitalOcean](https://www.digitalocean.com/products/kubernetes/), Google: [GKE](https://cloud.google.com/kubernetes-engine/),
    Amazon (AWS): [EKS](https://aws.amazon.com/eks/), Azure (Microsoft): [AKS](https://azure.microsoft.com/en-us/services/kubernetes-service/)

__Elasticsearch+Fluentd+Kibana__ [EFK]: technology stack that provides Log Aggregation

__ingress__: incoming traffic (a.k.a. inbound)

__egress:__ outgoing traffic (a.k.a. outbound)
