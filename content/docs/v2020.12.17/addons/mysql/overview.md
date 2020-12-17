---
title: MySQL Backup & Restore Overview | Stash
description: How MySQL Backup & Restore Works in Stash
menu:
  docs_v2020.12.17:
    identifier: stash-mysql-overview
    name: How does it works?
    parent: stash-mysql
    weight: 10
product_name: stash
menu_name: docs_v2020.12.17
section_menu_id: stash-addons
info:
  catalog: v2020.12.17
  cli: v0.11.8
  community: v0.11.8
  elasticsearch:
  - 5.6.4-v5
  - 6.2.4-v5
  - 6.3.0-v5
  - 6.4.0-v5
  - 6.5.3-v5
  - 6.8.0-v5
  - 7.2.0-v5
  - 7.3.2-v5
  enterprise: v0.11.8
  installer: v0.11.8
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
  - 5.7.25-v5
  - 8.0.14-v5
  - 8.0.3-v5
  percona-xtradb:
  - 5.7.0-v1
  postgres:
  - 10.14.0-v4
  - 11.9.0-v4
  - 12.4.0-v4
  - 13.1.0-v1
  - 9.6.19-v4
  version: v2020.12.17
---

{{< notice type="warning" message="This is an Enterprise-only feature. Please install [Stash Enterprise Edition](/docs/v2020.12.17/setup/install/enterprise) to try this feature." >}}

# How Stash Backup & Restore MySQL Database

Stash 0.9.0+ supports backup and restore operation of many databases. This guide will give you an overview of how MySQL database backup and restore process works in Stash.

## How Backup Works

The following diagram shows how Stash takes backup of a MySQL database. Open the image in a new tab to see the enlarged version.

<figure align="center">
  <img alt="MySQL Backup Overview" src="/docs/v2020.12.17/images/addons/mysql/backup_overview.svg">
  <figcaption align="center">Fig: MySQL Backup Overview</figcaption>
</figure>

The backup process consists of the following steps:

1. At first, a user creates a secret with access credentials of the backend where the backed up data will be stored.

2. Then, she creates a `Repository` crd that specifies the backend information along with the secret that holds the credentials to access the backend.

3. Then, she creates a `BackupConfiguration` crd targeting the [AppBinding](/docs/v2020.12.17/concepts/crds/appbinding) crd of the desired database. The `BackupConfiguration` object also specifies the `Task` to use to backup the database.

4. Stash operator watches for `BackupConfiguration` crd.

5. Once Stash operator finds a `BackupConfiguration` crd, it creates a CronJob with the schedule specified in `BackupConfiguration` object to trigger backup periodically.

6. On the next scheduled slot, the CronJob triggers a backup by creating a `BackupSession` crd.

7. Stash operator also watches for `BackupSession` crd.

8. When it finds a `BackupSession` object, it resolves the respective `Task` and `Function` and prepares a Job definition to backup.

9. Then, it creates the Job to backup the targeted database.

10. The backup Job reads necessary information to connect with the database from the `AppBinding` crd. It also reads backend information and access credentials from `Repository` crd and Storage Secret respectively.

11. Then, the Job dumps the targeted database and uploads the output to the backend. Stash pipes the output of dump command to uploading process. Hence, backup Job does not require a large volume to hold the entire dump output.

12. Finally, when the backup is complete, the Job sends Prometheus metrics to the Pushgateway running inside Stash operator pod. It also updates the `BackupSession` and `Repository` status to reflect the backup procedure.

## How Restore Process Works

The following diagram shows how Stash restores backed up data into a MySQL database. Open the image in a new tab to see the enlarged version.

<figure align="center">
  <img alt="Database Restore Overview" src="/docs/v2020.12.17/images/addons/mysql/restore_overview.svg">
  <figcaption align="center">Fig: MySQL Restore Process Overview</figcaption>
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

- Install MySQL addon for Stash following the guide from [here](/docs/v2020.12.17/addons/mysql/setup/install).
