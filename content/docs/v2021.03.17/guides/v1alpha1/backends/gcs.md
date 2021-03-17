---
title: GCS Backend | Stash
description: Configure Stash to use Google Cloud Storage (GCS) as Backend.
menu:
  docs_v2021.03.17:
    identifier: v1alpha1-backend-gcs
    name: Google Cloud Storage
    parent: v1alpha1-backend
    weight: 50
product_name: stash
menu_name: docs_v2021.03.17
section_menu_id: guides
info:
  cli: v0.12.0
  community: v0.12.0
  elasticsearch:
  - 5.6.4-v7
  - 6.2.4-v7
  - 6.3.0-v7
  - 6.4.0-v7
  - 6.5.3-v7
  - 6.8.0-v7
  - 7.2.0-v7
  - 7.3.2-v7
  enterprise: v0.12.0
  installer: v0.12.0
  mariadb:
  - 10.5.8-v1
  mongodb:
  - 3.4.17-v6
  - 3.4.22-v6
  - 3.6.13-v6
  - 3.6.8-v6
  - 4.0.11-v6
  - 4.0.3-v6
  - 4.0.5-v6
  - 4.1.13-v6
  - 4.1.4-v6
  - 4.1.7-v6
  - 4.2.3-v6
  mysql:
  - 5.7.25-v7
  - 8.0.14-v7
  - 8.0.21-v1
  - 8.0.3-v7
  percona-xtradb:
  - 5.7-v2
  postgres:
  - 10.14-v5
  - 11.9-v5
  - 12.4-v5
  - 13.1-v2
  - 9.6.19-v5
  version: v2021.03.17
---

# Google Cloud Storage (GCS)

Stash supports Google Cloud Storage(GCS) as a backend. This tutorial will show you how to configure **Restic** and storage **Secret** for GCS backend.

#### Create Storage Secret

To configure storage secret for this backend, following secret keys are needed:

| Key                               | Description                                                |
|-----------------------------------|------------------------------------------------------------|
| `RESTIC_PASSWORD`                 | `Required`. Password used to encrypt snapshots by `restic` |
| `GOOGLE_PROJECT_ID`               | `Required`. Google Cloud project ID                        |
| `GOOGLE_SERVICE_ACCOUNT_JSON_KEY` | `Required`. Google Cloud service account json key          |

Create storage secret as below,

```bash
$ echo -n 'changeit' > RESTIC_PASSWORD
$ echo -n '<your-project-id>' > GOOGLE_PROJECT_ID
$ mv downloaded-sa-json.key GOOGLE_SERVICE_ACCOUNT_JSON_KEY
$ kubectl create secret generic gcs-secret \
    --from-file=./RESTIC_PASSWORD \
    --from-file=./GOOGLE_PROJECT_ID \
    --from-file=./GOOGLE_SERVICE_ACCOUNT_JSON_KEY
secret "gcs-secret" created
```

Verify that the secret has been created with respective keys,

```yaml
$ kubectl get secret gcs-secret -o yaml

apiVersion: v1
data:
  GOOGLE_PROJECT_ID: PHlvdXItcHJvamVjdC1pZD4=
  GOOGLE_SERVICE_ACCOUNT_JSON_KEY: ewogICJ0eXBlIjogInNlcnZpY2VfYWNjb3V...9tIgp9Cg==
  RESTIC_PASSWORD: Y2hhbmdlaXQ=
kind: Secret
metadata:
  creationTimestamp: 2017-06-28T13:06:51Z
  name: gcs-secret
  namespace: default
  resourceVersion: "5461"
  selfLink: /api/v1/namespaces/default/secrets/gcs-secret
  uid: a6983b00-5c02-11e7-bb52-08002711f4aa
type: Opaque
```

#### Configure Restic

Now, you have to configure Restic crd to use GCS bucket. You have to provide previously created storage secret in `spec.backend.storageSecretName` field.

Following parameters are available for `gcs` backend.

| Parameter      | Description                                                                     |
|----------------|---------------------------------------------------------------------------------|
| `gcs.bucket`   | `Required`. Name of Bucket. If the bucket does not exist yet, it will be created in the default location (US). It is not possible at the moment to have restic create a new bucket in a different location, so you need to create it using a different program.        |
| `gcs.prefix`   | `Optional`. Path prefix into bucket where repository will be created.           |

Below, the YAML for Restic crd configured to use GCS bucket.

```yaml
apiVersion: stash.appscode.com/v1alpha1
kind: Restic
metadata:
  name: gcs-restic
  namespace: default
spec:
  selector:
    matchLabels:
      app: gcs-restic
  fileGroups:
  - path: /source/data
    retentionPolicyName: 'keep-last-5'
  backend:
    gcs:
      bucket: stash-qa
      prefix: demo
    storageSecretName: gcs-secret
  schedule: '@every 1m'
  volumeMounts:
  - mountPath: /source/data
    name: source-data
  retentionPolicies:
  - name: 'keep-last-5'
    keepLast: 5
    prune: true
```

Now, create the Restic we have configured above for `gcs` backend,

```bash
$ kubectl apply -f https://github.com/stashed/docs/raw/{{< param "info.version" >}}/docs/examples/backends/gcs/gcs-restic.yaml
restic "gcs-restic" created
```

## Next Steps

- Learn how to use Stash in Google Kubernetes Engine (GKE) from [here](/docs/v2021.03.17/guides/v1alpha1/platforms/gke).
- Learn how to use Stash to backup a Kubernetes deployment from [here](/docs/v2021.03.17/guides/v1alpha1/backup).
- Learn how to recover from backed up snapshot from [here](/docs/v2021.03.17/guides/v1alpha1/restore).
