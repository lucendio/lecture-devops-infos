Frequently ask questions
========================


#### 1. Where do I even start?

The first step would be to get an overview of what [this repository](./README.md#table-of-contents) and the
[external resources](./README.md#external-resources) contain. As a second step, you might want to grab a coffee
and click your way through the [lists of link collections on *DevOps*](./links.md#devops-1). Most of them not
only provide a comprehensive list of technologies but refer to books, blogs and other knowledgeable resources, too.
Then, a next step for you would be to try and get [the application](https://github.com/lucendio/lecture-devops-app),
that is going to be deployed as part of the *project implementation*, up and running on your workstation.

You might also find [this roadmap](https://roadmap.sh/devops) useful. It shows you a possible learning path. Towards
the end, you may postpone the monitoring bits until the end and instead have a look at cloud-related things first.


#### 2. I have no clue about DevOps. How am I supposed to write this concept?

Don't worry. It's just an initial step to get you start digging into things related to DevOps. After handing in the
(first version of the) concept you'll receive individual feedback, whihc you can then use to iterate over your design
and implementation. So, nothing is set in store. You can change things along the way - after some consultation first.


#### 3. Where do I get some infrastructure to play with or to be used to do the tutorials or even fir the project work assignment?

Have a look at the link section on [*cloud providers*](./links.md#providers).


#### 4. How do I get access to AWS and unlock [AWS Educate](https://aws.amazon.com/education/awseducate/) credits?

1. Add your mail address to the provided list
2. When receiving the invitation email (subject: *Your AWS Educate Application*), follow the link to complete the AWS
   Educate application process. In case you already have a normal AWS account and you wish to use it - it is possible
   to *'connect'* them during the registration process.
3. Receive an approval notification via email and log in to the [AWS Educate platform](https://www.awseducate.com/signin/SiteLogin)
4. Once you logged the first time, choose *AWS Educate Starter Account* and click *Get Started* --> click button to
   create the account
5. To obtain your AWS credentials, go to *AWS Account* (top right) --> *AWS Educate Starter Account* button (opens a
   new window served by *labs.vocareum.com*) --> *Account Details* button --> AWS CLI: *Show* button.
   As an alternative, you may want to use the 
   [`refresh-aws-session.sh` script](https://github.com/lucendio/lecture-devops-code/blob/master/hack/refresh-aws-session.sh)
   and run it every 3 hours since a session only lasts so long.

Follow the [official documentation](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html) in order
to configure the CLI to use the credentials.


#### 5. Which AWS services are supported with the AWS Educate program?

Please refer to [this document](https://awseducate-starter-account-services.s3.amazonaws.com/AWS_Educate_Starter_Account_Services_Supported.pdf)
in order to find out which services are available in an AWS Educate account. Furthermore, please be aware that all services are
located in the `us-east-1` region, hence hosted outside of the european jurisdiction (no GDPR etc.).


#### 6. Which public DNS provider offers free domain registration and allows automated record creation?

A non-exhaustive list of public DNS providers:

| Name                                             | Free Domain            | API                                                                          | Terraform provider |
|--------------------------------------------------|:----------------------:|------------------------------------------------------------------------------|:------------------:|
| [Namecheap](https://www.namecheap.com)           | YES (via Github Edu)   | [URL](https://www.namecheap.com/support/api/intro/)                          |  X                 |
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
