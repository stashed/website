---
title: Backend Overview | Stash
description: An overview of the backends used by Stash to store backed up data.
menu:
  docs_v2020.09.16:
    identifier: backend-overview
    name: What is Backend?
    parent: backend
    weight: 10
product_name: stash
menu_name: docs_v2020.09.16
section_menu_id: guides
info:
  catalog: v2020.09.16
  cli: v0.11.0
  community: v0.11.0
  elasticsearch:
  - 5.6.4-v2
  - 6.2.4-v2
  - 6.3.0-v2
  - 6.4.0-v2
  - 6.5.3-v2
  - 6.8.0-v2
  - 7.2.0-v2
  - 7.3.2-v2
  enterprise: v0.11.0
  mongodb:
  - 3.4.17-v2
  - 3.4.22-v2
  - 3.6.13-v2
  - 3.6.8-v2
  - 4.0.11-v2
  - 4.0.3-v2
  - 4.0.5-v2
  - 4.1.13-v2
  - 4.1.4-v2
  - 4.1.7-v2
  - 4.2.3-v2
  mysql:
  - 5.7.25-v2
  - 8.0.14-v2
  - 8.0.3-v2
  percona-xtradb:
  - 5.7-v2
  postgres:
  - 10.14.0
  - 11.9.0
  - 12.4.0
  - 9.6.19
  version: v2020.09.16
---

> New to Stash? Please start [here](/docs/v2020.09.16/concepts/README).

# Stash Backends

Stash supports various backends for storing data snapshots. It can be a cloud storage like GCS bucket, AWS S3, Azure Blob Storage etc. or a Kubernetes persistent volume like [HostPath](https://kubernetes.io/docs/concepts/storage/volumes/#hostpath), [PersistentVolumeClaim](https://kubernetes.io/docs/concepts/storage/volumes/#persistentvolumeclaim), [NFS](https://kubernetes.io/docs/concepts/storage/volumes/#nfs) etc.

The following diagram shows how Stash sidecar container accesses and backs up data into a backend.

<figure align="center">
  <img alt="Stash Backend Overview" src="/docs/v2020.09.16/images/guides/latest/backends/backend_overview.svg">
  <figcaption align="center">Fig: Stash Backend Overview</figcaption>
</figure>

You have to create a [Repository](/docs/v2020.09.16/concepts/crds/repository) object which contains backend information and a `Secret` which contains necessary credentials to access the backend.

Stash sidecar/backup job reads backend information from the `Repository` and retrieves access credentials from the `Secret`. Then on the first backup session, Stash will initialize a repository in the backend.

Below, a screenshot that shows a repository created in AWS S3 bucket named `stash-qa`:

<figure align="center">
  <img alt="Repository in AWS S3 Backend" src="/docs/v2020.09.16/images/guides/latest/backends/s3_repository.png">
  <figcaption align="center">Fig: Repository in AWS S3 Backend</figcaption>
</figure>

You will see all snapshots taken by Stash at `/snapshot` directory of this repository.

> Note: Stash stores data encrypted at rest. So, snapshot files in the bucket will not contain any meaningful data until they are decrypted.

## Next Steps

- Learn how to configure `Kubernetes Volume` as backend from [here](/docs/v2020.09.16/guides/latest/backends/local).
- Learn how to configure `AWS S3/Minio/Rook` backend from [here](/docs/v2020.09.16/guides/latest/backends/s3).
- Learn how to configure `Google Cloud Storage (GCS)` backend from [here](/docs/v2020.09.16/guides/latest/backends/gcs).
- Learn how to configure `Microsoft Azure Storage` backend from [here](/docs/v2020.09.16/guides/latest/backends/azure).
- Learn how to configure `OpenStack Swift` backend from [here](/docs/v2020.09.16/guides/latest/backends/swift).
- Learn how to configure `Backblaze B2` backend from [here](/docs/v2020.09.16/guides/latest/backends/b2).
- Learn how to configure `REST` backend from [here](/docs/v2020.09.16/guides/latest/backends/rest).
