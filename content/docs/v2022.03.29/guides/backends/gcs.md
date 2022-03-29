---
title: GCS Backend | Stash
description: Configure Stash to use Google Cloud Storage (GCS) as Backend.
menu:
  docs_v2022.03.29:
    identifier: backend-gcs
    name: Google Cloud Storage
    parent: backend
    weight: 50
product_name: stash
menu_name: docs_v2022.03.29
section_menu_id: guides
info:
  cli: v0.19.0
  community: v0.19.0
  elasticsearch:
  - 5.6.4-v16
  - 6.2.4-v16
  - 6.3.0-v16
  - 6.4.0-v16
  - 6.5.3-v16
  - 6.8.0-v16
  - 7.14.0-v2
  - 7.2.0-v16
  - 7.3.2-v16
  enterprise: v0.19.0
  etcd:
  - 3.5.0-v3
  installer: v2022.03.29
  mariadb:
  - 10.5.8-v9
  mongodb:
  - 3.4.17-v15
  - 3.4.22-v15
  - 3.6.13-v15
  - 3.6.8-v15
  - 4.0.11-v15
  - 4.0.3-v15
  - 4.0.5-v15
  - 4.1.13-v15
  - 4.1.4-v15
  - 4.1.7-v15
  - 4.2.3-v15
  - 4.4.6-v6
  - 5.0.3-v3
  mysql:
  - 5.7.25-v16
  - 8.0.14-v16
  - 8.0.21-v10
  - 8.0.3-v16
  nats:
  - 2.6.1-v3
  percona-xtradb:
  - 5.7-v11
  postgres:
  - 10.14-v14
  - 11.9-v14
  - 12.4-v14
  - 13.1-v11
  - 14.0-v3
  - 9.6.19-v14
  redis:
  - 5.0.13-v4
  - 6.2.5-v4
  ui-server: v0.2.0
  version: v2022.03.29
---

# Google Cloud Storage (GCS)

Stash supports [Google Cloud Storage(GCS)](https://cloud.google.com/storage/) as a backend. This tutorial will show you how to use this backend.

In order to use Google Cloud Storage as backend, you have to create a `Secret` and a `Repository` object pointing to the desired GCS bucket.

> If the bucket already exists, the Google Cloud service account you provide to Stash only needs `Storage Object Creator` role permission. However, if the bucket does not exist, Stash  will create the bucket during the first backup. In this case, the Google Cloud service account key used for Stash must have `Storage Object Admin` role permission. To avoid giving this elevated level of permission to Stash, create the bucket manually (either from GCP console or gcloud cli) ahead of time.

#### Create Storage Secret

To configure storage secret for this backend, following secret keys are needed:

|                Key                |    Type    |                         Description                         |
| --------------------------------- | ---------- | ----------------------------------------------------------- |
| `RESTIC_PASSWORD`                 | `Required` | Password that will be used to encrypt the backup snapshots. |
| `GOOGLE_PROJECT_ID`               | `Required` | Google Cloud project ID.                                    |
| `GOOGLE_SERVICE_ACCOUNT_JSON_KEY` | `Required` | Google Cloud service account json key.                      |

Create storage secret as below,

```bash
$ echo -n 'changeit' > RESTIC_PASSWORD
$ echo -n '<your-project-id>' > GOOGLE_PROJECT_ID
$ mv downloaded-sa-json.key GOOGLE_SERVICE_ACCOUNT_JSON_KEY
$ kubectl create secret generic -n demo gcs-secret \
    --from-file=./RESTIC_PASSWORD \
    --from-file=./GOOGLE_PROJECT_ID \
    --from-file=./GOOGLE_SERVICE_ACCOUNT_JSON_KEY
secret/gcs-secret created
```

### Create Repository

Now, you have to create a `Repository` crd. You have to provide the storage secret that we have created earlier in `spec.backend.storageSecretName` field.

Following parameters are available for `gcs` backend.

|      Parameter       |    Type    |                                                                                                                    Description                                                                                                                     |
| -------------------- | ---------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `gcs.bucket`         | `Required` | Name of Bucket. If the bucket does not exist yet, it will be created in the default location (US). It is not possible at the moment for Stash to create a new bucket in a different location, so you need to create it using Google cloud console. |
| `gcs.prefix`         | `Optional` | Path prefix inside the bucket where backed up data will be stored.                                                                                                                                                                                 |
| `gcs.maxConnections` | `Optional` | Maximum number of parallel connections to use for uploading backup data. By default, Stash will use maximum 5 parallel connections.                                                                                                                |

Below, the YAML of a sample `Repository` crd that uses a GCS bucket as a backend.

```yaml
apiVersion: stash.appscode.com/v1alpha1
kind: Repository
metadata:
  name: gcs-repo
  namespace: demo
spec:
  backend:
    gcs:
      bucket: stash-backup
      prefix: /demo/deployment/my-deploy
    storageSecretName: gcs-secret
```

Create the `Repository` we have shown above using the following command,

```bash
$ kubectl apply -f https://github.com/stashed/docs/raw/{{< param "info.version" >}}/docs/examples/guides/backends/gcs.yaml
repository/gcs-repo created
```

Now, we are ready to use this backend to backup our desired data using Stash.

## Next Steps

- Learn how to use Stash to backup workloads data from [here](/docs/v2022.03.29/guides/workloads/overview).
- Learn how to use Stash to backup databases from [here](/docs/v2022.03.29/guides/addons/overview).
- Learn how to use Stash to backup stand-alone PVC from [here](/docs/v2022.03.29/guides/volumes/overview).
