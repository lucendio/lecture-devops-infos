---
title: 'Build a container image & start a container'
---


Build a container image and start a container
=============================================


## Objectives

* successfully launch a container
* verify that your credentials from your AWS Academy account work


## Prerequisites

* container engine is installed (e.g. Docker, Podman)
* valid AWS credentials that are configured in `~/.aws`


## Tasks

1. Build a container image based on a 
   [Containerfile](https://www.mankier.com/5/Containerfile)
   (e.g. `Dockerfile`). Start a container based on that image and expose the Nginx server
   in a way so that you are able to retrieve the landing page in your browser.   
2. Open up a shell in a container that contains the AWS cli. Mount your credentials into the
   container and verify that you are able to create resources.


## Solution

*Source code can be found
[here](https://github.com/lucendio/lecture-devops-code/tree/master/tutorials/01_build-container-image-and-start-container).*

Note, that even though the commands below user `podman`, the solution is equally valid if replaced with `docker`.


### (1) Building and running a Node server isolated in a container

```bash
podman build --file ./Containerfile --tag my-container-image:1.0.0 ./
podman run --publish 3000:80 --name my-container my-container-image:1.0.0
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

1. check the HTTP interface; either by opening up the URL in your browser or by using the command line:

    ```bash
    curl http://${PODMAN_HOST_IP}:3000
    ```
    *Response:* `[...] Hello World! [...]`

    {{< hint info >}}
Depending on *where* the container runtime actually has spawned the container, `PODMAN_HOST_IP` might be
an IP of a virtual machine running on your host system, or, if Linux is the host system, the value is probably
`127.0.0.1` aka. `localhost`.
    {{< /hint >}}

2.  use the container management tool

    ```bash
    podman ps 
    ```
    *Result:*
    ```
    CONTAINER ID  IMAGE                               COMMAND               CREATED        STATUS            PORTS                 NAMES
    8fb77582c6e5  localhost/my-container-image:1.0.0  nginx -g daemon o...  3 seconds ago  Up 3 seconds ago  0.0.0.0:3000->80/tcp  my-container
    ```


### (2) Creating AWS resources from within a container

1. Verify credentials

    ```bash
    podman run \
        --mount type=bind,source=${HOME}/.aws,destination=/root/.aws,readonly \
        --interactive \
        --tty \
        --rm \
        docker.io/amazon/aws-cli \
        sts get-caller-identity
    ```

2. Create a bucket

    Start a container:
    ```bash
    podman run \
        --name aws-container \
        --mount type=bind,source=${HOME}/.aws,destination=/root/.aws,readonly \
        --detach \
        --entrypoint sh \
        docker.io/amazon/aws-cli \
        -c 'while true; do sleep 3600; done'
    ```
    
    Allocate a shell in the container:
    ```bash
    podman exec \
        --interactive \
        --tty \
        aws-container \
        /bin/bash
    ```
    
    Actually create the bucket:
    ```bash
    aws s3api create-bucket --bucket my-globally-unique-bucket-name
    ```

    {{< hint info >}}
Check afterwards if the bucket actually exists: `s3 ls`. __Don't forget to delete the bucket
again after you are done: `aws s3api delete-bucket --bucket ${BUCKET_NAME}`__
    {{< /hint >}}

    Remove the container:
    ```bash
    podman rm --force aws-container
    ```

{{< hint >}}
More information and examples can be found in the
[official AWS CLI user guide](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2-docker.html).
{{< /hint >}}
