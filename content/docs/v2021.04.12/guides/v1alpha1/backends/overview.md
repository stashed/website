---
title: Backend Overview | Stash
description: An overview of backends used by Stash to store snapshot data.
menu:
  docs_v2021.04.12:
    identifier: v1alpha1-backend-overview
    name: What is Backend?
    parent: v1alpha1-backend
    weight: 10
product_name: stash
menu_name: docs_v2021.04.12
section_menu_id: guides
info:
  cli: v0.12.3
  community: v0.12.3
  elasticsearch:
  - 5.6.4-v9
  - 6.2.4-v9
  - 6.3.0-v9
  - 6.4.0-v9
  - 6.5.3-v9
  - 6.8.0-v9
  - 7.2.0-v9
  - 7.3.2-v9
  enterprise: v0.12.3
  installer: v2021.04.12
  mariadb:
  - 10.5.8-v3
  mongodb:
  - 3.4.17-v8
  - 3.4.22-v8
  - 3.6.13-v8
  - 3.6.8-v8
  - 4.0.11-v8
  - 4.0.3-v8
  - 4.0.5-v8
  - 4.1.13-v8
  - 4.1.4-v8
  - 4.1.7-v8
  - 4.2.3-v8
  mysql:
  - 5.7.25-v9
  - 8.0.14-v9
  - 8.0.21-v3
  - 8.0.3-v9
  percona-xtradb:
  - 5.7-v4
  postgres:
  - 10.14-v7
  - 11.9-v7
  - 12.4-v7
  - 13.1-v4
  - 9.6.19-v7
  version: v2021.04.12
---

> New to Stash? Please start [here](/docs/v2021.04.12/concepts/README).

# Stash Backends

Backend is where Stash stores backup snapshots. It can be a cloud storage like GCS bucket, AWS S3, Azure Blob Storage etc. or a Kubernetes persistent volume like [HostPath](https://kubernetes.io/docs/concepts/storage/volumes/#hostpath), [PersistentVolumeClaim](https://kubernetes.io/docs/concepts/storage/volumes/#persistentvolumeclaim), [NFS](https://kubernetes.io/docs/concepts/storage/volumes/#nfs) etc. Below diagram show how Stash sidecar container access and store backup data into a backend storage.

<p align="center">
  <img alt="Stash Backend Overview" height="350px", src="/docs/v2021.04.12/images/guides/latest/backends/backend_overview.svg">
</p>

Stash sidecar container receive backend information from `spec.backend` field of [Restic](/docs/v2021.04.12/concepts/crds/v1alpha1/restic) crd. It obtains necessary credentials to access the backend from the secret specified in `spec.backend.storageSecretName` field of Restic crd. Then on first backup schedule, Stash initialize a repository in the backend.

Below, a screenshot that show a repository created at AWS S3 bucket named `stash-qa` for a Deployment named `stash-demo`.

<p align="center">
  <img alt="Repository in AWS S3 Backend", src="/docs/v2021.04.12/images/guides/latest/backends/s3_repository.png">
</p>

You will see all snapshots taken by Stash at `/snapshot` directory of this repository.

> Note: Stash keeps all backup data encrypted. So, snapshot files in the bucket will not contain any meaningful data until they are decrypted.

Stash creates a [Repository](/docs/v2021.04.12/concepts/crds/repository) crd that represents original repository in backend in a Kubernetes native way. It holds information like number of backup snapshot taken, time when last backup was taken etc.

In order to use a backend, you have to configure `Restic` crd and create a `Secret` with necessary credentials.

## Next Steps

- Learn how to configure `local` backend from [here](/docs/v2021.04.12/guides/v1alpha1/backends/local).
- Learn how to configure `AWS S3` backend from [here](/docs/v2021.04.12/guides/v1alpha1/backends/s3).
- Learn how to configure `Google Cloud Storage (GCS)` backend from [here](/docs/v2021.04.12/guides/v1alpha1/backends/gcs).
- Learn how to configure `Microsoft Azure Storage` backend from [here](/docs/v2021.04.12/guides/v1alpha1/backends/azure).
- Learn how to configure `OpenStack Swift` backend from [here](/docs/v2021.04.12/guides/v1alpha1/backends/swift).
- Learn how to configure `Backblaze B2` backend from [here](/docs/v2021.04.12/guides/v1alpha1/backends/b2).
