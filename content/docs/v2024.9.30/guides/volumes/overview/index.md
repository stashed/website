---
title: Stand-alone Volume Backup Overview | Stash
description: An overview on how stand-alone volume backup works in Stash.
menu:
  docs_v2024.9.30:
    identifier: volume-backup-overview
    name: How does it work?
    parent: volume-backup
    weight: 10
product_name: stash
menu_name: docs_v2024.9.30
section_menu_id: guides
info:
  cli: v0.36.0
  community: v0.36.0
  elasticsearch:
  - 5.6.4-v32
  - 6.2.4-v32
  - 6.3.0-v32
  - 6.4.0-v32
  - 6.5.3-v32
  - 6.8.0-v32
  - 7.14.0-v18
  - 7.2.0-v32
  - 7.3.2-v32
  - 8.2.0-v15
  enterprise: v0.36.0
  etcd:
  - 3.5.0-v19
  installer: v2024.9.30
  kubedump:
  - 0.1.0-v15
  mariadb:
  - 10.5.8-v26
  mongodb:
  - 3.4.17-v33
  - 3.4.22-v33
  - 3.6.13-v33
  - 3.6.8-v33
  - 4.0.11-v33
  - 4.0.3-v33
  - 4.0.5-v33
  - 4.1.13-v33
  - 4.1.4-v33
  - 4.1.7-v33
  - 4.2.3-v33
  - 4.4.6-v24
  - 5.0.15-v6
  - 5.0.3-v21
  - 6.0.5-v9
  mysql:
  - 5.7.25-v32
  - 8.0.14-v32
  - 8.0.21-v26
  - 8.0.3-v32
  nats:
  - 2.6.1-v20
  - 2.8.2-v15
  percona-xtradb:
  - 5.7-v27
  postgres:
  - 10.14-v31
  - 11.9-v31
  - 12.4-v31
  - 13.1-v28
  - 14.0-v20
  - 15.1-v12
  - 16.1-v1
  - 9.6.19-v31
  redis:
  - 5.0.13-v20
  - 6.2.5-v20
  - 7.0.5-v13
  ui-server: v0.17.0
  vault:
  - 1.10.3-v12
  version: v2024.9.30
---

# Stand-alone Volume Backup Overview

If you are using a volume that can be mounted in multiple workloads, aka `ReadWriteMany`/`RWX`, you might want to backup the volume independent of the workloads. Stash supports backup of stand-alone volumes. This guide will give you an overview of how stand-alone volume backup and restore process works in Stash.

## How Backup Works

The following diagram shows how Stash takes backup of a stand-alone volume. Open the image in a new tab to see the enlarged version.

<figure align="center">
  <img alt="Stand-alone Volume Backup Overview" src="images/volume_backup_overview.svg">
  <figcaption align="center">Fig: Stand-alone Volume Backup Overview</figcaption>
</figure>

The backup process consists of the following steps:

1. At first, a user creates a secret with access credentials of the backend where the backed up data will be stored.

2. Then, she creates a `Repository` crd that specifies the backend information along with the secret that holds the credentials to access the backend.

3. Then, she creates a `BackupConfiguration` crd targeting the volume. The `BackupConfiguration` object also specifies the `Task` to use to backup the volume.

4. Stash operator watches for `BackupConfiguration` crd.

5. Once Stash operator finds a `BackupConfiguration` crd, it creates a CronJob with the schedule specified in `BackupConfiguration` object to trigger backup periodically.

6. On next scheduled slot, the CronJob triggers a backup by creating a `BackupSession` crd.

7. Stash operator also watches for `BackupSession` crd.

8. When it finds a `BackupSession` object, it resolves the respective `Task` and `Function` and prepares a backup Job definition.

9. Then, it mounts the targeted volume into the Job and creates it.

10. The Job takes backup of the targeted volume.

11. Finally, when backup is completed, the Job sends Prometheus metrics to the Pushgateway running inside Stash operator pod. It also updates the `BackupSession` and `Repository` status to reflect the backup procedure.

## How Restore Works

The following diagram shows how Stash restores backed up data into a stand-alone volume. Open the image in a new tab to see the enlarged version.

<figure align="center">
  <img alt="Stand-alone Volume Restore Overview" src="images/volume_restore_overview.svg">
  <figcaption align="center">Fig: Stand-alone Volume Restore Overview</figcaption>
</figure>

The restore process consists of the following steps:

1. At first, a user creates a `RestoreSession` crd which specifies the targeted volume where the backed up data will be restored and the `Repository` crd which holds the backend information where the backed up data has been stored. It also specifies the `Task` to use to restore the target.

2. Stash operator watches for `RestoreSession` object.

3. Once it finds a `RestoreSession` object, it resolves the respective `Task` and `Function` and prepares a restore Job definition.

4. Then, it mounts the targeted volume into the Job and creates it.

5. The Job restores the backed up data into the volume.

6. Finally, when the restore process is completed, the Job sends Prometheus metrics to the Pushgateway and update the `RestoreSession` status to reflect restore completion.

### Why use Function-Task model to backup or restore a volume

You might be wondering why we have used [Function](/docs/v2024.9.30/concepts/crds/function/) and [Task](/docs/v2024.9.30/concepts/crds/task/) to backup or restore a volume. Well, let us explain.

`Function-Task` model gives you the flexibility to customize the backup/restore process. For example, it enables you to execute some logic to prepare your apps before backup or execute logic to delete corrupted data before restore. For more details about what are the others benefits of `Function-Task` model, please visit [here](/docs/v2024.9.30/concepts/crds/task/##why-function-and-task).

## Next Steps

- Learn how to backup and restore a stand-alone PVC from [here](/docs/v2024.9.30/guides/volumes/pvc/).
