---
title: 'Create encrypted backup'
---


Create an encrypted backup 
==========================


## Objectives

* allocate Object Storage
* create a backup
* encrypt and upload the backup
* restore the backup


## Prerequisites

* credentials for a [supported](https://search.opentofu.org/providers)
  cloud platform
* tool that supports cryptographical operations (symmetric a/o asymmetric en- & decryption)
* additional requirements depending on the approach you choose (e.g. tools, *cloud provider*)


## Tasks

1. Use one of the approaches you learned about (declarative or imperative) to allocate some remote
   Object Storage (e.g. AWS S3 bucket)
2. Create an encrypted off-site backup,
    * compress a folder of your choice (represent the backup artifact)
    * choose a crypto suite
    * encrypt the artifact
    * upload it
3. Restore the backup again
    * download the encrypted artifact from Object Storage
    * decrypt the artifact
    * uncompress the resulting archive
    * verify the data


## Deliverables

* source code
* `terminal.log` file showing the commands and their output used to carry out the tasks


## Solution

*Please note that this solution is based on AWS S3 buckets. Source code can be found
[here](https://github.com/lucendio/lecture-devops-code/tree/master/exercises/create-encrypted-backup).*


### (0) Preparations

Ensure that the tool chain is installed:

* AWS CLI (directly on host or as container image) 
* OpenTofu
* [OpenSSL](https://www.openssl.org/) (alt. [LibreSSL](https://www.libressl.org)) a/o [GnuPG](https://gnupg.org/) (alt. [Sequoia-PGP](https://sequoia-pgp.org/))


### (1) Allocate external Object Storage

#### A - Declarative with the as-Code approach

Use OpenTofu to create an S3 bucket.

{{< hint >}}
Please refer to exercises like [*{{< page-title "./allocate-virtual-machine-in-cloud" >}}*]({{< relref "./allocate-virtual-machine-in-cloud" >}})
for details on how to manage infrastructure resources with OpenTofu. The resource you are looking for is called
`aws_s3_bucket`.
{{< /hint >}}

Verify that the bucket exists by running the following command outside or - as shown below - within a container.
```
aws s3 ls
```

#### B - Interactive via command-line interface in a container

1. Verify credentials

    ```bash
    podman run \
        --mount type=bind,source=${HOME}/.aws,destination=/root/.aws,readonly \
        --interactive \
        --tty \
        --rm \
        public.ecr.aws/aws-cli/aws-cli \
        sts get-caller-identity
    ```

2. Create a bucket

    Start a container:
    ```bash
    podman run \
        --name aws-container \
        --mount type=bind,source=${HOME}/.aws,destination=/root/.aws,readonly \
        --detach \
        --entrypoint sh \
        public.ecr.aws/aws-cli/aws-cli \
        -c 'while true; do sleep 3600; done'
    ```
    
    Allocate a shell in the container:

    __⚡ Context: *host/workstation*__
    ```bash
    podman exec \
        --interactive \
        --tty \
        aws-container \
        /bin/bash
    ```
    
    Actually create the bucket:

    __⚡ Context: *guest/container*__
    ```bash
    aws s3api create-bucket --bucket my-globally-unique-bucket-name
    ```

    {{< hint info >}}
Check afterwards if the bucket actually exists: `s3 ls`. __Don't forget to delete the bucket
again after you are done: `aws s3api delete-bucket --bucket ${BUCKET_NAME}`__
    {{< /hint >}}

    Remove the container:

    __⚡ Context: *host/workstation*__
    ```bash
    podman rm --force aws-container
    ```

{{< hint >}}
More information and examples can be found in the
[official AWS CLI user guide](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-docker.html#cliv2-docker-install).
{{< /hint >}}




### (2) Create a backup, encrypt it and store it remotely

1. Create a backup archive file

    ```bash
    export TEMP_DIR=$(mktemp -d)
    cp ./* ${TEMP_DIR}/
    tar --create --gzip --file ./backup.tar.gz --directory ${TEMP_DIR} .
    rm -r ${TEMP_DIR}
    unset TEMP_DIR
    ```

2. Encrypt the backup file

    {{< hint info >}}
Use `openssl` for symmetric (A) or `gpg` for asymmetric (B) encryption
    {{< /hint >}}

    (A) Encrypt symmetrically
    
    ```bash
    # INFO: will ask you to enter the symmetric key (password)
    openssl enc -aes-256-cbc -salt -in ./backup.tar.gz -out ./backup.tar.gz.enc
    ```
    
    (B) Generate a key pair (in case you don't already have a GPG key-pair) and encrypt asymmetrically
    
    ```bash
    # INFO: will ask you to initially set a passphrase
    gpg --batch --gen-key ./gpg-key.conf
    ```
    ```bash
    export OWN_KEY_FINGERPRINT=$(gpg --with-colons --fingerprint | grep fpr | head -n 1 | awk -F ':' '{print $10}')
    gpg --encrypt --recipient ${OWN_KEY_FINGERPRINT} --output ./backup.tar.gz.enc ./backup.tar.gz
    ```


3. Upload the backup to a bucket

    ```bash
    export BUCKET_NAME=$(tofu output -raw 'advancedBucketName')
    aws s3 cp ./backup.tar.gz.enc s3://${BUCKET_NAME}
    aws s3 ls s3://${BUCKET_NAME}
    rm ./backup.tar.gz.enc
    ```


### (3) Restore data from the backup

1. Choose and download the backup you want to restore

    ```bash
    aws s3 cp s3://${BUCKET_NAME}/backup.tar.gz.enc ./
    ```

2. Decrypt the backup file
   
    {{< hint info >}}
Use `openssl` for symmetric (A) or `gpg` for asymmetric (B) decryption
    {{< /hint >}}
    
    (A) Decrypt symmetrically
    ```bash
    # INFO: will ask you again to enter the symmetric key (password)
    openssl enc -aes-256-cbc -salt -d -in ./backup.tar.gz.enc -out ./backup.tar.gz
    ```

   (B) Decrypt asymmetrically
    
    ```bash
    # INFO: may ask you to enter your passphrase (depending on your cache TTL)
    gpg --decrypt --output ./backup.tar.gz ./backup.tar.gz.enc
    ```

3. Decompress the archive content

    ```bash
    mkdir -p ./backup && tar --extract --gzip --file ./backup.tar.gz --directory ./backup
    ```

4. Verify the result

    ```bash
    ls -al ./backup
    ```

5. Don't forget to clean up at the end

    ```bash
    tofu destroy -auto-approve
    rm -r ./backup.tar.gz.enc ./backup.tar.gz ./backup
    ```
