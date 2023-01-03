---
title: VolumeSnapshot Overview | Stash
description: An overview of how VolumeSnapshot works in Stash
menu:
  docs_v2023.01.05:
    identifier: volume-snapshot-overview
    name: How VolumeSnapshot works?
    parent: volume-snapshot
    weight: 10
product_name: stash
menu_name: docs_v2023.01.05
section_menu_id: guides
info:
  cli: v0.25.0
  community: v0.25.0
  elasticsearch:
  - 5.6.4-v21
  - 6.2.4-v21
  - 6.3.0-v21
  - 6.4.0-v21
  - 6.5.3-v21
  - 6.8.0-v21
  - 7.14.0-v7
  - 7.2.0-v21
  - 7.3.2-v21
  - 8.2.0-v4
  enterprise: v0.25.0
  etcd:
  - 3.5.0-v8
  installer: v2023.01.05
  kubedump:
  - 0.1.0-v4
  mariadb:
  - 10.5.8-v14
  mongodb:
  - 3.4.17-v21
  - 3.4.22-v21
  - 3.6.13-v21
  - 3.6.8-v21
  - 4.0.11-v21
  - 4.0.3-v21
  - 4.0.5-v21
  - 4.1.13-v21
  - 4.1.4-v21
  - 4.1.7-v21
  - 4.2.3-v21
  - 4.4.6-v12
  - 5.0.3-v9
  mysql:
  - 5.7.25-v21
  - 8.0.14-v21
  - 8.0.21-v15
  - 8.0.3-v21
  nats:
  - 2.6.1-v9
  - 2.8.2-v4
  percona-xtradb:
  - 5.7-v16
  postgres:
  - 10.14-v20
  - 11.9-v20
  - 12.4-v20
  - 13.1-v17
  - 14.0-v9
  - 15.1-v1
  - 9.6.19-v20
  redis:
  - 5.0.13-v9
  - 6.2.5-v9
  - 7.0.5-v2
  ui-server: v0.7.0
  vault:
  - 1.10.3-v1
  version: v2023.01.05
---

# VolumeSnapshot Using Stash

This guide will give you an overview of how VolumeSnapshot process works in Stash.

## How Backup Process Works?

The following diagram shows how Stash creates VolumeSnapshot via Kubernetes native API. Open the image in a new tab to see the enlarged version.

<figure align="center">
  <img alt="Stash Backup Flow" src="/docs/v2023.01.05/guides/volumesnapshot/overview/images/volumesnapshot-overview.svg">
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
  <img alt="Stash Backup Flow" src="/docs/v2023.01.05/guides/volumesnapshot/overview/images/restore-overview.svg">
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

1. See a step by step guide to snapshot the volumes of a Deployment [here](/docs/v2023.01.05/guides/volumesnapshot/deployment/).
2. See a step by step guide to snapshot the volumes of a StatefulSet [here](/docs/v2023.01.05/guides/volumesnapshot/statefulset/).
3. See a step by step guide to snapshot a stand-alone PVC [here](/docs/v2023.01.05/guides/volumesnapshot/pvc/).
