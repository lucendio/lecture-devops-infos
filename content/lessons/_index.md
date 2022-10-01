---
bookFlatSection: true
bookCollapseSection: true

title: 'Lessons'
weight: 1
---


Course plan
===========


The following topics are touched throughout the course. Depending on the circumstances, like the
length of the semester or student feedback, the actual agenda may vary. This list should be
understood more as a list of possibilities/choices rather than a TODO or check list.
 
{{< hint >}}
*__NOTE:__ the order of topics below does not necessarily reflect the order in which they are
being discussed*
{{< /hint >}}
 

| Topic                                 | Objectives                       | Technologies           | Tutorial         |
|---------------------------------------|----------------------------------|------------------------|------------------|
| What is DevOps and what is it not?    | DevOps culture, Agile, Site Reliability Engineering  | *N/A*  | *N/A*        | 
| DevOps principles                     | [Configuration, Infrastructure]-as-Code, Automation, Dependency Management, Software Lifecycle  | *N/A*  | *N/A*  |
| Deployment Strategies                 | Resilience, Zero-downtime deployment, Service Discovery, Load balancing  | Nginx, consul, DNS  | Deploy multiple instances of an app behind a load balancer  |
| Twelve-Factor App                     | Understand the 12 factors and how to satisfy them    | *N/A*  | Assess 12 factor compliance of an example app  |
| Virtualization vs. Containerization   | Differences - similar patterns - usage, hypervisors, container runtimes, build tools  | Docker, Podman, Buildah, VirtualBox, QEMU, Packer  | Build a container image and start a container  |
| Dev-Security-Ops                      | account types, secret & permission management, Identity Provider, 2-Factor-Auth  | Sops, Vault, SSH, `.gitignore`  | *N/A*  | 
| Infrastructure Allocation             | What classifies as Infrastructure?, [I,P]aaS, apply Infra-as-Code  | Terraform, Pulumi, Ansible  | Allocate and access a virtual machine in the cloud a/o locally |
| Configuration Management              | Apply Config-as-Code, giving a machine its purpose  | Ansible  | Install a webserver on a virtual machine  |
| Continuous Automation                 | CI/CD, Triggers, GitOps          | Jenkins, Gitlab, Github Actions  | Set up and run a pipeline  |
| Container Orchestration               | Purpose, implementations, market, how it works  | Kubernetes, Helm  | Deploy an application on K8s and make it available from the outside  |
| Persistence & Backups                 | Backup strategies, distributed storage, storage types  | AWS s3, NFS, Ceph  | Encrypt and store a backup offsite  |
| Monitoring & Observability            | Logging, Metrics, Alerts, Tracing, System health, Exporter vs. Collector, push vs. pull  | Prometheus, Grafana, Alertmanager, EFK (Elasticsearch, Fluentd, Kibana)  |
