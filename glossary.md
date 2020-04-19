Glossary
========


*Format: `TERM [ABBREVIATION]: DEFINITION`*
*Note: either abbreviation or definition are optional; terms, marked with `*` tent to be rather opinionated than
       generally accepted*



### General

environment* [env]: a *single installation* (or instance) of an application/service, all its *backing services*, and,
    at least to some extend, the infrastructure that is hosting all of it. Some resources/services might me shared
    across environments. The purpose(s), requirement(s) and thus the accessibility of an environment may vary.
    (Examples: production, staging, test, development, local) 

infrastructure* [infra]: (A) the physical/virtual resources (machines, network) one or more environments are running on  
    and, to some extend, (B) components/services that are necessary to facility environment(s), such as load balancer,
    CI/CD, or VCS 

stack/toolchain*: a set of technologies (software a/o hardware), that describes the composition/architecture of, for
    example, the infrastructure (i.e. infrastructure stack) and maybe how it's set up. Each item represents a dependency
    to the overall setup

Local environment*: refers of a toolchain that facilitates developing software a/o infrastructure code on a local
    computer. The locally instantiated environment is called the same. For example, it consists of a virtual machine
    simulating (a portion of) the actual remote setup.

provisioning*: bring up/instantiate infrastructure resources/components (e.g. machines, network) 

bootstrapping*: configure infrastructure resources or install software on top to give purpose

[Infrastructure-as-Code](https://en.wikipedia.org/wiki/Infrastructure_as_code) [IaC]: define the desired state of your
    infrastructure in source code. Common best practices in *Software Engineering* can, thereby, be applied.

[Greenfield project](https://en.wikipedia.org/wiki/Greenfield_project): A project started from scratch that doesn't have
    any dependencies yet, and hence no limitation regarding these decisions.

Version Control System [VCS]: software to manage source code versioning, that enables collaborative editing and helps to
    integrate changes into the code base 

Source Control Management [SCM]: see *VCS*; not to be confused with *Software Configuration Management*

[system/application development life cycle](https://en.wikipedia.org/wiki/Systems_development_life_cycle) [SDLC]:
    represents the process, and therefore describes every aspect, of software creation, delivery, updating, and even
    transition or retirement

[Software Configuration Management](https://en.wikipedia.org/wiki/Software_configuration_management) [SCM]: refers to
    the process of organizing and controlling software changes as part of the *SDLC*, e.g. prioritizing & ordering, 
    bug tracking, versioning, releasing & rollout, environment settings

[12-Factor-App](https://12factor.net) [12-Factor]:

[backing service](https://12factor.net/backing-services): a service, that is consumed (a.k.a. dependent on) by another 
    service/process/application

scale vertically: increase the available resources (processor, memory, storage, uplink) of an instance (a.k.a. scale up)

scale horizontally: increase the amount of instances and distribute load (a.k.a. scale up)

ephemeral: a process/service/instance has no persistent state (e.g. sessions), thus no need to be petted or make it last;
    this characteristic, i.a., makes it effortless to scale horizontally

persistent: the state of a process/service/instance remains (outlives) its shutdown/restart (a.k.a. *stateful*); opposite
    of *ephemeral*

idempotent: repeated execution or application results in the same state. Typically, referred to in the context of 
    *Configuration Management*

workload: any kind of program, non-native of it surrounding context, but executed within it; referred to, e.g, the
    context of *Kubernetes*

Automation tool/system [CI/CD]: helps to facility & automate certain steps in a *SDLC*, e.g. build, test, or deploy. It
    runs as a service that is usually accessing but also accessed by other component within an infrastructure setup.   

[Continuous Integration](https://en.wikipedia.org/wiki/Continuous_integration) [CI]: as part of an automation tool, it
    runs [integration tasks](https://en.wikipedia.org/wiki/Systems_integrator) unattended, e.g. merge, build, test  

Continuous Delivery/Deployment [CD]: as part of an automation tool, it runs tasks either to ensure that the software is
    in a deployable state (Continuous Delivery), or to actually release and deploy the software (Continuous Deployment) 

exporter: makes data (metrics, logs) available or even sends it proactively to one ore more collectors (push pattern)

collector: place, where all monitoring data comes together. Consists of storage and maybe logic (in case of polling)

[Fully qualified domain name](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) [FQDN]: consists of at least
    the *hostname* and one higher level (or top-level) domain (short: TLD) separated by a '.' (dot);
    e.g. `my-domain-name.tld`

virtual machine [VM]: emulation of a physical machine hosting a full-featured (own kernel) operating system 

Kubernetes [K8s]: open source container orchestration platform; managed versions are offered by public cloud provider,
    such as [DigitalOcean](https://www.digitalocean.com/products/kubernetes/), Google: [GKE](https://cloud.google.com/kubernetes-engine/),
    Amazon (AWS): [EKS](https://aws.amazon.com/eks/), Azure (Microsoft): [AKS](https://azure.microsoft.com/en-us/services/kubernetes-service/)

Elasticsearch+Fluentd+Kibana [EFK]: technology stack that provides Log Aggregation
