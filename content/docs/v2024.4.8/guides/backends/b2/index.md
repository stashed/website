---
title: Backblaze B2 Backend | Stash
description: Configure Stash to use Backblaze B2 as Backend.
menu:
  docs_v2024.4.8:
    identifier: backend-b2
    name: Backblaze B2
    parent: backend
    weight: 70
product_name: stash
menu_name: docs_v2024.4.8
section_menu_id: guides
info:
  cli: v0.34.0
  community: v0.34.0
  elasticsearch:
  - 5.6.4-v31
  - 6.2.4-v31
  - 6.3.0-v31
  - 6.4.0-v31
  - 6.5.3-v31
  - 6.8.0-v31
  - 7.14.0-v17
  - 7.2.0-v31
  - 7.3.2-v31
  - 8.2.0-v14
  enterprise: v0.34.0
  etcd:
  - 3.5.0-v18
  installer: v2024.4.8
  kubedump:
  - 0.1.0-v14
  mariadb:
  - 10.5.8-v25
  mongodb:
  - 3.4.17-v31
  - 3.4.22-v31
  - 3.6.13-v31
  - 3.6.8-v31
  - 4.0.11-v31
  - 4.0.3-v31
  - 4.0.5-v31
  - 4.1.13-v31
  - 4.1.4-v31
  - 4.1.7-v31
  - 4.2.3-v31
  - 4.4.6-v22
  - 5.0.15-v4
  - 5.0.3-v19
  - 6.0.5-v7
  mysql:
  - 5.7.25-v31
  - 8.0.14-v31
  - 8.0.21-v25
  - 8.0.3-v31
  nats:
  - 2.6.1-v19
  - 2.8.2-v14
  percona-xtradb:
  - 5.7-v26
  postgres:
  - 10.14-v30
  - 11.9-v30
  - 12.4-v30
  - 13.1-v27
  - 14.0-v19
  - 15.1-v11
  - "16.1"
  - 9.6.19-v30
  redis:
  - 5.0.13-v19
  - 6.2.5-v19
  - 7.0.5-v12
  ui-server: v0.15.0
  vault:
  - 1.10.3-v11
  version: v2024.4.8
---

# Backblaze B2

Stash supports Backblaze's [B2 Cloud Storage](https://www.backblaze.com/b2/cloud-storage.html) as a backend. This tutorial will show you how to use this backend.

In order to use Backblaze B2 Cloud Storage as backend, you have to create a `Secret` and a `Repository` object pointing to the desired B2 bucket.

>If the bucket does not exist yet and the credentials you have provided have the privilege to create bucket, it will be created automatically during the first backup. In this case, you have to make sure that the bucket name is unique across all B2 buckets.

#### Create Storage Secret

To configure storage secret for this backend, following secret keys are needed:

|        Key        |    Type    |                         Description                         |
| ----------------- | ---------- | ----------------------------------------------------------- |
| `RESTIC_PASSWORD` | `Required` | Password that will be used to encrypt the backup snapshots. |
| `B2_ACCOUNT_ID`   | `Required` | Backblaze B2 account id.                                    |
| `B2_ACCOUNT_KEY`  | `Required` | Backblaze B2 account key.                                   |

Create storage secret as below,

```bash
$ echo -n 'changeit' > RESTIC_PASSWORD
$ echo -n '<your-b2-account-id>' > B2_ACCOUNT_ID
$ echo -n '<your-b2-account-key>' > B2_ACCOUNT_KEY
$ kubectl create secret generic -n demo b2-secret \
    --from-file=./RESTIC_PASSWORD \
    --from-file=./B2_ACCOUNT_ID \
    --from-file=./B2_ACCOUNT_KEY
secret/b2-secret created
```

### Create Repository

Now, you have to create a `Repository` crd. You have to provide the storage secret that we have created earlier in `spec.backend.storageSecretName` field.

Following parameters are available for `b2` backend,

|      Parameter      |    Type    |                                                             Description                                                             |
| ------------------- | ---------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| `b2.bucket`         | `Required` | Name of the B2 bucket.                                                                                                              |
| `b2.prefix`         | `Optional` | Path prefix inside the bucket where the backed up data will be stored.                                                              |
| `b2.maxConnections` | `Optional` | Maximum number of parallel connections to use for uploading backup data. By default, Stash will use maximum 5 parallel connections. |

Below, the YAML of a sample `Repository` crd that uses a B2 bucket as a backend.

```yaml
apiVersion: stash.appscode.com/v1alpha1
kind: Repository
metadata:
  name: b2-repo
  namespace: demo
spec:
  backend:
    b2:
      bucket: stash-backup
      prefix: /demo/deployment/my-deploy
    storageSecretName: b2-secret
```

Create the `Repository` we have shown above using the following command,

```bash
$ kubectl apply -f https://github.com/stashed/docs/raw/{{< param "info.version" >}}/docs/guides/backends/b2/examples/b2.yaml
repository/b2-repo created
```

Now, we are ready to use this backend to backup our desired data using Stash.

## Next Steps

- Learn how to use Stash to backup workloads data from [here](/docs/v2024.4.8/guides/workloads/overview/).
- Learn how to use Stash to backup databases from [here](/docs/v2024.4.8/guides/addons/overview/).
- Learn how to use Stash to backup stand-alone PVC from [here](/docs/v2024.4.8/guides/volumes/overview/).
