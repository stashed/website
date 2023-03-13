---
title: Backend Overview | Stash
description: An overview of the backends used by Stash to store backed up data.
menu:
  docs_v2023.03.13:
    identifier: backend-overview
    name: What is Backend?
    parent: backend
    weight: 10
product_name: stash
menu_name: docs_v2023.03.13
section_menu_id: guides
info:
  cli: v0.27.0
  community: v0.27.0
  elasticsearch:
  - 5.6.4-v23
  - 6.2.4-v23
  - 6.3.0-v23
  - 6.4.0-v23
  - 6.5.3-v23
  - 6.8.0-v23
  - 7.14.0-v9
  - 7.2.0-v23
  - 7.3.2-v23
  - 8.2.0-v6
  enterprise: v0.27.0
  etcd:
  - 3.5.0-v10
  installer: v2023.03.13
  kubedump:
  - 0.1.0-v6
  mariadb:
  - 10.5.8-v16
  mongodb:
  - 3.4.17-v23
  - 3.4.22-v23
  - 3.6.13-v23
  - 3.6.8-v23
  - 4.0.11-v23
  - 4.0.3-v23
  - 4.0.5-v23
  - 4.1.13-v23
  - 4.1.4-v23
  - 4.1.7-v23
  - 4.2.3-v23
  - 4.4.6-v14
  - 5.0.3-v11
  mysql:
  - 5.7.25-v23
  - 8.0.14-v23
  - 8.0.21-v17
  - 8.0.3-v23
  nats:
  - 2.6.1-v11
  - 2.8.2-v6
  percona-xtradb:
  - 5.7-v18
  postgres:
  - 10.14-v22
  - 11.9-v22
  - 12.4-v22
  - 13.1-v19
  - 14.0-v11
  - 15.1-v3
  - 9.6.19-v22
  redis:
  - 5.0.13-v11
  - 6.2.5-v11
  - 7.0.5-v4
  ui-server: v0.9.0
  vault:
  - 1.10.3-v3
  version: v2023.03.13
---

> New to Stash? Please start [here](/docs/v2023.03.13/concepts/README).

# Stash Backends

Stash supports various backends for storing data snapshots. It can be a cloud storage like GCS bucket, AWS S3, Azure Blob Storage etc. or a Kubernetes persistent volume like [HostPath](https://kubernetes.io/docs/concepts/storage/volumes/#hostpath), [PersistentVolumeClaim](https://kubernetes.io/docs/concepts/storage/volumes/#persistentvolumeclaim), [NFS](https://kubernetes.io/docs/concepts/storage/volumes/#nfs) etc.

The following diagram shows how Stash sidecar container accesses and backs up data into a backend.

<figure align="center">
  <img alt="Stash Backend Overview" src="images/backend_overview.svg">
  <figcaption align="center">Fig: Stash Backend Overview</figcaption>
</figure>

You have to create a [Repository](/docs/v2023.03.13/concepts/crds/repository/) object which contains backend information and a `Secret` which contains necessary credentials to access the backend.

Stash sidecar/backup job reads backend information from the `Repository` and retrieves access credentials from the `Secret`. Then on the first backup session, Stash will initialize a repository in the backend.

Below, a screenshot that shows a repository created in AWS S3 bucket named `stash-qa`:

<figure align="center">
  <img alt="Repository in AWS S3 Backend" src="images/s3_repository.png">
  <figcaption align="center">Fig: Repository in AWS S3 Backend</figcaption>
</figure>

You will see all snapshots taken by Stash at `/snapshot` directory of this repository.

> Note: Stash stores data encrypted at rest. So, snapshot files in the bucket will not contain any meaningful data until they are decrypted.

## Next Steps

- Learn how to configure `Kubernetes Volume` as backend from [here](/docs/v2023.03.13/guides/backends/local/).
- Learn how to configure `AWS S3/Minio/Rook` backend from [here](/docs/v2023.03.13/guides/backends/s3/).
- Learn how to configure `Google Cloud Storage (GCS)` backend from [here](/docs/v2023.03.13/guides/backends/gcs/).
- Learn how to configure `Microsoft Azure Storage` backend from [here](/docs/v2023.03.13/guides/backends/azure/).
- Learn how to configure `OpenStack Swift` backend from [here](/docs/v2023.03.13/guides/backends/swift/).
- Learn how to configure `Backblaze B2` backend from [here](/docs/v2023.03.13/guides/backends/b2/).
- Learn how to configure `REST` backend from [here](/docs/v2023.03.13/guides/backends/rest/).
