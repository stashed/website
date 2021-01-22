---
title: MariaDB Backup & Restore Overview | Stash
description: How MariaDB Backup & Restore Works in Stash
menu:
  docs_v2021.01.21:
    identifier: stash-mariadb-overview
    name: How does it works?
    parent: stash-mariadb
    weight: 10
product_name: stash
menu_name: docs_v2021.01.21
section_menu_id: stash-addons
info:
  catalog: v2021.01.21
  cli: v0.11.9
  community: v0.11.9
  elasticsearch:
  - 5.6.4-v6
  - 6.2.4-v6
  - 6.3.0-v6
  - 6.4.0-v6
  - 6.5.3-v6
  - 6.8.0-v6
  - 7.2.0-v6
  - 7.3.2-v6
  enterprise: v0.11.9
  installer: v0.11.9
  mariadb:
  - 10.5.8
  mongodb:
  - 3.4.17-v5
  - 3.4.22-v5
  - 3.6.13-v5
  - 3.6.8-v5
  - 4.0.11-v5
  - 4.0.3-v5
  - 4.0.5-v5
  - 4.1.13-v5
  - 4.1.4-v5
  - 4.1.7-v5
  - 4.2.3-v5
  mysql:
  - 5.7.25-v6
  - 8.0.14-v6
  - 8.0.21
  - 8.0.3-v6
  percona-xtradb:
  - 5.7.0-v1
  postgres:
  - 10.14.0-v4
  - 11.9.0-v4
  - 12.4.0-v4
  - 13.1.0-v1
  - 9.6.19-v4
  version: v2021.01.21
---

{{< notice type="warning" message="This is an Enterprise-only feature. Please install [Stash Enterprise Edition](/docs/v2021.01.21/setup/install/enterprise) to try this feature." >}}

# How Stash Backups & Restores MariaDB Database

Stash 0.9.0+ supports backup and restore operation of many databases. This guide will give you an overview of how MariaDB database backup and restore process works in Stash.

## Logical Backup

Stash supports taking [logical backup](https://mariadb.com/kb/en/backup-and-restore-overview/#logical-vs-physical-backups) of MariaDB databases using [mysqldump](https://mariadb.com/kb/en/mysqldump/). It is the most flexible way to perform a backup and restore, and a good choice when the data size is relatively small.

### How Logical Backup Works

The following diagram shows how Stash takes logical backup of a MariaDB database. Open the image in a new tab to see the enlarged version.

<figure align="center">
  <img alt="MariaDB Backup Overview" src="/docs/v2021.01.21/images/addons/mariadb/mariadb-logical-backup.svg">
  <figcaption align="center">Fig: MariaDB Logical Backup Overview</figcaption>
</figure>

The backup process consists of the following steps:

1. At first, a user creates a secret with access credentials of the backend where the backed up data will be stored.

2. Then, she creates a `Repository` crd that specifies the backend information along with the secret that holds the credentials to access the backend.

3. Then, she creates a `BackupConfiguration` crd targeting the [AppBinding](/docs/v2021.01.21/concepts/crds/appbinding) crd of the desired database. The `BackupConfiguration` object also specifies the `Task` to use to backup the database.

4. Stash operator watches for `BackupConfiguration` crd.

5. Once Stash operator finds a `BackupConfiguration` crd, it creates a CronJob with the schedule specified in `BackupConfiguration` object to trigger backup periodically.

6. On the next scheduled slot, the CronJob triggers a backup by creating a `BackupSession` crd.

7. Stash operator also watches for `BackupSession` crd.

8. When it finds a `BackupSession` object, it resolves the respective `Task` and `Function` and prepares a Job definition to backup.

9. Then, it creates the Job to backup the targeted database.

10. The backup Job reads necessary information to connect with the database from the `AppBinding` crd. It also reads backend information and access credentials from `Repository` crd and Storage Secret respectively.

11. Then, the Job dumps the targeted database and uploads the output to the backend. Stash pipes the output of dump command to uploading process. Hence, backup Job does not require a large volume to hold the entire dump output.

12. Finally, when the backup is complete, the Job sends Prometheus metrics to the Pushgateway running inside Stash operator pod. It also updates the `BackupSession` and `Repository` status to reflect the backup procedure.

### How Restore from Logical Backup Works

The following diagram shows how Stash restores a MariaDB database from a logical backup. Open the image in a new tab to see the enlarged version.

<figure align="center">
  <img alt="Database Restore Overview" src="/docs/v2021.01.21/images/addons/mariadb/mariadb-logical-restore.svg">
  <figcaption align="center">Fig: MariaDB Logical Restore Process Overview</figcaption>
</figure>

The restore process consists of the following steps:

1. At first, a user creates a `RestoreSession` crd targeting the `AppBinding` of the desired database where the backed up data will be restored. It also specifies the `Repository` crd which holds the backend information and the `Task` to use to restore the target.

2. Stash operator watches for `RestoreSession` object.

3. Once it finds a `RestoreSession` object, it resolves the respective `Task` and `Function` and prepares a Job definition to restore.

4. Then, it creates the Job to restore the target.

5. The Job reads necessary information to connect with the database from respective `AppBinding` crd. It also reads backend information and access credentials from `Repository` crd and Storage Secret respectively.

6. Then, the job downloads the backed up data from the backend and injects into the desired database. Stash pipes the downloaded data to the respective database tool to inject into the database. Hence, restore job does not require a large volume to download entire backup data inside it.

7. Finally, when the restore process is complete, the Job sends Prometheus metrics to the Pushgateway and update the `RestoreSession` status to reflect restore completion.

## Next Steps

- Install MariaDB addon for Stash following the guide from [here](/docs/v2021.01.21/addons/mariadb/setup/install).
