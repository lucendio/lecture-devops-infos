---
bookFlatSection: true
bookCollapseSection: true

title: 'Exercises'
weight: 4
---


Exercises 💡
============

These exercises are intended to be small and __voluntary__ hands-on activities that might be
worked on during class. Ideally, every lecture topic (weekly) comes with one or more
corresponding exercises.

Performing these tasks should help you to get familiar with technologies you
might be going to use in your [project assignment]({{< ref "/assignments/project-work" >}}).

{{< hint >}}
There are also some information on a recommended [tool chain and its OS-compatibility]({{< relref "/tool-chain">}}).
{{< /hint >}}

The exercises vary a bit in their workload but still cover a limited scope. Solutions may only
utilize the basics of the technologies that are involved, while their capabilities would reach
far beyond that.

For more comprehensive exercises either refer to the
[link collection]({{< ref "/links" >}}), their corresponding [lesson]({{< ref "/lessons" >}}) or 
the web search engine of your least distrust.

Each exercise contains:

* a description, including prerequisites, tasks and objectives(s)
* at least one possible solution, including the steps to reproduce, and, it needed, any kind of *assets*


## Skill Tree

The diagram below shows how exercises are build on one another or at least how they are thematically related.
This may help you to determine which exercises to choose when completing the
[assignment]({{< ref "/assignments/exercises" >}}). There are different directions you can go. For example, if you decide
to include containers in to you project stack. Or when you want to follow the path of exploring cloud provider offerings
and resources.

{{< figure src="/graphics/skill-tree.png" >}}

{{< expand "Mermade code" "..." >}}
```
%%{init: {"flowchart": {"defaultRenderer": "elk"}} }%%
flowchart TB
    1[Learn Git workflows]
    2[Define & run a Pipeline]
    3[Spin up virtual machine locally]
    4[Build images & start containers]
    5[
        Allocate virtual
        machine in cloud
    ]
    6[
        Automate system configuration
    ]
    7[Set up CI/CD system]
    8[
        Update version as instance group
        behind load balancer
    ]
    9[
        Publish container
        images from pipeline
    ]
    10[Provision Kubernetes]
    11[Deploy an application on Kubernetes]
    12[Manage Kubernetes objects with Helm]
    13[Collect and visualize metrics]
    14[Investigate system & write exporter]
    15[Persist state on Kubernetes]
    16[Create encrypted backup]
    
    1 --> 2

    2 --> 3
    2 --> 5
    
    3 --> 4
    3 --> 14
    3 --> 6
    

    4 --> 9

    5 --> 14
    5 --> 6
    5 --> 16

    6 --> 7
    6 --> 8
    6 --> 10
    6 --> 13

    9 --> 11

    10 --> 11
    
    11 --> 12
    11 --> 15

    12 --> 15
```
{{< /expand >}}
