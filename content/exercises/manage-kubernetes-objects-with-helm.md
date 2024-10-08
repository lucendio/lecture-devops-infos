---
title: 'Manage Kubernetes objects with Helm'
---


Manage Kubernetes objects with Helm
===================================


## Objectives

* write your own Helm chart
* use Helm to deploy and update an application
* encrypt Helm secrets with `sops` 


## Prerequisites

* Exercise: [{{< page-title "./update-version-as-instance-group" >}}]({{< relref "./update-version-as-instance-group" >}})
* Exercise: [{{< page-title "./deploy-app-on-kubernetes" >}}]({{< relref "./deploy-app-on-kubernetes" >}}) 
* [Helm](https://helm.sh/docs/intro/install/) is installed on your workstation
* [SOPS](https://github.com/mozilla/sops#download) is installed on your workstation (and optionally [GPG](https://www.gnupg.org/download/))
* [Helm plugin to integrate SOPS](https://github.com/jkroepke/helm-secrets) is installed on your workstation


## Remarks

* [Chart template guide](https://helm.sh/docs/chart_template_guide/getting_started/) 📖


## Tasks

1. Write a Helm chart from scratch for a service of your choosing that includes
    * meta files such as `Chart.yaml`
    * `./templates` based on static Kubernetes object files
    * `./templates/_helpers.tpl` (functions, available in templates)
    * default `values.yaml`
    * add subchart(s) for backing service(s) if needed
2. From outside the chart, override defaults with another `values.yaml` file 
3. Define possible sensitive values in a `secrets.yaml` and use Sops to encrypt
   these values; redeploy using Helm to verify that decryption during runtime
   works 
4. *[optional]* Enable TLS and ensure database state persists across redeployment


## Deliverables

* source code
* `terminal.log` file showing the commands and their output used to carry out the tasks


## Solution: Edu cluster {id="solution-edu-cluster"}

*Source code can be found
[here](https://github.com/lucendio/lecture-devops-code/tree/master/exercises/manage-kubernetes-objects-with-helm/edu-cluster).*


### (1) Create a chart using an iterative approach   

Create a folder and add each file manually, or run `helm create {{ CHART_NAME }}` to generate a
boilerplate. The latter may require you to remove a couple of files again.

1. Creation
   
    a) Create meta files, such as `Chart.yaml`

    b) Put static Kubernetes object files from previous exercise in `./templates`
      * Deployments
      * Services
      * Ingress

    c) Deploy
      
      Note that if the `context` of your `.kube/config` doesn't define a `namespace`, use the
      `--namespace` option of the command below to define the target namespace for the Helm release.
      ```bash
      helm install {{ RELEASE_NAME }} ./chart
      ```

2. Parameterization

    a) Refactor the object files by moving out certain data points into a `values.yaml`, e.g.:
      * image repository and tag
      * replication factor
      * usernames and passwords
   
      {{< hint info >}}
As a rule of thumb, data points that might be subject to change when deploying different releases
are usually the ones worth getting parameterized 
      {{< /hint >}}

    b) Deploy again
      ```bash
      helm upgrade {{ RELEASE_NAME }} ./chart
      ```

3. Refactoring/Separation

    a) Break down the content of `./chart/values.yaml` into three categories:    
      * defaults (stays inside the chart)
      * parameters (move into a `./values.yaml` outside of the chart)
      * sensitive information (move into a `./secrets.yaml` outside of the chart)

    b) Deploy again
      ```bash
      helm upgrade --install \
          --values=./values.yaml \
          --values=./secrets.yaml \
          --set "image.tag=:scratch" \
          {{ RELEASE_NAME }} ./chart
      ```

4. Add a chart dependency

    a) Add the [Redis](https://github.com/bitnami/charts/tree/master/bitnami/redis) chart
       to the list of `dependencies` in `Chart.yaml` to become a sub-chart, and install it:
      ```bash
      cd ./chart && helm dependency build
      ```

    b) Adjust `./values.yaml`, `./secrets.yaml` and `./chart/values.yaml` by adding
       [sub-chart values](https://github.com/bitnami/charts/tree/main/bitnami/redis#parameters)

    c) Set necessary environment variables in the pod template by using Helm values, e.g. `DB_HOST`

    d) Create a secret of type `Opaque` holding the database password. Either make sure the value
       set for `redis.auth.password` is also used in that secret, or reference the secret name 
       in `redis.auth.existingSecret` and the data key in `redis.auth.existingSecretPasswordKey`
       (refer to the
       [documentation](https://github.com/bitnami/charts/tree/main/bitnami/redis#redis-common-configuration-parameters)
       for more details)

    e) Add a volume list of the *PodSpec* referring to the secret name holding the database password.
       and add an item to the list of `volumeMounts` in order to mount the password as a file into the
       container. Lastly, set the environment variable `DB_PASSWORD` to the respective file path.

      {{< hint info >}}
