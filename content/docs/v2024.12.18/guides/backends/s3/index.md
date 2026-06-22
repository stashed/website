---
title: AWS S3/Minio/Rook Backend | Stash
description: Configure Stash to use AWS S3/Minio/Rook as Backend.
menu:
  docs_v2024.12.18:
    identifier: backend-s3
    name: AWS S3/Minio/Rook
    parent: backend
    weight: 30
product_name: stash
menu_name: docs_v2024.12.18
section_menu_id: guides
info:
  cli: v0.37.0
  community: v0.37.0
  elasticsearch:
  - 5.6.4-v33
  - 6.2.4-v33
  - 6.3.0-v33
  - 6.4.0-v33
  - 6.5.3-v33
  - 6.8.0-v33
  - 7.14.0-v19
  - 7.2.0-v33
  - 7.3.2-v33
  - 8.2.0-v16
  enterprise: v0.37.0
  etcd:
  - 3.5.0-v20
  installer: v2024.12.18
  kubedump:
  - 0.1.0-v16
  mariadb:
  - 10.5.8-v27
  mongodb:
  - 3.4.17-v34
  - 3.4.22-v34
  - 3.6.13-v34
  - 3.6.8-v34
  - 4.0.11-v34
  - 4.0.3-v34
  - 4.0.5-v34
  - 4.1.13-v34
  - 4.1.4-v34
  - 4.1.7-v34
  - 4.2.3-v34
  - 4.4.6-v25
  - 5.0.15-v7
  - 5.0.3-v22
  - 6.0.5-v10
  mysql:
  - 5.7.25-v34
  - 8.0.14-v33
  - 8.0.21-v27
  - 8.0.3-v33
  nats:
  - 2.6.1-v21
  - 2.8.2-v16
  percona-xtradb:
  - 5.7-v28
  postgres:
  - 10.14-v32
  - 11.9-v32
  - 12.4-v32
  - 13.1-v29
  - 14.0-v21
  - 15.1-v13
  - 16.1-v2
  - "17.2"
  - 9.6.19-v32
  redis:
  - 5.0.13-v21
  - 6.2.5-v21
  - 7.0.5-v14
  ui-server: v0.18.0
  vault:
  - 1.10.3-v13
  version: v2024.12.18
---

# AWS S3

