---
title: 'Decisions'
---


Making some decisions
=====================


This document should help you to make some decisions regarding your technology stack.

If you rather want to keep some freedom of choice and find more inspiration, take a
look at [these examples]({{< ref "/guide/examples" >}})

In general, you should base your choice on where your interests lie. That said, it's always good
to know the limitations of all the different possibilities. Please read on, if you want lo learn
more about those limitations and get some fruit for thought to make your decisions.


## Remote or local?

Everything you do on your local workstation gives you full control over the entire stack. This is
valuable when trying to figure out why things are not working.
But, the physical resources on that workstation might more limiting. Either this fact a/o the
convenience of just using/allocating a managed service, can make you consider using some remote
services for your implementation.

Nothing prevents you from going for a more hybrid architecture. Though, running a certain component
locally and other remotely may have som implications. For example, deploying the application locally
from a remote CI/CD usually doesn't work - well, it may, but it would involve some hacky workarounds,
and it surely doesn't make things less complicated.

Not having a public IP might feel like a limitation, but it's actually only the case when it comes
to valid (as in *trusted* or not *self-signed*) TLS certificates. Good thing that it's not explicitly
required - neither in the [grading criteria]({{< ref "/grading#implementation" >}}) nor in the
[assignment]({{< ref "/assignments/project-work" >}}) (also see the [tips]({{< ref "/guide/tips#tls" >}}) for more information).


## Kubernetes or not?

Kubernetes is the new kid on the block and has slowly evolved into a de facto industry standard. So,
chances are that you eventually bump into it one way or another over the course of your career.

While it already provides a variety of features out of the box, like horizontal scaling or service discovery,
at the same time, it's complex and hides (or rather *abstracts*) some mechanisms from you, that you otherwise
would need to do *by hand*, but at the same time would understand them better.

Available [*cloud provides*]({{ ref "/links#cloud-providers" }}) may already offer a turnkey-ready managed Kubernetes
cluster. Or, you can provision it yourself, for example, with a couple of virtual machines (spun up locally
or allocated in the cloud) and [Kubespray](https://github.com/kubernetes-sigs/kubespray). The university also
hosts a Kubernetes cluster, that you are free to use.

While one the university cluster you are limited to
[namespaced resources](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/#not-all-objects-are-in-a-namespace),
self-provisioned or managed clusters on the public cloud usually allow administrative access to the entire
cluster. 


## Public cloud provider or university resources?

The university resources are limited to GitLab and a Kubernetes cluster. While this might be everything you
need to implement the project, both have some limitations:

* GitLab: no administrative access to the [runners](https://docs.gitlab.com/runner/); but can [add any other
  machine (workstation or virtual cloud instance) as a runner](https://docs.gitlab.com/runner/register/) to you project
* Kubernetes: [namespaced resources](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/#not-all-objects-are-in-a-namespace) 
  as mentioned above

It's worth mentioning, that the question may not be either or, but which pieces fit into your architecture the
best - according to the requirements that you set for yourself - and which limitations might slow you down in
the process of implementing it.
