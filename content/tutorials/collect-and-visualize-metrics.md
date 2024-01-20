---
title: 'Collect, store, and visualize metrics'
---


Expose, collect, store, and visualize metrics
=============================================


## Objectives

* find something to observe
* set up an Observability stack
* expose, collect, and visualize data


## Prerequisites

* Tutorial: [{{< page-title "./spin-up-virtual-machine-locally" >}}]({{< relref "./spin-up-virtual-machine-locally" >}})
  or [{{< page-title "./allocate-machine-in-cloud" >}}]({{< relref "./allocate-machine-in-cloud" >}})
* Tutorial: [{{< page-title "./automate-system-configuration" >}}]({{< relref "./automate-system-configuration" >}})
  or [{{< page-title "./deploy-workload-with-helm" >}}]({{< relref "./deploy-workload-with-helm" >}})


## Remarks

* Prometheus: [Ansible collection](https://github.com/prometheus-community/ansible), [Helm charts](https://github.com/prometheus-community/helm-charts)
* Grafana: [Ansible collection](https://github.com/grafana/grafana-ansible-collection), [Helm charts](https://github.com/grafana/helm-charts)


## Tasks

1. Decide on something worth observing (e.g. [node exporter](https://prometheus.io/docs/guides/node-exporter/), 
   [webservice](https://gitlab.bht-berlin.de/fb6-wp11-devops/webservice/-/blob/cfc9395fc82f552e470682eb88ced6b45c5ed5e6/routing/routes.go#L100-L127))
   and ensure data is being exposed
2. Choose a platform on which the necessary components of the observability are going to be deployed
3. Install and configure the components that collect, store and visualize the data
4. Assemble a dashboard or reuse an existing one
5. Cause the metrics to change and verify that the dashboard actually indicates the changes   


## Deliverables

* source code
* conclusive screenshot of the dashboard
* `terminal.log` file showing the commands and their output used to carry out the tasks


## Solution: Ansible {id="solution-ansible"}

*This approach utilizes a single local machine as platform and the system itself as subject for observation.
Source code can be found
[here](https://github.com/lucendio/lecture-devops-code/tree/master/tutorials/collect-and-visualize-metrics).*


### (0 + 1) Preparations

Allocate one or more machines - in the cloud or locally - according to previous tutorials. Make sure, they are
accessible via SSH from an Ansible control node.

For this solution, data about the system itself shall be collected. Luckily, the community already developed the
[*node exporter*](https://github.com/prometheus/node_exporter) for such occasions. To install the exporter via
Ansible, use the respective [role](https://prometheus-community.github.io/ansible/branch/main/node_exporter_role.html)
from the `prometheus.prometheus` collection, which is also maintained by the community.

Ansible dependencies can be installed from command-line. But, as it is best practice to define dependencies in
source code, the respective manifest file 
[`./requirements.yaml`](https://docs.ansible.com/ansible/latest/collections_guide/collections_installing.html#install-multiple-collections-with-a-requirements-file)
shall be used moving forward.
To install dependencies listed in said file, run

```bash
ansible-galaxy collection install \
    --requirements-filer ./requirements.yaml \
    --collections-path ./.local/collections
``` 

{{< hint warning >}}
The collection path option is defined explicitly to distinguish between project-level collections and upstream
collections. This requires  `collections_path` to be set to `./collections:./.local/collections` in
`./ansible.cfg`.
{{< /hint >}}

Define the group called `emitters` in the inventory and assign the necessary variables. By referring to the role
`prometheus.prometheus.node_exporter` in a play and applying the playbook, the *node exporter* gets installed on
the hosts that are part of the `emitters` group.

```bash
ansible-playbook \
    --inventory ./inventory \
    ./playbook.yaml
```

{{< hint warning >}}
If the role can't be found, something might be wrong with the paths Ansible uses to discover collections and
roles. Run `ansible-playbook --version` to display the list of paths in which Ansible looks for them.
{{< /hint >}}


### (2) Choose a platform

Collector, storage and visualization components are also installed via Ansible. Effectively, this is done
with the help of the system package manager (no containers just binaries, configuration files and system
process manager).

Due to limited local resources, the entire stack is installed into the very same machine that is also
supposed to be observed. Which means, the following steps are similar compared to setting up the
*node exporter*. 


### (3) Install the observability stack

Create another group called `collectors` and define the variables needed to collect data from the node
exporter (see `scrape_configs`). Apply the `prometheus` role - which is part of the same collection
as the *node exporter* role - by referring to it in a new play. Then, run the playbook again.

{{< hint info >}}
Here, collector and database are actually one and the same. Prometheus does not only collect data, it's
also a time-series database.
{{< /hint >}}

Do the same for *Grafana* - the software that visualizes the emitted and collected data. Define a new group
called `visualizers`. Then, install Grafana's Ansible collection `grafana.grafana` as second dependency.  

Write yet another play for the `visualizers` that refers to the
[`grafana`](https://github.com/grafana/grafana-ansible-collection/tree/main/roles/grafana) role and tells
Grafana where to find the data (see `datasources`, aka. how to connect to Prometheus) by setting respective
role variables. 

Eventually, when looking at all three plays in the playbook, the collector (Prometheus) should refer to
the emitter (node exporter), and the visualizer should refer to the collector. Meaning, each pair should
share some variable values (e.g. `node_exporter_web_telemetry_path` and 
`prometheus_scrape_configs[*].static_configs[*].targets`)


### (4) Assemble a dashboard

[Create a Grafana dashboard](https://grafana.com/docs/grafana/latest/dashboards/build-dashboards/create-dashboard/)
on your own or [reuse one of the many existing ones](https://grafana.com/grafana/dashboards/?search=node+exporter).

For this solution we are going for the latter. Just choose one and copy the ID (right bottom). Add a new item
to the list of `grafana_dashboards` and define attributes for the ID and the data source name. Finally, re-apply
the Ansible playbook.

Open up Grafana's web console, log in if you defined credentials in the `visualizer` play, and navigate to
*Home > Dashboards > ${DASHBOARD_NAME}*

{{< hint info >}}
To make the dashboard showing up right after login

1. Mark it as favorite (top left, the ⭐ button)
2. Set it as *Home Dashboard* (top right, *Profile > Preferences*)
{{< /hint >}}


### (5) Cause metrics to change

Open up an SSH connection to the machine that runs the *node exporter* and increase CPU utilization

```bash
sha1sum /dev/zero
```

The corresponding graph(s) should indicate a significant increase of CPU utilization.

{{< hint info >}}
If changes don't appear,

1. make the time frame smaller (top right)
2. set the refresh rate to a more frequent one
{{< /hint >}}


### (6) Cleaning up

Decommission al machines as described in previous tutorials.


## Solution: Helm {id="solution-helm"}

TODO




