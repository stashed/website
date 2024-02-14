---
title: Etcd Backup & Restore Overview | Stash
description: How Etcd Backup & Restore Works in Stash
menu:
  docs_v2024.2.13:
    identifier: stash-etcd-overview
    name: How does it work?
    parent: stash-etcd
    weight: 10
product_name: stash
menu_name: docs_v2024.2.13
section_menu_id: stash-addons
info:
  cli: v0.33.0
  community: v0.33.0
  elasticsearch:
  - 5.6.4-v30
  - 6.2.4-v30
  - 6.3.0-v30
  - 6.4.0-v30
  - 6.5.3-v30
  - 6.8.0-v30
  - 7.14.0-v16
  - 7.2.0-v30
  - 7.3.2-v30
  - 8.2.0-v13
  enterprise: v0.33.0
  etcd:
  - 3.5.0-v17
  installer: v2024.2.13
  kubedump:
  - 0.1.0-v13
  mariadb:
  - 10.5.8-v24
  mongodb:
  - 3.4.17-v30
  - 3.4.22-v30
  - 3.6.13-v30
  - 3.6.8-v30
  - 4.0.11-v30
  - 4.0.3-v30
  - 4.0.5-v30
  - 4.1.13-v30
  - 4.1.4-v30
  - 4.1.7-v30
  - 4.2.3-v30
  - 4.4.6-v21
  - 5.0.15-v3
  - 5.0.3-v18
  - 6.0.5-v6
  mysql:
  - 5.7.25-v30
  - 8.0.14-v30
  - 8.0.21-v24
  - 8.0.3-v30
  nats:
  - 2.6.1-v18
  - 2.8.2-v13
  percona-xtradb:
  - 5.7-v25
  postgres:
  - 10.14-v29
  - 11.9-v29
  - 12.4-v29
  - 13.1-v26
  - 14.0-v18
  - 15.1-v10
  - 9.6.19-v29
  redis:
  - 5.0.13-v18
  - 6.2.5-v18
  - 7.0.5-v11
  ui-server: v0.14.0
  vault:
  - 1.10.3-v10
  version: v2024.2.13
---

# How Stash Backups & Restores Etcd Database

Stash `{{< param "info.version" >}}` supports backup and restore operation of many databases. This guide will give you an overview of how Etcd database backup and restore process works in Stash.

## Backup

Stash supports taking backup of Etcd database using [etcdctl](https://github.com/etcd-io/etcd/tree/main/etcdctl). It is the most flexible way to perform backup and restore of Etcd database.

### How Backup Works

The following diagram shows how Stash takes backup of a Etcd database. Open the image in a new tab to see the enlarged version.

<figure align="center">
  <img alt="Etcd Backup Overview" src="/docs/v2024.2.13/addons/etcd/overview/images/etcd-backup.svg">
  <figcaption align="center">Fig: Etcd Backup Overview</figcaption>
</figure>

The backup process consists of the following steps:

1. At first, a user creates a secret with access credentials of the backend where the backed up data will be stored.

2. Then, she creates a `Repository` crd that specifies the backend information along with the secret that holds the credentials to access the backend.

3. Then, she creates a `BackupConfiguration` crd targeting the [AppBinding](/docs/v2024.2.13/concepts/crds/appbinding/) crd of the desired database. The `BackupConfiguration` object also specifies the `Task` to use to backup the database.

4. Stash operator watches for `BackupConfiguration` crd.

5. Once Stash operator finds a `BackupConfiguration` crd, it creates a CronJob with the schedule specified in `BackupConfiguration` object to trigger backup periodically.

6. On the next scheduled slot, the CronJob triggers a backup by creating a `BackupSession` crd.

7. Stash operator also watches for `BackupSession` crd.

8. When it finds a `BackupSession` object, it resolves the respective `Task` and `Function` and prepares a Job definition to backup.

9. Then, it creates the Job to backup the targeted database.

10. The backup Job reads the necessary information to connect with the database from the `AppBinding` crd. It also reads backend information and access credentials from `Repository` crd and Storage Secret respectively.

11. Then, the Job takes snapshot of the targeted database using `etcdctl` and uploads the snapshot to the backend. Stash stores the snapshot temporarily in a directory before uploading that into the backend. You can limit the temporary directory size using `spec.TempDir` field of `BackupConfiguration` crd.

12. Finally, when the backup is complete, the Job sends Prometheus metrics to the Pushgateway running inside Stash operator pod. It also updates the `BackupSession` and `Repository` status to reflect the backup completion.

### How Restore from Backup Works

The following diagram shows how Stash restores an Etcd database from a backup. Open the image in a new tab to see the enlarged version.

<figure align="center">
  <img alt="Etcd Database Restore Overview" src="/docs/v2024.2.13/addons/etcd/overview/images/etcd-restore.svg">
  <figcaption align="center">Fig: Etcd Restore Process Overview</figcaption>
</figure>

The restore process consists of the following steps:

1. At first, a user creates a `RestoreSession` crd targeting the `AppBinding` of the desired database where the backed up data will be restored. It also specifies the `Repository` crd which holds the backend information and the `Task` to use to restore the target.

2. Stash operator watches for `RestoreSession` object.

3. Once it finds a `RestoreSession` object, it resolves the respective `Task` and `Function` and prepares a Job definition to restore.

4. Then, it creates the Job to restore the target.

5. The Job reads necessary information to connect with the database from respective `AppBinding` crd. It also reads backend information and access credentials from `Repository` crd and Storage Secret respectively.

6. Then, the job downloads the backed up snapshot from the backend and restore that snapshot into the `Etcd` database. Stash stores the downloaded files temporarily before inserting into the targeted database.

7. Finally, when the restore process is complete, the Job sends Prometheus metrics to the Pushgateway and update the `RestoreSession` status to reflect restore completion.

## Next Steps

- Backup your Etcd database using Stash following the guide from [here](/docs/v2024.2.13/addons/etcd/basic-auth/).
