---
title: Backblaze B2 Backend | Stash
description: Configure Stash to use Backblaze B2 as Backend.
menu:
  docs_v2024.2.9-rc.0:
    identifier: backend-b2
    name: Backblaze B2
    parent: backend
    weight: 70
product_name: stash
menu_name: docs_v2024.2.9-rc.0
section_menu_id: guides
info:
  cli: v0.33.0-rc.0
  community: v0.33.0-rc.0
  elasticsearch:
  - 5.6.4-v29
  - 6.2.4-v29
  - 6.3.0-v29
  - 6.4.0-v29
  - 6.5.3-v29
  - 6.8.0-v29
  - 7.14.0-v15
  - 7.2.0-v29
  - 7.3.2-v29
  - 8.2.0-v12
  enterprise: v0.33.0-rc.0
  etcd:
  - 3.5.0-v16
  installer: v2024.2.9-rc.0
  kubedump:
  - 0.1.0-v12
  mariadb:
  - 10.5.8-v23
  mongodb:
  - 3.4.17-v29
  - 3.4.22-v29
  - 3.6.13-v29
  - 3.6.8-v29
  - 4.0.11-v29
  - 4.0.3-v29
  - 4.0.5-v29
  - 4.1.13-v29
  - 4.1.4-v29
  - 4.1.7-v29
  - 4.2.3-v29
  - 4.4.6-v20
  - 5.0.15-v2
  - 5.0.3-v17
  - 6.0.5-v5
  mysql:
  - 5.7.25-v29
  - 8.0.14-v29
  - 8.0.21-v23
  - 8.0.3-v29
  nats:
  - 2.6.1-v17
  - 2.8.2-v12
  percona-xtradb:
  - 5.7-v24
  postgres:
  - 10.14-v28
  - 11.9-v28
  - 12.4-v28
  - 13.1-v25
  - 14.0-v17
  - 15.1-v9
  - 9.6.19-v28
  redis:
  - 5.0.13-v17
  - 6.2.5-v17
  - 7.0.5-v10
  ui-server: v0.14.0-rc.0
  vault:
  - 1.10.3-v9
  version: v2024.2.9-rc.0
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

- Learn how to use Stash to backup workloads data from [here](/docs/v2024.2.9-rc.0/guides/workloads/overview/).
- Learn how to use Stash to backup databases from [here](/docs/v2024.2.9-rc.0/guides/addons/overview/).
- Learn how to use Stash to backup stand-alone PVC from [here](/docs/v2024.2.9-rc.0/guides/volumes/overview/).
