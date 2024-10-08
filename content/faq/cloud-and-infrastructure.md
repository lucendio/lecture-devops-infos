---
title: 'Cloud & Infrastructure'
---


FAQ - Cloud & Infrastructure
============================


## Where do I get some infrastructure to play with, to do the exercises with, or even for the assignment?

Essentially, you have 3 different options:

a) your workstation has enough resources (CPU, memory) to host one or more virtual machines, which would enable you
   to mimic some *cloud resources* locally - as you know: the cloud is just a bunch of other people's computers

b) have a look at the link section on [*cloud providers*]({{< ref "/links#cloud-providers" >}})

c) the university provides a Kubernetes cluster managed with Rancher (requires student account, and VPN access if you
   are not on-site)


## Which cloud provider should I use?

As mentioned before, there is a variety of common [*cloud providers*]({{< ref "/links#cloud-providers" >}}) you can choose from.
While all of them provide similar concepts which are sometimes named differently, they may have some limitations:

* AWS Academy (formerly called *Educate*); see
  [PDF](/assets/AWS-Academy-Learner-Lab_Services_20230518.pdff) for a complete list of available services and limits, e.g.
  * amount and size of machines (EC2 instances) that you are allowed to spin up
  * no IAM
  * no NAT gateway creation
  * ...  
* Google Cloud Platform for Education: no known limitation, except the amount of credits & a mandatory google
  account (can be created with the university email address)
* Azure for Students: no known limitation, except the amount of credits
* DigitalOcean: no known limitation, except the amount of credits


## How do I get access to AWS and unlock AWS Academy[^1] credits? {id="how-to-get-access-to-aws-and-unlock-aws-academy"}

1. Add your email address to the provided list
2. When receiving the invitation email (subject: *Course Invitation* from *instructure.com*), follow the link to join
   the *Learner Lab - Foundation Services* on AWS Academy
3. You'll need a *Canvas* account - please create one in case you don't have one already. Canvas is a 3rd-party platform
   functioning as the intermediary providing and managing the access to the AWS API.
4. Once you logged the first time on [AWS Academy Instructure](https://awsacademy.instructure.com/login/canvas),
   navigate to *Courses* --> *AWS Academy Learner Lab - Foundation Services* --> *Modules* (in the course context) --> 
   *Learner Lab - Foundational Services* to accept the Terms of Service and then press *Start Lab* (top right)
5. Take a look at the *Readme* (top right) for further information, e.g. for available *Service usage and other
   restrictions*
6. To access the AWS management console (GUI), click on *AWS* right next to the green status indicator (top left)
7. To obtain your credentials, follow the same navigation path from (4.) followed by *AWS Details* (top right) --> AWS
   CLI: *Show* button
   As an alternative, you may want to use the 
   [`aws-academy.sh` script](https://github.com/lucendio/lecture-devops-code/blob/master/hack/aws-academy.sh)
   and run it every 4 hours since a session only lasts so long.

Follow the [official documentation](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html) in order
to configure the CLI so that is uses the credentials.

[^1]: [Official website](https://aws.amazon.com/training/awsacademy/) of the *AWS Academy*


## Which AWS services are supported with the AWS Academy program?

Please refer to [this document](/assets/AWS-Academy-Learner-Lab_Foundational-Services_20211002.pdf) in order to find
out which services are available in an AWS Academy account. Alternatively, you may find these information in mentioned
in *6.8*. Furthermore, please be aware that all services are located in the `us-east-1` or `us-west-2` regions, hence
hosted outside of the european jurisdiction (no GDPR etc.).


## Which public DNS provider offers free domain registration and allows automated record creation? {id="dns-provider-free-and-automation"}

A non-exhaustive list of public DNS providers:

| Name                                             |                         Free Domain                          | API                                                                           | Terraform provider |
|--------------------------------------------------|:------------------------------------------------------------:|-------------------------------------------------------------------------------|:------------------:|
| [Namecheap](https://www.namecheap.com)           |  YES (via [Github Edu](https://education.github.com/pack))   | [URL](https://www.namecheap.com/support/api/intro/)                           |  X                 |
| [Name.com](https://www.name.com)                 | YES (via [Github Edu](https://education.github.com/pack))    | [URL](https://www.name.com/api-docs)                                          |  -                 |
| [INWX](https://www.inwx.de/en)                   |                              NO                              | [URL](https://www.inwx.de/en/offer/api)                                       |  X                 |
| [AWS Route 53](https://aws.amazon.com/route53/)  |                              NO                              | [URL](https://docs.aws.amazon.com/Route53/latest/APIReference/Welcome.html)   |  X                 |
| [GCP Cloud DNS](https://cloud.google.com/dns)    |                              NO                              | [URL](https://cloud.google.com/dns/docs/apis)                                 |  X                 |
| [FreeDNS](https://freedns.afraid.org/)           |                             YES                              | [URL](https://freedns.afraid.org)                                             |  -                 |

Also, [nip.io](https://nip.io) can be used to mimic FQDNs in case of private (local) or public IP-based deployment/hosting 

Keep in mind that each part of your infrastructure configuration must be reproducible. So, it's recommended to utilize
APIs, if they exist, to automate setting DNS records, regardless

__*Example without DNS delegation (DigitalOcean + Namecheap):*__

1. Register a domain (e.g. `domain.tld`) on Namecheap by using its web console (after signing up)
2. Create records, e.g. `sub.doamin.tld` on Namecheap with OpenTofu


__*Example with DNS delegation (AWS + Name.com):*__

1. Register a domain (e.g. `domain.tld`) on Name.com by using its API (after signing up)
2. Create a *Hosted Zone* for that domain on AWS Route 53 with OpenTofu
3. Set the name servers defined by the *Hosted Zone* as custom name servers of your domain via Name.com API
4. Create additional records, e.g. `sub.doamin.tld` in the *Hosted Zone* on AWS Route 53 with OpenTofu


## Which registry supports pushing container images? 

The following registries are hosted offerings:

* [GitLab Container Registry](https://docs.gitlab.com/ee/user/packages/container_registry/) [public, private]
* [Github Container Registry (ghcr.io)](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry) [public, private]
* [Quay](https://quay.io) *by Read Hat* [public, private]
* [canister](https://canister.io/) [private]
* [public.ecr.aws](https://public.ecr.aws) *by Amazon AWS* [public, private]
* [docker.io](https://hub.docker.com/) [public, private]
* [gcr.io](https://console.cloud.google.com/gcr/images/google-containers/GLOBAL) *by Google Cloud Platform* [public, private]
* [Azure Container Registry (ACR)](https://azure.microsoft.com/en-us/products/container-registry/) *by Microsoft* [private]

But it's also possible to host a registry yourself:

* [Project Quay](https://www.projectquay.io/)
* [Harbor](https://goharbor.io/)
* [Nexus](https://www.sonatype.com/products/sonatype-nexus-oss)
* [Docker Registry](https://hub.docker.com/_/registry)
