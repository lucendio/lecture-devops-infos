A guided path *(WIP)*
=====================


*Please note, that the following outlines do not claim to be exhaustive.*


### 20 weeks to go - a rough timeline to help you structure the work ahead

*__DISCLAIMER__: This is no one-size-fits-all but merely an incomplete suggestion. It may or may not be helpful 
on your journey through the course, and it's probably not (fully) in sync with the course road map.*

Based on an average of 20 weeks (starting with the course *onboarding* and concluding with *review*).

| Week  | TODOs                                                                                                         |
| ----- |-------------------------------------------------------------------------------------------------------------- |
| `01`  | Go through the provided materials and note down questions                                                     |
| `02`  | Familiarize yourself with the [scope](./../links.md#devops) of this course and its topics                     | 
| `03`  | Refresh [linux](./../links.md#unixlinux-basics) and [git](./../links.md#git) skills                           |
| `04`  | [Choose](./../faq.md#12-how-to-choose-the-application-for-the-project-assignment) & fork an app, that will be used as *deployable workload* and get it to work locally  |
| `05`  | Revisit the *12-Factor-App*; install a container runtime on your workstation, start some containers and try to also build some container images  |
| `06`  | Install a hypervisor on your workstation; start a virtual machine; connect via terminal to a shell inside that machine; find out which process manager is being used and try to stop and start some of the processes running in there  |
| `07`  | Revisit the agile-based software development life cycle; start to note down your first thoughts about your project implementation; allocate some cloud resources (e.g. machines, networks, storage, managed services)  |
| `08`  | Automate the resource allocation so that it becomes reproducible; destroy and re-create your setup            |
| `09`  | Use a Configuration Management Tool to install and configure some software/services on these machines         |
| `10`  | Learn about CI/CD and think about its role in your architecture; write a pipeline and set up a trigger        |
| `11`  | Finish the initial version of your concept; add a diagram (or some other visualization) to show off your architecture design  |
| `12`  | Transform the application source code into a deployable artifact; incorporate this process into a pipeline (that's the CI part) and set up a trigger; of course, this requires a CI/CD platform to begin with, so make sure you have one - either deploy (automate!) one yourself (remember poll vs. push) or use a managed one (SaaS)  |  
| `13`  | Set up an environment founded on the knowledge you gained and the technologies you learned about over the past weeks; deploy the application and automate the process; implement some deployment strategy to prevent any down time  |
| `14`  | Evaluate Container Orchestration to see whether it would bring any value to your setup                        |
| `15`  | Integrate the application deployment into your CI/CD platform (that's the CD part) to reflect a complete software development life cycle  |
| `16`  | Define and provision a second target environment; configure another trigger; ensure that the application successfully deploys to that environment, too  |
| `17`  | Evaluate monitoring solutions and go through their documentation to understand how it's being deployed and configured; maybe do a test deployment - by hand - to allow easier debugging; take notes  |
| `18`  | Use those notes to automate the provisioning of that monitoring service; integrate and document               |
| `19`  | Ensure complete reproducibility by destroying the whole setup and re-creating it from scratch; make sure everything still works  |
| `20`  | Double-check the grading criteria and see if they are met; plan and trial run the review presentation         |


### A possible approach

#### (0) Thinking out loud

1. What do you already know about app deployment, automation and infrastructure (from your
   previous courses & projects a/o from you work place)?

2. What is the application made of that you are supposed to deploy? What tool chain is
   used to create it? Which dependencies does it have?

3. What would be the simplest architecture (if the software development life cycle would
   be completely automated)?

*Imagine, you follow the app from its written source all the way down to when it's actually
being started, so that others can access it on the internet.*
```
  Source code            Automation                    Deployment
  -----------    ===>    ------------------    ===>    ----------------
  (app code)             (test + build app)            (host / run app) 
                                                      
                          * Who?                        * Where?
                          * Where?                      * How is it getting there?
                          * Tool chain?               
                          * Dependencies?             
```
Which steps would the app need to go through?

4. Which (backing) services would be needed to facilitate this process? How to set them
   up in a reproducible way? Which of them will be provisioned by yourself and which ones
   can be managed services?


#### (1) Preparation

* [does your workstation provide the right working environment](./../faq.md#3-my-workstation-runs-microsoft-windows-where-do-i-get-a-unix-like-context-from-to-work-in)? 

* if you have little to no experience working with POSIX-based systems a/o using the terminal, take a look at the 
  [tutorial](./../links.md#basic-knowledge) and spent some time making yourself familiar with these technologies

* [choose an application](./../faq.md#12-how-to-choose-the-application-for-the-project-assignment) and try to get it
  to work locally

* think about where you want to host your infrastructure (and thus the application) 
  * is one of the [available cloud providers](./../links.md#providers) an option?
  * does your own workstation have enough resources (to host one or more services a/o virtual machines)?
  * if none of these options work for you, please get in touch
  * (or if you are uncertain about this whole part)

* take a look at some of the core technologies you might be going to use
  * runtime:
    * hypervisors: VirtualBox, QEMU, VMware 
    * container engines: Podman, Docker, LXC 
  * automation: 
    * infrastructure-as-code:
      * Terraform, if you want to host (parts of) your project on a public cloud
      * Vagrant, if you want to run things locally (verify that the available resources of your workstation are sufficient)
    * configuration-as-code: Ansible, Chef, Salt
  * CI/CD: Gitlab, Github Actions, Jenkins, Concourse
  * Monitoring: Prometheus + Grafana, EFK Stack

* all the fundamental technologies should be understood (in principle) before the initial version of the 
  [concept](./../assignments/deliverables/project_concept.md) is due


#### (2) Writing the concept

* essential pieces: VCS, CI/CD, infrastructure hosting some services incl. the application itself,
  backing services (database, monitoring)

* make yourself familiar with a few *Internet standards & distributed concepts:* IP, DNS, HTTP(S), load balancing,
  proxying

* describe how the deployment process of the *application* could look like
* think about how you want to create the infrastructure and automate that process (entirely described in code)
* steps necessary to prepare & set up automated infrastructure provisioning or CI/CD (e.g. generate and set up a secret) 

* create a new Git repository and put a `concept.md` in there

* visualize the overall setup that you are planning to implement - maybe [draw a nice diagram](https://app.diagrams.net)

* by the time you hand in the first version of your concept, you should already have started to write some
  infrastructure code, even if it's just a few lines; maybe start by setting up an environment where the application
  is going to be deployed in; usually, one would first need some kind of computer (e.g. virtual machine) or
  compute context (e.g. container runtime) to then configure the environment and deploy a service into it
  
* only explain technologies to an extent that helps to better understand your specific solution, otherwise refer to
  parts of the documentation; commonly or taught (during class) knowledge can already be assumed


#### (3) Implementation

* use the repository that already contains your concept and add all the infrastructure code that is not required to
  sit next to the source code of [your application](./../faq.md#12-how-to-choose-the-application-for-the-project-assignment)  

* fork the repository of the application you [chose](./../faq.md#12-how-to-choose-the-application-for-the-project-assignment)
  so that you are able to introduce changes necessary to integrate it with the rest of your setup


#### (4) Deliverables

* make sure that each part of your infrastructure configuration is either defined and described by 'code', or if not,
  then at least document the steps you have taken in detail (e.g. configuring DNS in a provider's web console, 
  generate and set up a secret for CI/CD); so that everything is reproducible when reviewing and trying to follow your
  work
