---
title: 'Build images & start containers'
---


Build a container image and start a container based on that image
=================================================================


## Objectives

* learn about two conceptual distinct use cases for containers: [1] *ever running* (daemon)
  and [2] *intentional terminate/conclude* (one-off)
* successfully launch a container
* verify that your credentials from your AWS Academy account work


## Prerequisites

* container engine is installed (e.g. Podman)
* valid AWS credentials located in `~/.aws`

{{< hint >}}
On non Linux-based systems, [installing Podman](https://podman.io/docs/installation) comes with a virtual machine under
the hood, see the `podman machine` command. In case a graphical user interface is preferred, take a look at
[podman desktop](https://podman-desktop.io/downloads). Thirdly, while the actual engine is running on a virtual machine
(guest), the *"remote client"* `podman` can be invoked on the workstation (host). For more details, please refer to
the respective [tutorial(s)](https://docs.podman.io/en/latest/Tutorials.html).
{{< /hint >}}


## Tasks

1. Build a container image based on a 
   [Containerfile](https://github.com/containers/common/blob/main/docs/Containerfile.5.md)
   (formerly known as [`Dockerfile`](https://docs.docker.com/reference/dockerfile/)) and include the
   [webservice](https://gitlab.bht-berlin.de/fb6-wp11-devops/webservice)
2. Start a container based on that image and expose the included service 
   and confirm that it's accessible from the *"outside"* via CLI  
2. Run a container for the purpose of invoking the AWS CLI. Mount your credentials into the
   container and verify that you would be able to create resources


## Deliverables

* `Containerfile` showing contents and default configuration of the container image
* `terminal.log` file showing the commands and their output used to carry out the tasks


## Solution

*Source code can be found
[here](https://github.com/lucendio/lecture-devops-code/tree/master/exercises/build-images-and-start-containers).*


### (1) Build a container image with the static binary inside

{{< hint info >}}
ARM or x86? Check the system on which the container engine is running and set `GOARCH` accordingly
{{< /hint >}}

```bash
git clone ssh://git@gitlab.bht-berlin.de/fb6-wp11-devops/webservice
cd ./webservice
GOARCH=amd64 GOOS=linux CGO_ENABLED=0 go build -o ./artifact.bin ./*.go
```

{{< hint danger >}}
Notice the `CGO_ENABLED=0` above? If starting `FROM scratch` when building the image, a static binary is required
because an empty file system does even contain dynamic libraries. Whereas, if a Linux file system 
(e.g. `FROM public.ecr.aws/lts/ubuntu:24.04`) marks the foundation, said dynamic libraries are part of the file system
already. When cross-compiling i.e. build for a target context that is not the same the build runs on, it always produces
a static binary. Please refer to [this](https://mt165.co.uk/blog/static-link-go/) blog post for more details. 
{{< /hint >}}

Describe the contents of the container image by writing a `Containerfile`

{{< hint info >}}
There are at least two options: (1) build on the host and just put the result inside the
image, or (2) use a [multi-stage build](https://docs.docker.com/build/building/multi-stage/)
{{< /hint >}}

and then build the image:

```bash
podman build --file ./Containerfile --tag my-webservice:1.0.0 ./
```


### (2) Start a container based on this container image
```bash
podman run --publish 8080:3000 --name my-container my-webservice:1.0.0
```

{{< hint warning >}}
You may want to detach from the container shell by adding `--detach` to the command. Also,
in case you already ran the command before, you may need to stop and remove the container:

```bash
podman stop my-container
podman rm my-container
```
{{< /hint >}}

To verify that the container is actually running

1. check the HTTP interface, either by open up the URL in your browser or by using the command line:

    ```bash
    curl http://${PODMAN_HOST_IP}:8080
    ```
    Result:
    ```
    Hello, World!
    ```
    
    {{< hint info >}}
Depending on *where* the container engine actually runs, `PODMAN_HOST_IP` might be an IP of a virtual
machine running on your host system, or, if Linux is the host system, the value is probably
`127.0.0.1` aka. `localhost`.
    {{< /hint >}}


2. use the container management tool

    ```bash
    podman ps 
    ```

   Result:
    ```
    CONTAINER ID  IMAGE                          COMMAND          CREATED        STATUS            PORTS                   NAMES
    b6da3afc59ef  localhost/my-webservice:1.0.0  /bin/webservice  2 minutes ago  Up 2 minutes ago  0.0.0.0:8080->3000/tcp  my-container
    ```


### (3) Execute a CLI through container

Another use case of container images lies in its bundling nature, which makes a dependency tree
easily portable and allows to execute arbitrary programs, like command-line interfaces (CLIs), in isolation.

Invoke the AWS CLI executed in a container and confirm that it functions properly.
