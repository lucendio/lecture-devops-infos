---
title: 'Software'
---


Software
========

A list of software and a quick explanation of their purpose and what they could do for you. The 
content is focused on open source software instead of as-a-service solutions or even proprietary
software.

{{< hint >}}
*__NOTE:__ the list is not exhaustive*
{{< /hint >}}


## Version control systems

* [Gitea](https://docs.gitea.io/en-us/)


## Build tools

* [Make](https://www.gnu.org/software/make/manual/make.html#Introduction)
* [Bazel](https://bazel.build/) *(automation tool written in Java)*
* [Bosh](https://bosh.io/docs/problems/) *(release & deploy management)*


## Proxy / vhosting / load balancing

* [Nginx](https://nginx.org/en/docs/beginners_guide.html)
* [HAProxy](https://docs.haproxy.org/2.6/intro.html)
* [Caddy (v2)](https://caddyserver.com/) *(web server with Let's Encrypt support; no LB capabilities)*


## Dev-Security-Ops

* [SOPS](https://github.com/mozilla/sops#usage) *(VCS-compatible en- & decryption of arbitrary text files)*
* [Ansible Vault](https://docs.ansible.com/ansible/latest/user_guide/vault.html) *(built-in en- & decryption of Ansible inventories)*
* ACME clients (automatically renew TLS certificates):
  * [Certbot](https://eff-certbot.readthedocs.io/en/stable/) *(stand-alone)*
  * [cert-manager](https://cert-manager.io/docs/) *(Kubernetes operator)*
* password managers (Keepass-compatible)
  * [KeePassXC](https://keepassxc.org/)
  * [MacPass](https://macpassapp.org/)


## Virtualization

* [VirtualBox](https://www.virtualbox.org/) *(Hypervisor for AMD64/Intel64)*
* [Vagrant](https://developer.hashicorp.com/vagrant/docs) *(Configuration-as-Code abstraction for Type-2 Hypervisors)*
* [QEMU](https://www.qemu.org/docs/master/system/qemu-manpage.html) *(Hardware Emulation)*
* [Packer](https://www.packer.io/docs) *(build virtual machine images)*
* [UTM](https://mac.getutm.app/) *(macOS-only)*
* [Lima](https://lima-vm.io/) *(Linux virtual machines on macOS and Linux)*
* [Hyper-V](https://learn.microsoft.com/en-us/virtualization/hyper-v-on-windows/about/) *(Windows-only)*


## Containerization

* [Podman](https://podman.io/getting-started/) *(container engine)*
* [Buildah](https://github.com/containers/buildah/tree/main/docs/tutorials) *(build container images)*
* [Docker](https://docs.docker.com/engine/reference/commandline/docker/) *(build container images, container engine)*
* [LXC](https://linuxcontainers.org/lxd/introduction/#get-started) *(Linux-only)*


## Infrastructure as Code

* [OpenTofu](https://opentofu.org/docs/intro/core-workflow) / [Terraform](https://www.terraform.io/intro)
* [Pulumi](https://www.pulumi.com/docs/guides/)
* [Google Cloud CLI](https://cloud.google.com/sdk/docs/install)
* [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html)

## Configuration Management

* [Ansible](https://docs.ansible.com/ansible/latest/user_guide/index.html)
* [cloud-init](https://cloudinit.readthedocs.io/en/latest/) / [ignition](https://github.com/coreos/ignition) *(initial machine setup during first boot)*
* [Chef](https://docs.chef.io/chef_overview/)
* [Puppet](https://puppet.com/docs/puppet/7/what_is_puppet.html)
* [Salt](https://docs.saltproject.io/salt/user-guide/en/latest/topics/overview.html)


## Configuration languages

* [Jsonnet](https://jsonnet.org/learning/tutorial.html)
* [CUE](https://cuelang.org/docs/)
* [OpenTofu Language](https://opentofu.org/docs/language/) / [HCL](https://www.terraform.io/language/syntax/configuration)
* [Dhall](https://dhall-lang.org/)


## Service discovery

* [Zookeeeper](https://zookeeper.apache.org/doc/current/index.html)
* [Consul](https://developer.hashicorp.com/consul/docs/intro)


## CI/CD

* [GitLab](https://docs.gitlab.com/ee/ci/quick_start/#create-a-gitlab-ciyml-file) *(VCS + CI/CD + Project Management)*
* [Concourse](https://concourse-ci.org/config-basics.html)
* [Jenkins](https://www.jenkins.io/doc/book/pipeline/jenkinsfile/)
* [Drone.io](https://docs.drone.io/pipeline/overview/)
* [Argo](https://argoproj.github.io/argo-workflows/workflow-concepts/) *(runs on top of Kubernetes)*
* [Tekton](https://tekton.dev/docs/pipelines/) *(runs on top of Kubernetes)*
* [GitHub Actions](https://docs.github.com/en/actions/using-workflows/about-workflows#create-an-example-workflow) *(SaaS)*
* [CicleCI](https://circleci.com/docs/workflows/#workflows-configuration-examples) *(SaaS)*
* [Dagger](https://dagger.io) *(portable CI/CD pipeline)*


## Container Orchestration

### Distributions

* [Kubernetes](https://kubernetes.io/docs/tutorials/kubernetes-basics/)
* OpenShift: [OKD](https://docs.okd.io/latest/welcome/index.html) *(Community edition)*
  and [OCP](https://developers.redhat.com/products/openshift/overview) *(enterprise edition)*
* [RKE2](https://docs.rke2.io/)


### Try out locally {id="k8s-local"}

* [Minikube](https://kubernetes.io/de/docs/setup/minikube/)
* [MicroK8s](https://microk8s.io/#install-microk8s)
* [kind](https://kind.sigs.k8s.io/)
* [k0s](https://docs.k0sproject.io/stable/install/)
* [Minishift (for OpenShift v3.x)](https://github.com/minishift/minishift)
* [CodeReady Containers (for OpenShift v4.x)](https://www.okd.io/crc/) ([repo](https://github.com/code-ready/crc))
* [Vagrant + Kubespray (Ansible)](https://github.com/kubernetes-sigs/kubespray#vagrant)
* [K3S](https://docs.k3s.io/quick-start) (or [KD3](https://k3d.io/))


### Tooling {id="k8s-tooling"}

* [Kustomize](https://kubernetes.io/docs/tasks/manage-kubernetes-objects/kustomization/)
* [Helm](https://helm.sh/docs/intro/quickstart/)
* Dashboards
  * [Official](https://github.com/kubernetes/dashboard)
  * [Weave Scope](https://github.com/weaveworks/scope)
* [Istio](https://istio.io/latest/about/service-mesh/)
* [K9s](ttps://github.com/derailed/k9s) *(terminal UI for `kubectl`)*
* [Knative](https://knative.dev/) *(serverless (FaaS) on top of Kubernetes)*
* [garden.io](https://docs.garden.io/) *(development and testing)*
* [Rancher v2](https://docs.ranchermanager.rancher.io/getting-started/introduction/overview) *(Kubernetes Cluster Manager)*


## Persistence

* [Ceph](https://docs.ceph.com/en/quincy/) *(versatile persistence technology supporting File-, Object- & Block-Level Storage)*
* [Rook](https://rook.io/docs/rook/latest/Getting-Started/intro/) *(Kubernetes Storage Provider & Operator for Ceph)*

### Message Queue (MQ)

* [Kafka](https://kafka.apache.org/documentation/#introduction)
* [RabbitMQ](https://www.rabbitmq.com/download.html)

### Key-Value stores

* [Redis](https://redis.io/docs/getting-started/)
* [etcd](https://etcd.io/docs/latest/quickstart/)


## Monitoring

### Metrics & Alerting

* [Prometheus](https://prometheus.io/docs/introduction/first_steps/) *(time-series database)*
* [Alertmanager](https://prometheus.io/docs/alerting/latest/alertmanager) *(uses data stored in Prometheus)*
* [Grafana](https://grafana.com/docs/grafana/latest/getting-started/build-first-dashboard/) *(visualize metrics)*
* [Checkmk (CRE)](https://docs.checkmk.com/latest/en/intro_setup.html) *(aggregate, store, visualize & notify)*
* [Netdata](https://learn.netdata.cloud/guides/step-by-step/step-00) *(aggregate, store, visualize & notify)*
* [Graphite](https://graphite.readthedocs.io/en/latest/overview.html) *(time-series database, visualize metrics)*

### Observability

* [OpenTelemetry](https://opentelemetry.io/ecosystem/vendors/) *(a spec combining metrics, logs, and traces)*


#### Clients

* [Node exporter](https://github.com/prometheus/node_exporter)
* [Telegraf](https://github.com/influxdata/telegraf)


### Logging & tracing

* [Elasticsearch (OSS)](https://www.elastic.co/guide/en/elasticsearch/reference/current/install-elasticsearch.html#elasticsearch-deployment-options) *(storage & indexing; part of ELK or EFK stack)*
* [Kibana](https://www.elastic.co/guide/en/kibana/current/get-started.html) *(UI & Log querying; part of ELK or EFK stack)*
* [Loki](https://grafana.com/oss/loki/) *(storage & indexing)*
* [Graylog](https://www.graylog.org/products/open-source) *(aggregate, store & access)*
* [Skywalking]() *(application performance monitor: tracing, metrics and logging)*
* [Jaeger](https://www.jaegertracing.io/docs/latest/getting-started/) *(distributed tracing system)*


#### Clients

* [Fluentd](https://docs.fluentd.org/quickstart)
* [Promtail](https://grafana.com/docs/loki/latest/clients/promtail/)
* [Logstash](https://github.com/elastic/logstash)
* [systemd-netlogd](https://github.com/systemd/systemd-netlogd)
* [rsyslog](https://www.rsyslog.com/)
