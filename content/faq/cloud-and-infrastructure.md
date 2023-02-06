---
title: 'Cloud & Infrastructure'
---


FAQ - Cloud & Infrastructure
============================


## Where do I get some infrastructure to play with, to do the tutorials with, or even for the assignment?

Essentially, you have 3 different options:

a) your workstation has enough resources (CPU, memory) to host one or more virtual machines, which would enable you
   to mimic some *cloud resources* locally - as you know: the cloud is just a bunch of other people's computers

b) have a look at the link section on [*cloud providers*]({{< ref "/links#cloud-providers" >}})

c) the university provides a Kubernetes cluster managed with Rancher (requires student account, and VPN access if you
   are not on-site)


## Which cloud provider should I use?

As mentioned before, there is a variety of common [*cloud providers*]({{< ref "/links#cloud-providers" >}}) you can choose from.
While all of them provide similar concepts which are sometimes named differently, they may have some limitations:

* AWS Academy (formerly called *Educate*), e.g. see
  [PDF](/assets/AWS-Academy-Learner-Lab_Foundational-Services_20211002.pdf) for a complete list:
  * limit the amount or size of machines (EC2 instances) that you are allowed to spin up
  * no IAM
  * no NAT gateway creation
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
5. Take a look at the *Readme* (top right) for further information, e.g. available Services or restrictions
6. To access the AWS management console (GUI), click on *AWS* right next to the green status indicator (top left)
7. To obtain your credentials, follow the same navigation path from (4.) followed by *AWS Details* (top right) --> AWS
   CLI: *Show* button
   As an alternative, you may want to use the 
   [`aws-academy.sh` script](https://github.com/lucendio/lecture-devops-code/blob/master/hack/aws-academy.sh)
   and run it every 4 hours since a session only lasts so long.

Follow the [official documentation](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html) in order
to configure the CLI to use the credentials.

[^1]: [Official website](https://aws.amazon.com/training/awsacademy/) of the *AWS Academy*


## Which AWS services are supported with the AWS Academy program?

Please refer to [this document](/assets/AWS-Academy-Learner-Lab_Foundational-Services_20211002.pdf) in order to find
out which services are available in an AWS Academy account. Alternatively, you may find these information in mentioned
in *6.8*. Furthermore, please be aware that all services are located in the `us-east-1` or `us-west-2` regions, hence
hosted outside of the european jurisdiction (no GDPR etc.).


## Which public DNS provider offers free domain registration and allows automated record creation? {id="dns-provider-free-and-automation"}

A non-exhaustive list of public DNS providers:

| Name                                             | Free Domain            | API                                                                          | Terraform provider |
|--------------------------------------------------|:----------------------:|------------------------------------------------------------------------------|:------------------:|
| [Namecheap](https://www.namecheap.com)           | YES (via Github Edu)   | [URL](https://www.namecheap.com/support/api/intro/)                          |  X                 |
| [Name.com](https://www.name.com)                 | YES (via Github Edu)   | [URL](https://www.name.com/api-docs)                                         |  -                 |
| [INWX](https://www.inwx.de/en)                   | NO                     | [URL](https://www.inwx.de/en/offer/api)                                      |  X                 |
| [AWS Route 53](https://aws.amazon.com/route53/)  | NO                     | [URL](https://docs.aws.amazon.com/Route53/latest/APIReference/Welcome.html)  |  X                 |
| [GCP Cloud DNS](https://cloud.google.com/dns)    | NO                     | [URL](https://cloud.google.com/dns/docs/apis)                                |  X                 |
| [Freenom](https://freenom.com)                   | YES                    | [URL](https://www.freenom.com/en/freenom-api.html)                           |  -                 |

For local deployment/hosting, [nip.io](https://nip.io) can be used to mimic FQDNs. 

Keep in mind that each part of your infrastructure configuration 

__*Example (AWS + Freenom):*__

1. Register a domain (e.g. `domain.tld`) on Freenom by using its API (after signing up)
2. Create a *Hosted Zone* for that domain on AWS Route 53 with Terraform
3. Set the name servers defined by the *Hosted Zone* as custom name servers of your domain via Freenom API
4. Create additional records, e.g. `sub.doamin.tld` in the *Hosted Zone* on AWS Route 53 with Terraform


## How to obtain the `.kube/config` (`KUBECONFIG`) file necessary to access the university's Kubernetes cluster? {id="kubeconfig-of-edu-cluster"}

{{< hint warning >}}
Did you already enter your university email address in the respective account list?
{{< /hint >}}

1. *(optional)* [login to the VPN](https://doku.bht-berlin.de/zugang/vpn)
2. log in to the [cluster management web console](https://rancher.ris.bht-berlin.de)
   with your university account 
3. click on *edu-cluster* in the list of clusters
4. click on *cluster* in the main navigation bar (top left next to *edu-cluster*) - you should see
   some kind of resource utilization meters in the main area of the page
5. obtain the *Kubeconfig File* (top right) and store the content on your workstation under `~/.kube/config`
   or in whatever location your `KUBECONFIG` environment variable points to