Note that the keys in the secret's `data` attribute are used as file names within the `mountPath` directory. 
      {{< /hint >}}

    f) Deploy again
      ```bash
      helm upgrade \
          --values=./values.yaml \
          --values=./secrets.yaml \
          {{ RELEASE_NAME }} ./chart
      ```


### (3) Encrypt your secrets

1. Generate a pair of keys (in case you don't already have a GPG key pair)

```bash
gpg --batch --gen-key ./gpg-key.conf
```

2. Install the helm-secrets plugin

```bash
export SKIP_SOPS_INSTALL=true
helm plugin install https://github.com/jkroepke/helm-secrets --version v4.5.1
```

3. Encrypt `secrets.yaml`

```bash
cp secrets.yaml secrets.yaml.dec 
export SOPS_PGP_FP=$(gpg --with-colons --fingerprint Operator | grep fpr | awk -F ':' '{print $10}')
helm secrets encrypt -i secrets.yaml
```

{{< hint info >}}
The option `-i` replaces the plain-text file content in-line instead of printing it to `stdout`. 
{{< /hint >}}

{{< hint info >}}
Instead of using variables like `SOPS_PGP_FP` to configure Sops, you may choose to create a 
[`.sops.yaml`](https://github.com/getsops/sops#212using-sopsyaml-conf-to-select-kms-pgp-and-age-for-new-files)
file at the top-level of you workspace.
{{< /hint >}}


4. Redeploy to see if Helm decrypts the secrets during runtime

```bash
helm secrets \
    upgrade \
    --values=./values.yaml \
    --values=./secrets.yaml \
    {{ RELEASE_NAME }} ./chart
```

5. Cleaning up

```bash
helm uninstall {{ RELEASE_NAME }}
```


### (4) Enable TLS

The feature is covered in the upstream documentation
([TLS](https://gitlab.bht-berlin.de/ris/doku/-/wikis/educluster#ressourcen-service-ingress),
You just have to integrate the respective attributes and objects into you chart(s) and make them
configurable via Helm values if needed.


## Solution: Minikube {id="solution-minikube"}

*Source code can be found
[here](https://github.com/lucendio/lecture-devops-code/tree/master/exercises/manage-kubernetes-objects-with-helm/edu-cluster/minikube).*


### (0) Preparations

Please refer to the [*Preparations* section of the previous exercise]({{< relref "./deploy-app-on-kubernetes#0-prep-minikube" >}}).


### (1) Create a chart using an iterative approach   

Create a folder and add each file manually, or run `helm create {{ CHART_NAME }}` to generate a
boilerplate. The latter may require you to remove a couple of files again.

1. Creation
   
    a) Create meta files, such as `Chart.yaml`

    b) Put static Kubernetes object files from previous exercise in `./templates`
      * Deployments
      * Services
      * Ingress

    c) Deploy
      
      Note that if the `context` of your `.kube/config` doesn't define a `namespace`, use the
      `--namespace` option of the command below to define the target namespace for the Helm release.
      ```bash
      helm install {{ RELEASE_NAME }} ./chart
      ```

2. Parameterization

    a) Refactor the object files by moving out certain data points into a `values.yaml`, e.g.:
      * image repository and tag
      * replication factor
      * usernames and passwords
   
      {{< hint info >}}
As a rule of thumb, data points that might be subject to change when deploying different releases
are usually the ones worth getting parameterized 
      {{< /hint >}}

    b) Deploy again
      ```bash
      helm upgrade {{ RELEASE_NAME }} ./chart
      ```

3. Refactoring/Separation

    a) Break down the content of `./chart/values.yaml` into three categories:    
      * defaults (stays inside the chart)
      * parameters (move into a `./values.yaml` outside of the chart)
      * sensitive information (move into a `./secrets.yaml` outside of the chart)

    b) Deploy again
      ```bash
      helm upgrade --install \
          --values=./values.yaml \
          --values=./secrets.yaml \
          --set "image.tag=:scratch" \
          {{ RELEASE_NAME }} ./chart
      ```

