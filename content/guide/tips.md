Tips
====

This is a rather unstructured list of tips, DOs and DON'Ts, mainly focused on technical aspects and details around the
*project* assignment. Chances are that some might already mentioned or covered somewhere else in this repository.


#### Workflow

* if a technology is new to you, start with the "Getting Started / Quickstart" section or at the beginning of the
  documentation; not some Stackoverflow thread or a blog article
* to prevent side effects and undesired states that are usually hard to reproduce, it is recommended to not install
  any service that is part of your project architecture directly into your local host system of your workstation
* on public clouds, it can help you to save some credits, when you are able to *allocate* and *configure* your
  infrastructure from scratch in an automated fashion on demand (e.g. tear down everything after a day of development
  work and next day bring everything back up)
* infrastructure provisioning can be triggered manually and doesn't have to be integrated into the automation service
* when executing Terraform keep in mind its state and where it is located/stored depending on where it's being 
  invoked (e.g. local workstation vs. CI/CD)
* commit message matter - a lot


#### Deployable workload / application

* even though the application requires a persistence layer, it can be considered *ephemeral*; meaning data doesn't have
  to persist across restarts or re-deployments
* usually there is no need to touch/change the actual source code logic of the application, instead additional files
  might need to be added, for example, to define the *pipeline*


#### CI/CD

* a change in VCS can be, for example, a modification in the source code or pushing a new tag
* triggering the pipeline explicitly (or controlled) usually refers to a merge commit to a release branch, to the creation
  of a tag, or simply to manually by pressing a button; usually it's to coordinate & control the deployment (aka.
  continuous delivery) TODO: mainline is somehow guarded, so that nobody can accidentally deploy to production


#### DNS

* FQDNs are typically provided through DNS
    * simulated locally: (a) by editing the `/etc/hosts` file or (b) by running a local DNS service (e.g. `dnsmasq`)
    * public: third-party DNS provider
      (see [FAQ]({{< ref "/faq/cloud-and-infrastructure#dns-provider-free-and-automation">}}))
      that runs *name servers* (requires to control a *domain*)
    * service: [nip.io](https://nip.io/) provides *wildcard DNS for any IP address*
* to access the same application but deployed in different environments, typically they get assign distinct FQDNs,
  instead of, for example, different ports (e.g. `dev.myapp.tld`, `prod.myapp.tld` instead of `myapp.tld:8080`, 
  `myapp.tld:80`)


#### Architecture / Design

* in order to distribute incoming traffic across multiple instances of an application and to enable zero-downtime
  deployment a load balancer can be placed *in front* of them


### TLS 

* to implement TLS for local (non-public) services, self-signed certificates are a valid solution (keep in mind, that
  other services might need to have the certificate installed, when communicating between each other)
* [Let's Encrypt](https://letsencrypt.org/docs/) issues certificates free of charge for public domains (e.g. used by
  [*cert-manager*](https://github.com/jetstack/cert-manager) on K8s)
