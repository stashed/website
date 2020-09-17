---
title: Percona XtraDB Backup & Restore Overview | Stash
description: How Percona XtraDB Backup & Restore Works in Stash
menu:
  docs_v2020.09.16:
    identifier: stash-percona-xtradb-overview
    name: How does it works?
    parent: stash-percona-xtradb
    weight: 10
product_name: stash
menu_name: docs_v2020.09.16
section_menu_id: stash-addons
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
  - 3.4.1-v2
  - 3.4.2-v2
  - 3.6.1-v2
  - 3.6.8-v2
  - 4.0.11-v2
  - 4.0.3-v2
  - 4.0.5-v2
  - 4.1.1-v2
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

{{< notice type="warning" message="This is an Enterprise-only feature. Please install [Stash Enterprise Edition](/docs/v2020.09.16/setup/install/enterprise) to try this feature." >}}

# How Stash Backup & Restore Percona XtraDB Database

Stash 0.9.0+ supports backup and restore operation of many databases. This guide will give you an overview of how Percona XtraDB database backup and restore process works in Stash.

## How Backup Works

The following diagram shows how Stash takes backup of a Percona XtraDB database. Open the image in a new tab to see the enlarged version.

<figure align="center">
  <img alt="Percona XtraDB Backup Overview" src="/docs/v2020.09.16/images/addons/percona-xtradb/backup_overview.svg">
  <figcaption align="center">Fig: Percona XtraDB Backup Overview</figcaption>
</figure>

The backup process consists of the following steps:

1. At first, a user creates a secret with access credentials of the backend where the backed up data will be stored.

2. Then, she creates a `Repository` crd that specifies the backend information along with the secret that holds the credentials to access the backend.

3. Then, she creates a `BackupConfiguration` crd targeting the [AppBinding](/docs/v2020.09.16/concepts/crds/appbinding) crd of the desired database. The `BackupConfiguration` object also specifies the `Task` to use to backup the database.

4. Stash operator watches for `BackupConfiguration` crd.

5. Once Stash operator finds a `BackupConfiguration` crd, it creates a CronJob with the schedule specified in `BackupConfiguration` object to trigger backup periodically.

6. On the next scheduled slot, the CronJob triggers a backup by creating a `BackupSession` crd.

7. Stash operator also watches for `BackupSession` crd.

8. When it finds a `BackupSession` object, it resolves the respective `Task` and `Function` and prepares a Job definition to backup.

9. Then, it creates the Job to backup the targeted database.

10. The backup Job reads necessary information to connect with the database from the `AppBinding` crd. It also reads backend information and access credentials from `Repository` crd and Storage Secret respectively.

11. Then, the Job dumps the targeted database(s) and uploads the output to the backend. Stash pipes the output of the dump command to the upload process. Hence, backup Job does not require a large volume to hold the entire dump output.

12. Finally, when the backup is complete, the Job sends Prometheus metrics to the Pushgateway running inside Stash operator pod. It also updates the `BackupSession` and `Repository` status to reflect the backup procedure.

### Backup Different Percona XtraDB Configurations

This section will show you how backup works for different Percona XtraDB Configurations.

#### Standalone Percona XtraDB

For a standalone Percona XtraDB database, the backup job directly dumps the database using `mysqldump` and pipe the output to the backup process.

<figure align="center">
 <img alt="Standalone Percona XtraDB Backup Overview" src="/docs/v2020.09.16/images/addons/percona-xtradb/standalone_backup.png">
  <figcaption align="center">Fig: Standalone Percona XtraDB Backup</figcaption>
</figure>

#### Percona XtraDB Cluster

For a standalone Percona XtraDB database, the backup Job runs the backup procedure to take the backup of the targeted databases and uploads the output to the backend. In backup procedure, the Job runs a process called `garbd` ([Galera Arbitrator](https://galeracluster.com/library/documentation/arbitrator.html)) which uses `xtrabackup-v2` script during State Snapshot Transfer (SST). Basically this Job takes a full copy of the data stored in  the data directory (`/var/lib/mysql`) and pipes the output of the backup procedure to the uploading process. Hence, backup Job does not require a large volume to hold the entire backed up data.

<figure align="center">
 <img alt="Percona XtraDB Cluster Backup Overview" src="/docs/v2020.09.16/images/addons/percona-xtradb/cluster_backup.png">
  <figcaption align="center">Fig: Percona XtraDB Cluster Backup</figcaption>
</figure>

## How Restore Process Works

The following diagram shows how Stash restores backed up data into a Percona XtraDB database. Open the image in a new tab to see the enlarged version.

<figure align="center">
  <img alt="Percona XtraDB Restore Overview" src="/docs/v2020.09.16/images/addons/percona-xtradb/restore_overview.svg">
  <figcaption align="center">Fig: Percona XtraDB Restore Process Overview</figcaption>
</figure>

The restore process consists of the following steps:

1. At first, a user creates a `RestoreSession` crd targeting the `AppBinding` of the desired database where the backed up data will be restored. It also specifies the `Repository` crd which holds the backend information and the `Task` to use to restore the target.

2. Stash operator watches for `RestoreSession` object.

3. Once it finds a `RestoreSession` object, it resolves the respective `Task` and `Function` and prepares a Job (in case of restoring cluster more than one Job and PVC) definition(s) to restore.

4. Then, it creates the Job(s) (as well as PVCs in case of cluster) to restore the target.

5. The Job(s) reads necessary information to connect with the database from respective `AppBinding` crd. It also reads backend information and access credentials from `Repository` crd and Storage Secret respectively.

6. Then, the Job(s) downloads the backed up data from the backend and injects into the desired database. Stash pipes the downloaded data to inject into the database. Hence, the restore Job(s) does not require a large volume to download entire backup data inside it.

7. Finally, when the restore process is complete, the Job(s) sends Prometheus metrics to the Pushgateway and update the `RestoreSession` status to reflect restore completion.

### Restore Different Percona XtraDB Configurations

This section will show you how restore works for different Percona XtraDB Configurations.

#### Standalone Percona XtraDB

For a standalone Percona XtraDB database, the restore Job downloads the backed up data from the backend and pipe the downloaded data to `mysql` command which inserts the data into the desired database.

<figure align="center">
 <img alt="Standalone Percona XtraDB Restore Overview" src="/docs/v2020.09.16/images/addons/percona-xtradb/standalone_restore.png">
  <figcaption align="center">Fig: Standalone Percona XtraDB Restore</figcaption>
</figure>

#### Percona XtraDB Cluster

For a Percona XtraDB Cluster, the Stash operator creates a number (equal to the value of `.spec.target.replicas` of `RestoreSession` object) of Jobs to restore. Each of these Jobs requires a PVC to store the previously backed up data of the data directory `/var/lib/mysql` from the backend. Then each Job downloads the backed up data from the backend and injects into the associated PVC.

<figure align="center">
 <img alt="Percona XtraDB Cluster Restore Overview" src="/docs/v2020.09.16/images/addons/percona-xtradb/cluster_restore.png">
  <figcaption align="center">Fig: Percona XtraDB Cluster Restore</figcaption>
</figure>

## Next Steps

- Install Percona XtraDB addon for Stash following the guide from [here](/docs/v2020.09.16/addons/percona-xtradb/setup/install).
