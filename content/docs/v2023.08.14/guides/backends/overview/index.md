---
title: Backend Overview | Stash
description: An overview of the backends used by Stash to store backed up data.
menu:
  docs_v2023.08.14:
    identifier: backend-overview
    name: What is Backend?
    parent: backend
    weight: 10
product_name: stash
menu_name: docs_v2023.08.14
section_menu_id: guides
info:
  cli: v0.31.0
  community: v0.31.0
  elasticsearch:
  - 5.6.4-v27
  - 6.2.4-v27
  - 6.3.0-v27
  - 6.4.0-v27
  - 6.5.3-v27
  - 6.8.0-v27
  - 7.14.0-v13
  - 7.2.0-v27
  - 7.3.2-v27
  - 8.2.0-v10
  enterprise: v0.31.0
  etcd:
  - 3.5.0-v14
  installer: v2023.08.14
  kubedump:
  - 0.1.0-v10
  mariadb:
  - 10.5.8-v20
  mongodb:
  - 3.4.17-v27
  - 3.4.22-v27
  - 3.6.13-v27
  - 3.6.8-v27
  - 4.0.11-v27
  - 4.0.3-v27
  - 4.0.5-v27
  - 4.1.13-v27
  - 4.1.4-v27
  - 4.1.7-v27
  - 4.2.3-v27
  - 4.4.6-v18
  - 5.0.15
  - 5.0.3-v15
  - 6.0.5-v3
  mysql:
  - 5.7.25-v27
  - 8.0.14-v27
  - 8.0.21-v21
  - 8.0.3-v27
  nats:
  - 2.6.1-v15
  - 2.8.2-v10
  percona-xtradb:
  - 5.7-v22
  postgres:
  - 10.14-v26
  - 11.9-v26
  - 12.4-v26
  - 13.1-v23
  - 14.0-v15
  - 15.1-v7
  - 9.6.19-v26
  redis:
  - 5.0.13-v15
  - 6.2.5-v15
  - 7.0.5-v8
  ui-server: v0.13.0
  vault:
  - 1.10.3-v7
  version: v2023.08.14
---

> New to Stash? Please start [here](/docs/v2023.08.14/concepts/README).

# Stash Backends

Stash supports various backends for storing data snapshots. It can be a cloud storage like GCS bucket, AWS S3, Azure Blob Storage etc. or a Kubernetes persistent volume like [HostPath](https://kubernetes.io/docs/concepts/storage/volumes/#hostpath), [PersistentVolumeClaim](https://kubernetes.io/docs/concepts/storage/volumes/#persistentvolumeclaim), [NFS](https://kubernetes.io/docs/concepts/storage/volumes/#nfs) etc.

The following diagram shows how Stash sidecar container accesses and backs up data into a backend.

<figure align="center">
  <img alt="Stash Backend Overview" src="images/backend_overview.svg">
  <figcaption align="center">Fig: Stash Backend Overview</figcaption>
</figure>

You have to create a [Repository](/docs/v2023.08.14/concepts/crds/repository/) object which contains backend information and a `Secret` which contains necessary credentials to access the backend.

Stash sidecar/backup job reads backend information from the `Repository` and retrieves access credentials from the `Secret`. Then on the first backup session, Stash will initialize a repository in the backend.

Below, a screenshot that shows a repository created in AWS S3 bucket named `stash-qa`:

<figure align="center">
  <img alt="Repository in AWS S3 Backend" src="images/s3_repository.png">
  <figcaption align="center">Fig: Repository in AWS S3 Backend</figcaption>
</figure>

You will see all snapshots taken by Stash at `/snapshot` directory of this repository.

> Note: Stash stores data encrypted at rest. So, snapshot files in the bucket will not contain any meaningful data until they are decrypted.

## Next Steps

- Learn how to configure `Kubernetes Volume` as backend from [here](/docs/v2023.08.14/guides/backends/local/).
- Learn how to configure `AWS S3/Minio/Rook` backend from [here](/docs/v2023.08.14/guides/backends/s3/).
- Learn how to configure `Google Cloud Storage (GCS)` backend from [here](/docs/v2023.08.14/guides/backends/gcs/).
- Learn how to configure `Microsoft Azure Storage` backend from [here](/docs/v2023.08.14/guides/backends/azure/).
- Learn how to configure `OpenStack Swift` backend from [here](/docs/v2023.08.14/guides/backends/swift/).
- Learn how to configure `Backblaze B2` backend from [here](/docs/v2023.08.14/guides/backends/b2/).
- Learn how to configure `REST` backend from [here](/docs/v2023.08.14/guides/backends/rest/).
