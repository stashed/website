---
title: Backend Overview | Stash
description: An overview of backends used by Stash to store snapshot data.
menu:
  docs_0.8.2:
    identifier: backend-overview
    name: What is Backend?
    parent: backend
    weight: 10
product_name: stash
menu_name: docs_0.8.2
section_menu_id: guides
info:
  version: 0.8.2
---

> New to Stash? Please start [here](/docs/0.8.2/concepts/README).

# Stash Backends

Backend is where Stash stores backup snapshots. It can be a cloud storage like GCS bucket, AWS S3, Azure Blob Storage etc. or a Kubernetes persistent volume like [HostPath](https://kubernetes.io/docs/concepts/storage/volumes/#hostpath), [PersistentVolumeClaim](https://kubernetes.io/docs/concepts/storage/volumes/#persistentvolumeclaim), [NFS](https://kubernetes.io/docs/concepts/storage/volumes/#nfs) etc. Below diagram show how Stash sidecar container access and store backup data into a backend storage.

<p align="center">
  <img alt="Stash Backup Overview" height="350px", src="/docs/0.8.2/images/backup-overview.png">
</p>

Stash sidecar container receive backend information from `spec.backend` field of [Restic](/docs/0.8.2/concepts/crds/restic) crd. It obtains necessary credentials to access the backend from the secret specified in `spec.backend.storageSecretName` field of Restic crd. Then on first backup schedule, Stash initialize a repository in the backend.

Below, a screenshot that show a repository created at AWS S3 bucket named `stash-qa` for a Deployment named `stash-demo`.

<p align="center">
  <img alt="Repository in AWS S3 Backend", src="/docs/0.8.2/images/platforms/eks/s3-backup-repository.png">
</p>

You will see all snapshots taken by Stash at `/snapshot` directory of this repository.

> Note: Stash keeps all backup data encrypted. So, snapshot files in the bucket will not contain any meaningful data until they are decrypted.

Stash creates a [Repository](/docs/0.8.2/concepts/crds/repository) crd that represents original repository in backend in Kubernetes native way. It holds information like number of backup snapshot taken, time when last backup was taken etc.

In order to use a backend, you have to configure `Restic` crd and create a `Secret` with necessary credentials.

## Next Steps

- Learn how to configure `local` backend from [here](/docs/0.8.2/guides/backends/local).
- Learn how to configure `AWS S3` backend from [here](/docs/0.8.2/guides/backends/s3).
- Learn how to configure `Google Cloud Storage (GCS)` backend from [here](/docs/0.8.2/guides/backends/gcs).
- Learn how to configure `Microsoft Azure Storage` backend from [here](/docs/0.8.2/guides/backends/azure).
- Learn how to configure `OpenStack Swift` backend from [here](/docs/0.8.2/guides/backends/swift).
- Learn how to configure `Backblaze B2` backend from [here](/docs/0.8.2/guides/backends/b2).