Stash supports AWS S3 or S3 compatible storage services like [Minio](https://minio.io/) servers, [Rook Object Store](https://rook.io/docs/rook/v0.9/ceph-object.html), [DigitalOceans Space](https://www.digitalocean.com/products/spaces/) as a backend. This tutorial will show you how to use this backend.

In order to use S3 or S3 compatible storage service as backend, you have to create a `Secret` and a `Repository` object pointing to the desired bucket.

>If the bucket does not exist yet, Stash will create it automatically in the default region (`us-east-1`) during the first backup. In this case, you have to make sure that the bucket name is unique across all S3 buckets. Currently, it is not possible for Stash to create bucket in different region. You have to create the bucket in your desired region before using it in Stash.

#### Create Storage Secret

To configure storage secret for this backend, following secret keys are needed:

| Key                     | Type       | Description                                                                                                                                                            |
| ----------------------- | ---------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `RESTIC_PASSWORD`       | `Required` | Password that will be used to encrypt the backup snapshots.                                                                                                            |
| `AWS_ACCESS_KEY_ID`     | `Required` | AWS / Minio / Rook / DigitalOcean Spaces access key ID                                                                                                                 |
| `AWS_SECRET_ACCESS_KEY` | `Required` | AWS / Minio / Rook / DigitalOcean Spaces secret access key                                                                                                             |
| `CA_CERT_DATA`          | `optional` | CA certificate used by storage backend. This can be used to pass the root certificate that has been used to sign the server certificate of a TLS secured Minio server. |

Create storage secret as below,

```bash
$ echo -n 'changeit' > RESTIC_PASSWORD
$ echo -n '<your-aws-access-key-id-here>' > AWS_ACCESS_KEY_ID
$ echo -n '<your-aws-secret-access-key-here>' > AWS_SECRET_ACCESS_KEY
$ kubectl create secret generic -n demo s3-secret \
    --from-file=./RESTIC_PASSWORD \
    --from-file=./AWS_ACCESS_KEY_ID \
    --from-file=./AWS_SECRET_ACCESS_KEY
secret/s3-secret created
```

For TLS secured Minio Server, create secret as below,

```bash
$ echo -n 'changeit' > RESTIC_PASSWORD
$ echo -n '<your-minio-access-key-id-here>' > AWS_ACCESS_KEY_ID
$ echo -n '<your-minio-secret-access-key-here>' > AWS_SECRET_ACCESS_KEY
$ cat ./directory/of/root/certificate/ca.crt > CA_CERT_DATA
$ kubectl create secret generic -n demo minio-secret \
    --from-file=./RESTIC_PASSWORD \
    --from-file=./AWS_ACCESS_KEY_ID \
    --from-file=./AWS_SECRET_ACCESS_KEY \
    --from-file=./CA_CERT_DATA
secret/minio-secret created
```

{{< notice type="warning" message="If you are using a Minio backend, make sure that you are using `AWS_ACCESS_KEY_ID` instead of `MINIO_ACCESS_KEY` and `AWS_SECRET_ACCESS_KEY` instead of `MINIO_SECRET_KEY`." >}}

### Create Repository

Now, you have to create a `Repository` crd. You have to provide the storage secret that we have created earlier in `spec.backend.storageSecretName` field.

Following parameters are available for `S3` backend.

| Parameter        | Type       | Description                                                                                                                                                                                                                                                                                                                                                                              |
|------------------| ---------- |------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `s3.endpoint`    | `Required` | For S3, use `s3.amazonaws.com`. If your bucket is in a different location, S3 server (s3.amazonaws.com) will redirect Stash to the correct endpoint. For DigitalOCean, use `nyc3.digitaloceanspaces.com` etc. depending on your bucket region. For S3-compatible other storage services like Minio / Rook use URL of the server.                                                         |
| `s3.bucket`      | `Required` | Name of Bucket. If the bucket does not exist yet it will be created in the default location (`us-east-1` for S3). It is not possible at the moment for Stash to create a new bucket in a different location, so you need to create it using a different program.                                                                                                                         |
| `s3.region`      | `Optional` | Specify the region of your bucket.                                                                                                                                                                                                                                                                                                                                                       |
| `s3.prefix`      | `Optional` | Path prefix inside the bucket where the backed up data will be stored.                                                                                                                                                                                                                                                                                                                   |
| `s3.insecureTLS` | `Optional` | Specify whether to skip TLS certificate verification. Setting this field to `true` disables verification, which might be necessary in cases where the server uses self-signed certificates or certificates from an untrusted CA. Use this option with caution, as it can expose the client to man-in-the-middle attacks and other security risks. Only use it when absolutely necessary. |

Below, the YAML of a sample `Repository` crd that uses an `S3` bucket as a backend.

```yaml
apiVersion: stash.appscode.com/v1alpha1
kind: Repository
metadata:
  name: s3-repo
  namespace: demo
spec:
  backend:
    s3:
      endpoint: s3.amazonaws.com # use server URL for s3 compatible other storage service
      bucket: stash-demo
      region: us-west-1
      prefix: /backup/demo/deployment/stash-demo
    storageSecretName: s3-secret
```

For S3 compatible Minio and other storage services, specify the endpoint with connection scheme (`http`, or `https`),

```yaml
apiVersion: stash.appscode.com/v1alpha1
kind: Repository
metadata:
  name: minio-repo
  namespace: demo
spec:
  backend:
    s3:
      endpoint: https://my-minio-service.minio-namespace.svc
      bucket: stash-demo
      prefix: /backup/demo/deployment/stash-demo
    storageSecretName: s3-secret
```

Create the `s3-repo` Repository we have shown above using the following command,

```bash
$ kubectl apply -f https://github.com/stashed/docs/raw/{{< param "info.version" >}}/docs/guides/backends/s3/examples/s3.yaml
repository/s3-repo created
```

Now, we are ready to use this backend to backup our desired data using Stash.

## Next Steps

- Learn how to use Stash to backup workloads data from [here](/docs/v2024.12.18/guides/workloads/overview/).
- Learn how to use Stash to backup databases from [here](/docs/v2024.12.18/guides/addons/overview/).
- Learn how to use Stash to backup stand-alone PVC from [here](/docs/v2024.12.18/guides/volumes/overview/).
