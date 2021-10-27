---
title: Etcd Backup & Restore Overview | Stash
description: How Etcd Backup & Restore Works in Stash
menu:
  docs_v2021.10.11:
    identifier: stash-etcd-overview
    name: How does it works?
    parent: stash-etcd
    weight: 10
product_name: stash
menu_name: docs_v2021.10.11
section_menu_id: stash-addons
info:
  cli: v0.16.0
  community: v0.16.0
  elasticsearch:
  - 5.6.4-v13
  - 6.2.4-v13
  - 6.3.0-v13
  - 6.4.0-v13
  - 6.5.3-v13
  - 6.8.0-v13
  - 7.2.0-v13
  - 7.3.2-v13
  enterprise: v0.16.0
  etcd:
  - 3.5.0
  installer: v2021.10.11
  mariadb:
  - 10.5.8-v6
  mongodb:
  - 3.4.17-v12
  - 3.4.22-v12
  - 3.6.13-v12
  - 3.6.8-v12
  - 4.0.11-v12
  - 4.0.3-v12
  - 4.0.5-v12
  - 4.1.13-v12
  - 4.1.4-v12
  - 4.1.7-v12
  - 4.2.3-v12
  - 4.4.6-v3
  - 5.0.3
  mysql:
  - 5.7.25-v13
  - 8.0.14-v13
  - 8.0.21-v7
  - 8.0.3-v13
  nats:
  - 2.6.1
  percona-xtradb:
  - 5.7-v8
  postgres:
  - 10.14-v11
  - 11.9-v11
  - 12.4-v11
  - 13.1-v8
  - "14.0"
  - 9.6.19-v11
  redis:
  - 5.0.13-v1
  - 6.2.5-v1
  version: v2021.10.11
---

{{< notice type="warning" message="This is an Enterprise-only feature. Please install [Stash Enterprise Edition](/docs/v2021.10.11/setup/install/enterprise) to try this feature." >}}

# How Stash Backups & Restores Etcd Database

Stash `{{< param "info.version" >}}` supports backup and restore operation of many databases. This guide will give you an overview of how Etcd database backup and restore process works in Stash.

## Backup

Stash supports taking backup of Etcd database using [etcdctl](https://github.com/etcd-io/etcd/tree/main/etcdctl). It is the most flexible way to perform backup and restore of Etcd database.

### How Backup Works

The following diagram shows how Stash takes backup of a Etcd database. Open the image in a new tab to see the enlarged version.

<figure align="center">
  <img alt="Etcd Backup Overview" src="/docs/v2021.10.11/addons/etcd/overview/images/etcd-backup.svg">
  <figcaption align="center">Fig: Etcd Backup Overview</figcaption>
</figure>

The backup process consists of the following steps:

1. At first, a user creates a secret with access credentials of the backend where the backed up data will be stored.

2. Then, she creates a `Repository` crd that specifies the backend information along with the secret that holds the credentials to access the backend.

3. Then, she creates a `BackupConfiguration` crd targeting the [AppBinding](/docs/v2021.10.11/concepts/crds/appbinding) crd of the desired database. The `BackupConfiguration` object also specifies the `Task` to use to backup the database.

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
  <img alt="Etcd Database Restore Overview" src="/docs/v2021.10.11/addons/etcd/overview/images/etcd-restore.svg">
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

- Backup your Etcd database using Stash following the guide from [here](/docs/v2021.10.11/addons/etcd/basic-auth/).