4. Add a chart dependency

    a) Add the [Redis](https://github.com/bitnami/charts/tree/master/bitnami/redis) chart
       to the list of `dependencies` in `Chart.yaml` to become a sub-chart, and install it:
      ```bash
      cd ./chart && helm dependency build
      ```

    b) Adjust `./values.yaml`, `./secrets.yaml` and `./chart/values.yaml` by adding
       [sub-chart values](https://github.com/bitnami/charts/tree/main/bitnami/redis#parameters)

    c) Set necessary environment variables in the pod template by using Helm values, e.g. `DB_HOST`

    d) Create a secret of type `Opaque` holding the database password. Either make sure the value
       set for `redis.auth.password` is also used in that secret, or reference the secret name 
       in `redis.auth.existingSecret` and the data key in `redis.auth.existingSecretPasswordKey`
       (refer to the
       [documentation](https://github.com/bitnami/charts/tree/main/bitnami/redis#redis-common-configuration-parameters)
       for more details)

    e) Add a volume list of the *PodSpec* referring to the secret name holding the database password.
       and add an item to the list of `volumeMounts` in order to mount the password as a file into the
       container. Lastly, set the environment variable `DB_PASSWORD` to the respective file path.

      {{< hint info >}}
Note that the keys in the secret's `data` attribute are used as file names within the `mountPath` directory. 
      {{< /hint >}}

    f) Deploy again
      ```bash
      helm upgrade \
          --values=./values.yaml \
          --values=./secrets.yaml \
          {{ RELEASE_NAME }} ./chart
      ```


### (3) Encrypt your secrets

1. Generate a pair of keys (in case you don't already have a GPG key pair)

```bash
gpg --batch --gen-key ./gpg-key.conf
```

2. Install the helm-secrets plugin

```bash
export SKIP_SOPS_INSTALL=true
helm plugin install https://github.com/jkroepke/helm-secrets --version v4.5.1
```

3. Encrypt `secrets.yaml`

```bash
cp secrets.yaml secrets.yaml.dec 
export SOPS_PGP_FP=$(gpg --with-colons --fingerprint Operator | grep fpr | awk -F ':' '{print $10}')
helm secrets encrypt -i secrets.yaml
```

{{< hint info >}}
The option `-i` replaces the plain-text file content in-line instead of printing it to `stdout`. 
{{< /hint >}}

{{< hint info >}}
Instead of using variables like `SOPS_PGP_FP` to configure Sops, you may choose to create a 
[`.sops.yaml`](https://github.com/getsops/sops#212using-sopsyaml-conf-to-select-kms-pgp-and-age-for-new-files)
file at the top-level of you workspace.
{{< /hint >}}


4. Redeploy to see if Helm decrypts the secrets during runtime

```bash
helm secrets \
    upgrade \
    --values=./values.yaml \
    --values=./secrets.yaml \
    {{ RELEASE_NAME }} ./chart
```

5. Cleaning up

```bash
helm uninstall {{ RELEASE_NAME }}
```


### (4) Enable TLS

1. Generate a self-signed TLS key pair and add it to `./secrets.yaml`

{{< hint warning >}}
Minikube runs locally on your workstation. Which means, DNS is not available out of
the box - if you haven't tinkered with your `/etc/hosts` file, this is. Though, a
valid approach.

Another option is to utilize [nip.io](https://nip.io), which is a DNS server that
responds dynamically based on the FQDN requested to be resolved. Meaning, the IP
you want it to respond for an FQDN, is *"encoded"* in the FQDN itself.   
{{< /hint >}}

```bash
export SUB_DOMAIN=$(minikube ip | tr . -)
openssl req \
    -new -nodes -newkey \
    -x509 -days 365 rsa:4096 -sha512 \
    -subj "/CN=webservice.${ SUB_DOMAIN }.nip.io" \
    -keyout tls.key -out tls.crt
```

2. Add another template to the chart in order to create a `Secret` of type `kubernetes.io/tls` 

3. Set the respective values for TLS private key and certificate in `./secrets.yaml.dec` and
   encrypt the file with Sops again
 
4. Allow to set the FQDN of the `Ingess` object via Helm value and redeploy

```bash
helm secrets \
    upgrade \
    --values=./values.yaml \
    --values=./secrets.yaml \
    --set "fqdn=pad.{{ SUB_DOMAIN }}.nip.io" 
    {{ RELEASE_NAME }} ./chart
```
