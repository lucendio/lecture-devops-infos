---
title: 'Define & run a pipeline'
---


Define and run a pipeline
=========================


## Objectives

* gain basic knowledge of how to use an automation platform
* configure and run a CI/CD pipeline


## Prerequisites

* `git` is installed on your workstation
* access to the source code of an application


## Remarks

* [GitLab: Quick start](https://docs.gitlab.com/ee/ci/quick_start/)
* [Jenkins: Declarative pipeline](https://www.jenkins.io/doc/book/pipeline/syntax/#declarative-pipeline)
* [Github Actions: Workflow syntax](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions)


## Tasks

0. Set up a Git repository (locally & remote)

   * fork the application code of your choosing

1. Choose an automation platform 

    {{< hint info >}}
In cases, where code repository and automation platform are not one and the same, it might be
required to tell the automation platform where the repository is actually located. (e.g. create a new
item in Jenkins and add the Github URL)
    {{< /hint >}}

2. Consult the documentation to find out how to define and configure a pipeline  

    {{< hint >}}
Following the approach of configuration-as-code, it is recommended to place the pipeline definition
right 'next' to the source code that it's supposed to process.
    {{< /hint >}}

3. Configure at least three stages (e.g. build, test, release) which eventually would produce an artifact
   (e.g. executable, archive bundle, container image) 

    {{< hint >}}
The build steps carried out in the pipeline are probably very similar if not even the same used to build
the application locally on your workstation. Often, it's just a matter of adding the build commands to a shell/script
section in the pipeline definition.
    {{< /hint >}}

    {{< hint info >}}
If the code base of your chosen application does not contain any tests (e.g. unit tests) or even a test framework 
consider other ways to verify whether code works or not. Starting the app and check its response might suffice.
    {{< /hint >}}

4. Develop a branch strategy that would allow to publish artifacts in a controlled manner (e.g. merging into a certain
   branch or tagging a commit) but still always run tests 

5. Verify via browser that the artifact is actually published, download it and start the application locally


## Deliverables

* source code of the pipeline


## Solution

*Please note that this solution utilizes the automation platform GitLab. Source code
can be found
[here](https://github.com/lucendio/lecture-devops-code/tree/master/tutorials/05_define-and-run-pipeline).*

{{< hint info >}}
#### Useful links:
* [GitLab: CI/CD](https://docs.gitlab.com/ee/ci/README.html)
* [GitLab: Keyword reference](https://docs.gitlab.com/ee/ci/yaml/README.html)
* [GitLab: Workflow examples](https://docs.gitlab.com/ee/ci/yaml/workflow.html)
* [GitLab: Publish a generic package by using CI/CD](https://docs.gitlab.com/ee/user/packages/generic_packages/#publish-a-generic-package-by-using-cicd)
{{< /hint >}}


### (0) Preparations

* fork the [application repository](https://gitlab.bht-berlin.de/fb6-wp11-devops/webservice) 
* clone the source code to the local workspace
* check CI/CD settings `Settings > CI/CD` to see if everything is in order


### (1) Write a very simple pipeline and verify that it's being executed

`.gitlab-ci.yml`:
```yml
hello-world-job:
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
The logs of the job should show the top-level content of the repository.
{{< /hint >}}

{{< hint warning >}}
in case of 'yaml invalid' errors, check the `CI/CD > Editor` for a more detailed explanation.
{{< /hint >}}


### (3) Write a pipeline that ...

... tests the code/artifact - see stage:
[`test` in `.gitlab-ci.yml`](https://github.com/lucendio/lecture-devops-code/blob/fa968d5f32fc91649fddb30d8eda9147669a660b/tutorials/05_define-and-run-pipeline/.gitlab-ci.yml#L23:L28)

... builds an artifact - see stage:
[`build` in `.gitlab-ci.yml`](https://github.com/lucendio/lecture-devops-code/blob/fa968d5f32fc91649fddb30d8eda9147669a660b/tutorials/05_define-and-run-pipeline/.gitlab-ci.yml#L31:L43)

... publishes the artifact(s) - see stage:
[`publish` in `.gitlab-ci.yml`](https://github.com/lucendio/lecture-devops-code/blob/fa968d5f32fc91649fddb30d8eda9147669a660b/tutorials/05_define-and-run-pipeline/.gitlab-ci.yml#L46:L67)

{{< hint >}}
Use the GitLab-internal [Package](https://docs.gitlab.com/ee/user/packages/package_registry/) Registry.
{{< /hint >}}


### (4) Implement a mechanism to control artifact publishing

By utilizing `workflow` one can control when the pipeline itself would run, and `rules` define whether a single job will
be executed. The [workflow](https://github.com/lucendio/lecture-devops-code/blob/fa968d5f32fc91649fddb30d8eda9147669a660b/tutorials/05_define-and-run-pipeline/.gitlab-ci.yml#L1:L12)
makes sure, that the pipeline is triggered either by a commit to a branch or a change in a merge request.
The [rule](https://github.com/lucendio/lecture-devops-code/blob/fa968d5f32fc91649fddb30d8eda9147669a660b/tutorials/05_define-and-run-pipeline/.gitlab-ci.yml#L49:L50)
that checks the commit ref name prevents the artifact from being published when the branch name is something other than
`stable`.

### (5) Verify that the artifact was published

Under *Packages & Registries > [Package] Registry* you should be able to see and even
download the artifact.

```bash
./webservice 

 ┌───────────────────────────────────────────────────┐ 
 │                    webservice                     │ 
 │                   Fiber v2.49.2                   │ 
 │               http://127.0.0.1:3000               │ 
 │                                                   │ 
 │ Handlers ............. 7  Processes ........... 1 │ 
 │ Prefork ....... Disabled  PID ............. ***** │ 
 └───────────────────────────────────────────────────┘ 

```
