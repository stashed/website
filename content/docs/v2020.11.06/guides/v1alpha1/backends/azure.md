---
title: Azure Backend | Stash
description: Configure Stash to use Microsoft Azure Storage as Backend.
menu:
  docs_v2020.11.06:
    identifier: v1alpha1-backend-azure
    name: Azure Blob Storage
    parent: v1alpha1-backend
    weight: 40
product_name: stash
menu_name: docs_v2020.11.06
section_menu_id: guides
info:
  catalog: v2020.11.06
  cli: v0.11.6
  community: v0.11.6
  elasticsearch:
  - 5.6.4-v4
  - 6.2.4-v4
  - 6.3.0-v4
  - 6.4.0-v4
  - 6.5.3-v4
  - 6.8.0-v4
  - 7.2.0-v4
  - 7.3.2-v4
  enterprise: v0.11.6
  installer: v0.11.6
  mongodb:
  - 3.4.17-v4
  - 3.4.22-v4
  - 3.6.13-v4
  - 3.6.8-v4
  - 4.0.11-v4
  - 4.0.3-v4
  - 4.0.5-v4
  - 4.1.13-v4
  - 4.1.4-v4
  - 4.1.7-v4
  - 4.2.3-v4
  mysql:
  - 5.7.25-v4
  - 8.0.14-v4
  - 8.0.3-v4
  percona-xtradb:
  - 5.7-v4
  postgres:
  - 10.14.0-v2
  - 11.9.0-v2
  - 12.4.0-v2
  - 9.6.19-v2
  version: v2020.11.06
---

# Microsoft Azure Storage

Stash supports Microsoft Azure Storage as a backend. This tutorial will show you how to configure **Restic** and storage **Secret** for Azure Storage.

#### Create Storage Secret

To configure storage secret for this backend, following secret keys are needed:

| Key                     | Description                                                |
|-------------------------|------------------------------------------------------------|
| `RESTIC_PASSWORD`       | `Required`. Password used to encrypt snapshots by `restic` |
| `AZURE_ACCOUNT_NAME`    | `Required`. Azure Storage account name                     |
| `AZURE_ACCOUNT_KEY`     | `Required`. Azure Storage account key                      |

Create storage secret as below,

```bash
$ echo -n 'changeit' > RESTIC_PASSWORD
$ echo -n '<your-azure-storage-account-name>' > AZURE_ACCOUNT_NAME
$ echo -n '<your-azure-storage-account-key>' > AZURE_ACCOUNT_KEY
$ kubectl create secret generic azure-secret \
    --from-file=./RESTIC_PASSWORD \
    --from-file=./AZURE_ACCOUNT_NAME \
    --from-file=./AZURE_ACCOUNT_KEY
secret "azure-secret" created
```

Verify that the secret has been created with respective keys,

```yaml
$ kubectl get secret azure-secret -o yaml

apiVersion: v1
data:
  AZURE_ACCOUNT_KEY: PHlvdXItYXp1cmUtc3RvcmFnZS1hY2NvdW50LWtleT4=
  AZURE_ACCOUNT_NAME: PHlvdXItYXp1cmUtc3RvcmFnZS1hY2NvdW50LW5hbWU+
  RESTIC_PASSWORD: Y2hhbmdlaXQ=
kind: Secret
metadata:
  creationTimestamp: 2017-06-28T13:27:16Z
  name: azure-secret
  namespace: default
  resourceVersion: "6809"
  selfLink: /api/v1/namespaces/default/secrets/azure-secret
  uid: 80f658d1-5c05-11e7-bb52-08002711f4aa
type: Opaque
```

#### Configure Restic

Now, you have to configure Restic crd to use Azure Storage. You have to provide previously created storage secret in `spec.backend.storageSecretName` field.

Following parameters are available for `Azure` backend.

|     Parameter     |                              Description                              |
| ----------------- | --------------------------------------------------------------------- |
| `azure.container` | `Required`. Name of Storage container                                 |
| `azure.prefix`    | `Optional`. Path prefix into bucket where repository will be created. |

Below, the YAML for Restic crd configured to use Azure Storage.

```yaml
apiVersion: stash.appscode.com/v1alpha1
kind: Restic
metadata:
  name: azure-restic
  namespace: default
spec:
  selector:
    matchLabels:
      app: azure-restic
  fileGroups:
  - path: /source/data
    retentionPolicyName: 'keep-last-5'
  backend:
    azure:
      container: stashqa
      prefix: demo
    storageSecretName: azure-secret
  schedule: '@every 1m'
  volumeMounts:
  - mountPath: /source/data
    name: source-data
  retentionPolicies:
  - name: 'keep-last-5'
    keepLast: 5
    prune: true
```

Now, create the Restic we have configured above for `azure` backend,

```bash
$ kubectl apply -f https://github.com/stashed/docs/raw/{{< param "info.version" >}}/docs/examples/backends/azure/azure-restic.yaml
restic "azure-restic" created
```

## Next Steps

- Learn how to use Stash in Azure Kubernetes Service (AKS) from [here](/docs/v2020.11.06/guides/v1alpha1/platforms/aks).
- Learn how to use Stash to backup a Kubernetes deployment from [here](/docs/v2020.11.06/guides/v1alpha1/backup).
- Learn how to recover from backed up snapshot from [here](/docs/v2020.11.06/guides/v1alpha1/restore).
