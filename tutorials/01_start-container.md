01 - Starting a container
=========================


### Objective(s)

* successfully launch a container
* verify that your credentials from your AWS Educate account work


### Prerequisite(s)

* container engine is installed (e.g. Docker, Podman)
* valid AWS credentials that are configured in `~/.aws`


### Task(s)

1. Start a container based on an [image](https://hub.docker.com/r/etherpad/etherpad) that run an 
   [Etherpad server](https://github.com/ether/etherpad-lite) inside and expose the server in a way
   so that you are able to retrieve the landing page in your browser
   
2. Open up a shell in a container that contains the AWS cli. Mount your credentials into the
   container and verify that you are able to create resources.


### Solution(s)

Can be found over in [lecture-devops-code](https://github.com/lucendio/lecture-devops-code/tree/master/tutorials-solutions/01_start-container).
