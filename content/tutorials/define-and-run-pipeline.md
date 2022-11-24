---
title: 'Define & run a pipeline'
---


Define and run a pipeline
=========================


## Objectives

* gain basic knowledge of how to use an automation platform
* configure and trigger a CI/CD pipeline


## Remarks

* [GitLab: Quick start](https://docs.gitlab.com/ee/ci/quick_start/)
* [Jenkins: Declarative pipeline](https://www.jenkins.io/doc/book/pipeline/syntax/#declarative-pipeline)
* [Github Actions: Workflow syntax](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions)


## Prerequisites

* `git` is installed on your workstation


## Tasks

0. Set up an empty Git repository (locally & remote) and add
   * application code of your choosing
   * other files/code required to build the application (e.g. container file)

1. Choose an automation platform 

    {{< hint info >}}
In cases, where code repository and automation platform are not one and the same, it might be
required to tell the automation platform where the repository actually is located. (e.g. create a new
item in Jenkins and add the Github URL)
    {{< /hint >}}

2. Consult the documentation to find out how to define and configure a pipeline  

    {{< hint >}}
Following the approach of configuration-as-code, it is recommended to place the pipeline definition
right 'next' to the source code it's supposed to process.
    {{< /hint >}}

3. Configure at least three stages (build, test, release) which eventually would produce an artifact that is a 
   container image and/or an archive bundle containing the executable source 

    {{< hint >}}
The build steps carried out in the pipeline are probably very similar if not even the same used to build
the application locally on your workstation. Often, it's just a matter of adding the build commands to a shell/script
section in the pipeline definition.
    {{< /hint >}}

    {{< hint info >}}
The solution does not contain any tests or even a test framework, but there are other ways to
verify whether code works or not. Starting the app and check its response might suffice.*
    {{< /hint >}}

4. Verify via browser that the artifact is actually published


## Solution

*Please note that this solution utilizes the automation platform GitLab. Source code
can be found
[here](https://github.com/lucendio/lecture-devops-code/tree/master/tutorials/05_define-and-run-pipeline).*

{{< hint info >}}
#### Useful links:
* [GitLab: CI/CD](https://docs.gitlab.com/ee/ci/README.html)
* [GitLab: Keyword reference](https://docs.gitlab.com/ee/ci/yaml/README.html)
* [GitLab: Building container images](https://docs.gitlab.com/ee/ci/docker/using_docker_build.html)
{{< /hint >}}


### (0) Preparations

* create a new project
* push the example code
* check CI/CD settings `Settings > CI/CD` to see if everything is in order


### (1) Write a very simple pipeline and verify that it's being executed

`.gitlab-ci.yml`:
```yml
test-job:
  script:
      - 'ls -al ./'
```

```bash
git add .gitlab-ci.yml
git commit -m 'testing GitLab CI/CD with a simple pipeline'
git push
```

Under `CI/CD > Pipelines`, you should see a pipeline execution with the status *passed*. Click on one of
the stage cycles in order to get to the log of a job execution. 

{{< hint >}}
The log of the job should show the top-level content of the repository.
{{< /hint >}}

{{< hint warning >}}
in case of 'yaml invalid' errors, check the `CI/CD > Editor` for a more detailed explanation.
{{< /hint >}}


### (2) Write a pipeline that ...

... builds an artifact (archived bundle a/o container image) - see stage:
[`build` in `.gitlab-ci.yml`](https://github.com/lucendio/lecture-devops-code/blob/cc6647dda647a1fe2c0e23f303d6db25c97bdfb0/tutorials/05_define-and-run-pipeline/.gitlab-ci.yml#L16)

{{< hint warning >}}
For container images, please note the docker-in-docker dilemma (see [Useful links]({{< relref "#useful-links" >}}))
{{< /hint >}}

... tests the code/artifact - see stage:
[`test` in `.gitlab-ci.yml`](https://github.com/lucendio/lecture-devops-code/blob/cc6647dda647a1fe2c0e23f303d6db25c97bdfb0/tutorials/05_define-and-run-pipeline/.gitlab-ci.yml#L34)

Start a container based on the image produced during build stage, and either check from the outside
if Nginx runs (see solution), or from the inside by
[explicitly configure the image built during previous stage for the test job](https://docs.gitlab.com/ee/ci/yaml/README.html#image).

... publishes the artifact(s) - see stage:
[`publish` in `.gitlab-ci.yml`](https://github.com/lucendio/lecture-devops-code/blob/cc6647dda647a1fe2c0e23f303d6db25c97bdfb0/tutorials/05_define-and-run-pipeline/.gitlab-ci.yml#L66)

{{< hint >}}
Use the GitLab-internal [Package](https://docs.gitlab.com/ee/user/packages/package_registry/) or 
[Container Image](https://docs.gitlab.com/ee/user/packages/container_registry/) Registries.
{{< /hint >}}

### (3) Verify that the artifacts were published

Under *Packages & Registries > [Package,Container] Registry* you should be able to see and even
download the artifact.
