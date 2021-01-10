05 - Install and set up a CI/CD system
======================================


### Objective(s)

* install Jenkins server in an automated fashion
* Set up and configure Jenkins as needed
* Jenkins web console is accessible, you can login successfully and a pipeline could be configured 


### Prerequisite(s)

* a Configuration Management Tool or Container Runtime is installed
* (optional) a running virtual machine (min. 2 GB memory), accessible via SSH


### Task(s)

1. Leverage the work others already did
   a) Does a 3rd-party container image already exist? One that might even be officially supported?
   b) Are there any packages supported by the CM tool you use, that can ease with the installation? 

2. Make yourself familiar with the documentation according to your choice and eventually install
   Jenkins.

   *You may want to disable the so called 'Setup/Upgrade Wizard' and instead go with 3.* 

3. (optional) Utilize the
   [*Jenkins Configuration as Code (a.k.a. JCasC)*](https://github.com/jenkinsci/configuration-as-code-plugin)
   approach to make your setup reproducible
   
4. Expose the web console (if you went through the 'Setup Wizard' this might already be the case) and
   try to login. 

*__NOTE:__You may find some of the [official tutorials](https://www.jenkins.io/doc/tutorials) helpful, or the one
on [Docker & JcaC](https://www.digitalocean.com/community/tutorials/how-to-automate-jenkins-setup-with-docker-and-jenkins-configuration-as-code).*


### Solution(s)

Can be found over in [lecture-devops-code](https://github.com/lucendio/lecture-devops-code/tree/master/tutorials-solutions/05_install-cicd-system).
