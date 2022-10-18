---
title: 'Deploy Kubernetes objects with Helm'
---


Deploy Kubernetes objects with Helm
===================================


## Objectives

* write your own Helm chart
* use Helm to deploy and update an application
* encrypt Helm secrets with `sops` 


## Prerequisites

* a reachable Kubernetes cluster
* working credentials to authenticate against the cluster
* `kubectl` is installed on your workstation to interact with the cluster
* [Helm](https://helm.sh/docs/intro/install/) is installed on your workstation
* [SOPS](https://github.com/mozilla/sops#download) is installed on your workstation (and optionally [GPG](https://www.gnupg.org/download/))
* [Sops/Helm plugin](https://github.com/jkroepke/helm-secrets) is installed 


## Tasks

0. Combine the effort of previous tutorials and create a Helm chart
    * manage and update a containerized application (07)
    * write Kubernetes Objects (08)
1. Write a chart from scratch including
    * meta files such as `Chart.yaml`
    * `./templates` based on static Kubernetes object files
    * `./templates/_helpers.tpl` (functions, available in templates)
    * `values.yaml` and `secrets.yaml`
    * 📖 [The Chart Template Developer's Guide](https://helm.sh/docs/chart_template_guide/)
2. Use Sops to encrypt sensitive values and re-deploy using Helm to verify that the decryption during runtime works 


## Solution

*Please note that this solution makes use of solutions from previous tutorials and
hence will result in a chart for [CodiMD](https://github.com/hackmdio/codimd#documentation).
Source code can be found
[here](https://github.com/lucendio/lecture-devops-code/tree/master/tutorials/09_deploy-workload-with-helm).*


### (0) Preparations

Verify that `kubectl` is configured correctly. The following command should list all the different
Kubernetes objects that are available via *kube-api*.

```bash
kubectl api-resources
```

{{< hint info >}}
Additionally, ensure that the CLIs `helm`, `sops` and `pgp` are available. Either try
your local package manager or just download them from their respective release pages,
make them executable and put them in a directory listed in `$PATH`.
{{< /hint >}}


### (1) Create a chart using an iterative method   

Create a folder and add each file manually, or run `helm create {{ CHART_NAME }}` to generate a
boilerplate. The latter may require you to remove a couple of files again.

1. Creation
   
    a) Create meta files, such as `Chart.yaml`

    b) Put static Kubernetes object files in `./templates`
      * Deployments
      * Services
      * Ingress

    c) Deploy
      ```bash
      helm install {{ RELEASE_NAME }} ./chart
      ```

2. Parameterization

    a) Refactor the object files by moving out certain data into a `values.yaml`, e.g.:
       * image repository and tag
       * replication factor
       * usernames and passwords

    b) Deploy again
      ```bash
      helm upgrade {{ RELEASE_NAME }} ./chart
      ```

3. Refactoring/Separation

    a) Break down the content of `./chart/values.yaml` into three categories:    
       * defaults (stay)
       * parameters (move into a `./values.yaml` outside of the chart)
       * sensitive information (move into a `./secrets.yaml` outside of the chart)

    b) Generate a self-signed TLS key pair and add it to `./secrets.yaml`
      ```bash
      export SUB_DOMAIN=$(minikube ip | tr . -)
      openssl req -new -nodes -x509 -days 365 -newkey rsa:4096 -sha512 -keyout tls.key -out tls.crt -subj "/CN=pad.{{ SUB_DOMAIN }}.nip.io"
      ```
    c) Deploy again
      ```bash
      helm upgrade --install \
          --values=./values.yaml \
          --values=./secrets.yaml \
          --set "image.tag=2.3.2" --set "fqdn=pad.{{ SUB_DOMAIN }}.nip.io" \
          {{ RELEASE_NAME }} ./chart
      ```
   
    {{< hint info >}}
Up to this point, the Pods resulting from the Deployment should end up in a CrashLoopBackoff.
Can you find out why?
    {{< /hint >}}

4. Add chart dependencies

    a) [PostgreSQL](https://github.com/bitnami/charts/tree/master/bitnami/postgresql)
      ```bash
      helm repo add bitnami https://charts.bitnami.com/bitnami
      cd ./chart && helm dependency build
      ```

    b) Adjust `./values.yaml`, `./secrets.yaml` and `./chart/values.yaml` by adding [sub-chart values](https://github.com/bitnami/charts/tree/master/bitnami/postgresql/README>md)

    c) Deploy again
      ```bash
      helm upgrade --values=./values.yaml --values=./secrets.yaml --set "fqdn=pad.{{ SUB_DOMAIN }}.nip.io" {{ RELEASE_NAME }} ./chart
      ```

    {{< hint info >}}
See the `./chart` folder in the current directory or find some inspiration in the
[official chart of [`codimd`](https://github.com/hackmdio/codimd-helm/tree/master/charts/codimd)]
    {{< /hint >}}


### (2) Encrypt your secrets

1. Generate a key pair (in case you don't already have a GPG key-pair)

```bash
# INFO: will ask you to initially set a passphrase
gpg --batch --gen-key ./gpg-key.conf
```

2. Install the Helm-Sops plugin

```bash
SKIP_SOPS_INSTALL=true helm plugin install https://github.com/jkroepke/helm-secrets --version v3.4.0
```

3. Encrypt `secrets.yaml`

```bash
cp secrets.yaml secrets-backup.yaml 
export SOPS_PGP_FP=$(gpg --with-colons --fingerprint Operator | grep fpr | awk -F ':' '{print $10}')
helm secrets dec secrets.yaml
```

4. Redeploy to see if Helm decrypts the secrets during runtime

```bash
helm secrets \
    upgrade \
    --values=./values.yaml \
    --values=./secrets.yaml \
    --set "fqdn=pad.{{ SUB_DOMAIN }}.nip.io" 
    {{ RELEASE_NAME }} ./chart
```
