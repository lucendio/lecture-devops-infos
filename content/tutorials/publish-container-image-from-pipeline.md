---
title: 'Publish a container image from CI/CD'
---


Build & publish a container image from CI/CD
============================================


## Objectives

* build a container image from within a CI/CD pipeline
* publish the container image in a container image registry


## Prerequisites

* Tutorial: [Define & run a pipeline]({{< relref "./define-and-run-pipeline" >}})
* Tutorial: [Build a container image & start a container]({{< relref "./build-container-image-and-start-container" >}})


## Remarks

* [GitLab: CI/CD](https://docs.gitlab.com/ee/ci/README.html)
* [GitLab: Keyword reference](https://docs.gitlab.com/ee/ci/yaml/README.html)
* [GitLab: Building container images with Docker-in-Docker](https://docs.gitlab.com/ee/ci/docker/using_docker_build.html#use-docker-in-docker)

{{< hint warning >}}
Depending on the CI/CD agent, you may face the docker-in-docker dilemma (see last link above)
{{< /hint >}}


## Tasks

1. Adjust the existing `Containerfile` if necessary
2. Define pipeline job to build and publish a container image in a container image registry of your choosing
3. Verify the result by pulling the image from the registry into the local workspace and start a container
   based on that image


## Deliverables

* source code of the pipeline
* `Containerfile`


## Solution

*Please note that this solution utilizes the automation platform GitLab. Source code
can be found
[here](https://github.com/lucendio/lecture-devops-code/tree/master/tutorials/publish-container-image-from-pipeline).*


### (0) Preparations

* fork and clone the [application repository](https://gitlab.bht-berlin.de/fb6-wp11-devops/webservice) 
  to the local workspace
* if not already present, copy over `.gitlab-ci.yml` and `Containerfile` created in previous tutorials


### (1) Adjust `Containerfile` if needed

{{< hint >}}
You may want to make the adjustments in a copy of the existing `Containerfile` (e.g. `Containerfile.ci`)
{{< /hint >}}
 
If you want to replace the existing build job with a
[multi-stage build](https://docs.docker.com/build/building/multi-stage/) approach, you may remove the build job
from the pipeline and instead add a first (build) stage to the `Containerfile`.

{{< hint >}}
Such change would make sense mainly if jobs are not already executed within a container
{{< /hint >}}


### (2) Define the pipeline job

Since the container technology as we know them is based on Linux technologies, the `matrix` build is not needed in
this case. The target platform is most likely either `amd64` or `arm64`.

Change the *upload* stage as follows:

Add a `services` attribute to the job based on Docker's
[*Docker-in-Docker* image](https://docs.gitlab.com/ee/ci/docker/using_docker_build.html#use-docker-in-docker)
in order to gain access to the Docker daemon "outside" of the job's container scope.

Create the image with a container image build tools (e.g. `podman` [`buildah`], `docker`) available within
the pipeline. Tag the image appropriately. If necessary, login to the container image registry, and push the image
to that registry by providing the fully qualified container image name, which consists of:

    <registryFQDN>/<[project,repository,user,scope,...]+>/<name>:<tag> 

{{< hint >}}
If available, use the GitLab-internal
[Container Image Registry](https://docs.gitlab.com/ee/user/packages/container_registry/)
{{< /hint >}}

At the end of the job, remove the newly container image from the local container image cache of the CI/CD agent. 


### (3) Verify the result

In GitLab, navigate to *Project --> Packages and registries --> Container Registry --> Repository*, which should
list the tag(s) published by the pipeline.

Pull the container image 

```bash
podman pull ${FULLY_QUALIFIED_CONTAINER_IMAGE_NAME}
```

and start a container based on the image

```bash
podman run --name my-container ${FULLY_QUALIFIED_CONTAINER_IMAGE_NAME}
```
