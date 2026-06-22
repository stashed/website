---
title: Azure Backend | Stash
description: Configure Stash to use Microsoft Azure Storage as Backend.
menu:
  docs_v2025.2.10:
    identifier: backend-azure
    name: Azure Blob Storage
    parent: backend
    weight: 40
product_name: stash
menu_name: docs_v2025.2.10
section_menu_id: guides
info:
  cli: v0.39.0
  community: v0.39.0
  elasticsearch:
  - 5.6.4-v35
  - 6.2.4-v35
  - 6.3.0-v35
  - 6.4.0-v35
  - 6.5.3-v35
  - 6.8.0-v35
  - 7.14.0-v21
  - 7.2.0-v35
  - 7.3.2-v35
  - 8.2.0-v18
  enterprise: v0.39.0
  etcd:
  - 3.5.0-v22
  installer: v2025.2.10
  kubedump:
  - 0.2.0-v3
  mariadb:
  - 10.5.8-v29
  mongodb:
  - 3.4.17-v36
  - 3.4.22-v36
  - 3.6.13-v36
  - 3.6.8-v36
  - 4.0.11-v36
  - 4.0.3-v36
  - 4.0.5-v36
  - 4.1.13-v36
  - 4.1.4-v36
  - 4.1.7-v36
  - 4.2.3-v36
  - 4.4.6-v27
  - 5.0.15-v9
  - 5.0.3-v24
  - 6.0.5-v12
  mysql:
  - 5.7.25-v36
  - 8.0.14-v35
  - 8.0.21-v29
  - 8.0.3-v35
  nats:
  - 2.6.1-v23
  - 2.8.2-v18
  percona-xtradb:
  - 5.7-v30
  postgres:
  - 10.14-v34
  - 11.9-v34
  - 12.4-v34
  - 13.1-v31
  - 14.0-v23
  - 15.1-v15
  - 16.1-v4
  - 17.2-v2
  - 9.6.19-v34
  redis:
  - 5.0.13-v23
  - 6.2.5-v23
  - 7.0.5-v16
  ui-server: v0.20.0
  vault:
  - 1.10.3-v15
  version: v2025.2.10
---

# Microsoft Azure Storage

Stash supports Microsoft's [Azure Blob Storage](https://azure.microsoft.com/en-us/services/storage/blobs/) as a backend. This tutorial will show you how to use this backend.

In order to use Azure Blob Storage as backend, you have to create a `Secret` and a `Repository` object pointing to the desired blob container.

#### Create Storage Secret

To configure storage secret for this backend, following secret keys are needed:

|         Key          |    Type    |                        Description                         |
| -------------------- | ---------- | ---------------------------------------------------------- |
| `RESTIC_PASSWORD`    | `Required` | Password that will be used to encrypt the backup snapshots |
| `AZURE_ACCOUNT_NAME` | `Required` | Azure Storage account name                                 |
| `AZURE_ACCOUNT_KEY`  | `Required` | Azure Storage account key                                  |

Create storage secret as below,

```bash
$ echo -n 'changeit' > RESTIC_PASSWORD
$ echo -n '<your-azure-storage-account-name>' > AZURE_ACCOUNT_NAME
$ echo -n '<your-azure-storage-account-key>' > AZURE_ACCOUNT_KEY
$ kubectl create secret generic -n demo azure-secret \
    --from-file=./RESTIC_PASSWORD \
    --from-file=./AZURE_ACCOUNT_NAME \
    --from-file=./AZURE_ACCOUNT_KEY
secret/azure-secret created
```

### Create Repository

Now, you have to create a `Repository` crd. You have to provide the storage secret that we have created earlier in `spec.backend.storageSecretName` field.

Following parameters are available for `azure` backend.

|       Parameter        |    Type    |                                                             Description                                                             |
| ---------------------- | ---------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| `azure.container`      | `Required` | Name of Storage container.                                                                                                          |
| `azure.prefix`         | `Optional` | Path prefix inside the container where backed up data will be stored.                                                               |
| `azure.maxConnections` | `Optional` | Maximum number of parallel connections to use for uploading backup data. By default, Stash will use maximum 5 parallel connections. |

Below, the YAML of a sample `Repository` crd that uses an Azure Blob container as a backend.

```yaml
apiVersion: stash.appscode.com/v1alpha1
kind: Repository
metadata:
  name: azure-repo
  namespace: demo
spec:
  backend:
    azure:
      container: stash-backup
      prefix: /demo/deployment/my-deploy
    storageSecretName: azure-secret
```

Create the `Repository` we have shown above using the following command,

```bash
$ kubectl apply -f https://github.com/stashed/docs/raw/{{< param "info.version" >}}/docs/guides/backends/azure/examples/azure.yaml
repository/azure-repo created
```

Now, we are ready to use this backend to backup our desired data using Stash.

## Next Steps

- Learn how to use Stash to backup workloads data from [here](/docs/v2025.2.10/guides/workloads/overview/).
- Learn how to use Stash to backup databases from [here](/docs/v2025.2.10/guides/addons/overview/).
- Learn how to use Stash to backup stand-alone PVC from [here](/docs/v2025.2.10/guides/volumes/overview/).
