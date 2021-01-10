04 - Automate web-server installation and apply configuration to a VM
=====================================================================


### Objective(s)

* install and configure Nginx on a virtual machine in an automated fashion
* tell Nginx to serve a custom landing page


### Prerequisite(s)

* Ansible is installed
* a running virtual machine with SSH access - either locally (Vagrant) or in the cloud (AWS)


### Task(s)

1. Write Ansible code (*playbook*, *inventory*, possibly a *role*) to automate the installation and
   configuration of Nginx

2. Apply the configuration to the virtual machine

3. Verify if your own custom landing page shows up in the browser  
   
2. Install *Nginx* into a virtual machine, expose it to the host and confirm with your browser that
   the default page is being served.
   
You can either create your own *vhost* by defining a
[`server` directive](https://docs.nginx.com/nginx/admin-guide/web-server/web-server/#setting-up-virtual-servers)
to server your custom landing page or you override the existing default page. 


### Solution(s)

Can be found over in [lecture-devops-code](https://github.com/lucendio/lecture-devops-code/tree/master/tutorials-solutions/04_automate-webserver-installation).
