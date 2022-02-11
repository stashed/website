---
title: VolumeSnapshot Overview | Stash
description: An overview of how VolumeSnapshot works in Stash
menu:
  docs_v2022.02.22:
    identifier: volume-snapshot-overview
    name: How VolumeSnapshot works?
    parent: volume-snapshot
    weight: 10
product_name: stash
menu_name: docs_v2022.02.22
section_menu_id: guides
info:
  cli: v0.18.0
  community: v0.18.0
  elasticsearch:
  - 5.6.4-v15
  - 6.2.4-v15
  - 6.3.0-v15
  - 6.4.0-v15
  - 6.5.3-v15
  - 6.8.0-v15
  - 7.14.0-v1
  - 7.2.0-v15
  - 7.3.2-v15
  enterprise: v0.18.0
  etcd:
  - 3.5.0-v2
  installer: v2022.02.22
  mariadb:
  - 10.5.8-v8
  mongodb:
  - 3.4.17-v14
  - 3.4.22-v14
  - 3.6.13-v14
  - 3.6.8-v14
  - 4.0.11-v14
  - 4.0.3-v14
  - 4.0.5-v14
  - 4.1.13-v14
  - 4.1.4-v14
  - 4.1.7-v14
  - 4.2.3-v14
  - 4.4.6-v5
  - 5.0.3-v2
  mysql:
  - 5.7.25-v15
  - 8.0.14-v15
  - 8.0.21-v9
  - 8.0.3-v15
  nats:
  - 2.6.1-v2
  percona-xtradb:
  - 5.7-v10
  postgres:
  - 10.14-v13
  - 11.9-v13
  - 12.4-v13
  - 13.1-v10
  - 14.0-v2
  - 9.6.19-v13
  redis:
  - 5.0.13-v3
  - 6.2.5-v3
  ui-server: v0.1.0
  version: v2022.02.22
---

# How VolumeSnapshot works in Stash

This guide will show you how Stash takes snapshot of PersistentVolumeClaims and restore them from snapshot using Kubernetes VolumeSnapshot API.

## Before You Begin

- At first, you need to have a Kubernetes cluster and ensure that a CSI driver that implements snapshots is deployed on your cluster. You can find a list of CSI drivers that supports snapshots [here](https://kubernetes.io/blog/2019/01/17/update-on-volume-snapshot-alpha-for-kubernetes/). In this guide we are going to use [GCE Persistent Disk CSI Driver](https://github.com/kubernetes-sigs/gcp-compute-persistent-disk-csi-driver).

- You need to enable the Kubernetes `VolumeSnapshotDataSource` alpha feature via Kubernetes feature gates
  - `--feature-gates=VolumeSnapshotDataSource=true`
- Install `Stash` in your cluster following the steps [here](/docs/v2022.02.22/setup/README).
- You should be familiar with the following Stash concepts:
  - [BackupConfiguration](/docs/v2022.02.22/concepts/crds/backupconfiguration)
  - [BackupSession](/docs/v2022.02.22/concepts/crds/backupsession)
  - [RestoreSession](/docs/v2022.02.22/concepts/crds/restoresession)
- You should be also familiar with the following Kubernetes concepts:
  - [VolumeSnapshot](https://kubernetes.io/docs/concepts/storage/volume-snapshots/#volumesnapshots)
  - [VolumeSnapshotContent](https://kubernetes.io/docs/concepts/storage/volume-snapshots/#volume-snapshot-contents)
  - [VolumeSnapshotClass](https://kubernetes.io/docs/concepts/storage/volume-snapshot-classes/)

## How Backup Process Works?

The following diagram shows how Stash creates VolumeSnapshot via Kubernetes native API. Open the image in a new tab to see the enlarged version.

<figure align="center">
  <img alt="Stash Backup Flow" src="/docs/v2022.02.22/images/guides/volumesnapshot/volumesnapshot-overview.svg">
<figcaption align="center">Fig: Volume Snapshotting Process in Stash</figcaption>
</figure>

The `VolumeSnapshot` process consists of the following steps:

1. At first, a user creates a `BackupConfiguration` crd which specifies the targeted workload or targeted PVC.

2. Stash operator watches for `BackupConfiguration` crd.

3. When it finds a `BackupConfiguration` crd, it creates a `CronJob` to take a periodic backup of the target volumes.

4. The `CronJob` triggers backup on each scheduled time slot by creating a `BackupSession` crd.

5. Stash operator watches for `BackupSession` crd.

6. When it finds a `BackupSession` crd, it creates a volume snapshotter `Job` to take snapshot of the targeted volumes.

7. The volume snapshotter `Job` creates `VolumeSnapshot` crd for each PVC of the target and waits for the CSI driver to complete snapshotting. These `VolumeSnasphot` crd names follow the following format:
```bash
  <PVC name>-<BackupSession creation timestamp in Unix epoch seconds>
```

8. CSI `external-snapshotter` controller watches for `VolumeSnapshot`.

9. When it finds a `VolumeSnapshot` object, it backups `VolumeSnapshot` in the respective cloud storage.

10. Once the snapsotting is completed, Stash Operator updates the `status.phase` field of the `BackupSession` crd.

## How Restore Process Works?

The following diagram shows how Stash restores PersistentVolumeClaims from snapshot using Kubernetes VolumeSnapshot API. Open the image in a new tab to see the enlarged version.

<figure align="center">
  <img alt="Stash Backup Flow" src="/docs/v2022.02.22/images/guides/volumesnapshot/restore-overview.svg">
<figcaption align="center">Fig: Restore process from snapshot in Stash</figcaption>
</figure>

The restore process consists of the following steps:

1. At first, a user creates a `RestoreSession` crd which specifies the `volumeClaimTemplates`. `VolumeClaimTemplates` hold the information about `VolumeSnapshot`.

2. Stash operator watches for `RestoreSession` crd.

3. When it finds a `RestoreSession` crd, it creates a Restore `Job`to restore PVC from the snapshot.

4. The restore `Job` creates PVC with `spec.dataSource` field set to the respective VolumeSnapshot name.

5. CSI `external-snapshotter` controller watches for PVC.

6. When it finds a new PVC with `spec.dataSource` field set, it reads the information about the `VolumeSnapshot`.

7. The controller downloads the respective data from the cloud and populate the PVC with it.

8. Once restore process is completed, the Stash operator updates the `status.phase` field of the `BackupSession` crd.

## Next Steps

1. See a step by step guide to snapshot the volumes of a Deployment [here](/docs/v2022.02.22/guides/volumesnapshot/deployment).
2. See a step by step guide to snapshot the volumes of a StatefulSet [here](/docs/v2022.02.22/guides/volumesnapshot/statefulset).
3. See a step by step guide to snapshot a stand-alone PVC [here](/docs/v2022.02.22/guides/volumesnapshot/pvc).
