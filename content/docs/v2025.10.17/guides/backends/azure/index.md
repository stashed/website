---
title: Azure Backend | Stash
description: Configure Stash to use Microsoft Azure Storage as Backend.
menu:
  docs_v2025.10.17:
    identifier: backend-azure
    name: Azure Blob Storage
    parent: backend
    weight: 40
product_name: stash
menu_name: docs_v2025.10.17
section_menu_id: guides
info:
  cli: v0.42.0
  community: v0.42.0
  elasticsearch:
  - 5.6.4-v38
  - 6.2.4-v38
  - 6.3.0-v38
  - 6.4.0-v38
  - 6.5.3-v38
  - 6.8.0-v38
  - 7.14.0-v24
  - 7.2.0-v38
  - 7.3.2-v38
  - 8.2.0-v21
  enterprise: v0.42.0
  etcd:
  - 3.5.0-v25
  installer: v2025.10.17
  kubedump:
  - 0.2.0-v6
  mariadb:
  - 10.6.23-v1
  mongodb:
  - 3.4.17-v39
  - 3.4.22-v39
  - 3.6.13-v39
  - 3.6.8-v39
  - 4.0.11-v39
  - 4.0.3-v39
  - 4.0.5-v39
  - 4.1.13-v39
  - 4.1.4-v39
  - 4.1.7-v39
  - 4.2.3-v39
  - 4.4.6-v30
  - 5.0.15-v12
  - 5.0.3-v27
  - 6.0.5-v15
  mysql:
  - 5.7.25-v39
  - 8.0.14-v38
  - 8.0.21-v32
  - 8.0.3-v38
  nats:
  - 2.6.1-v26
  - 2.8.2-v21
  percona-xtradb:
  - 5.7-v33
  postgres:
  - 10.14-v37
  - 11.9-v37
  - 12.4-v37
  - 13.1-v34
  - 14.0-v26
  - 15.1-v18
  - 16.1-v7
  - 17.2-v5
  - 9.6.19-v37
  redis:
  - 5.0.13-v26
  - 6.2.5-v26
  - 7.0.5-v19
  ui-server: v0.23.0
  vault:
  - 1.10.3-v18
  version: v2025.10.17
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

- Learn how to use Stash to backup workloads data from [here](/docs/v2025.10.17/guides/workloads/overview/).
- Learn how to use Stash to backup databases from [here](/docs/v2025.10.17/guides/addons/overview/).
- Learn how to use Stash to backup stand-alone PVC from [here](/docs/v2025.10.17/guides/volumes/overview/).
