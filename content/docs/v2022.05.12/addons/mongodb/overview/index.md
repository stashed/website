---
title: MongoDB Backup Overview | Stash
description: How MongoDB Backup Works in Stash
menu:
  docs_v2022.05.12:
    identifier: stash-mongodb-overview
    name: How does it work?
    parent: stash-mongodb
    weight: 10
product_name: stash
menu_name: docs_v2022.05.12
section_menu_id: stash-addons
info:
  cli: v0.20.0
  community: v0.20.0
  elasticsearch:
  - 5.6.4-v17
  - 6.2.4-v17
  - 6.3.0-v17
  - 6.4.0-v17
  - 6.5.3-v17
  - 6.8.0-v17
  - 7.14.0-v3
  - 7.2.0-v17
  - 7.3.2-v17
  enterprise: v0.20.0
  etcd:
  - 3.5.0-v4
  installer: v2022.05.12
  kubedump:
  - 0.1.0
  mariadb:
  - 10.5.8-v10
  mongodb:
  - 3.4.17-v16
  - 3.4.22-v16
  - 3.6.13-v16
  - 3.6.8-v16
  - 4.0.11-v16
  - 4.0.3-v16
  - 4.0.5-v16
  - 4.1.13-v16
  - 4.1.4-v16
  - 4.1.7-v16
  - 4.2.3-v16
  - 4.4.6-v7
  - 5.0.3-v4
  mysql:
  - 5.7.25-v17
  - 8.0.14-v17
  - 8.0.21-v11
  - 8.0.3-v17
  nats:
  - 2.6.1-v4
  percona-xtradb:
  - 5.7-v12
  postgres:
  - 10.14-v15
  - 11.9-v15
  - 12.4-v15
  - 13.1-v12
  - 14.0-v4
  - 9.6.19-v15
  redis:
  - 5.0.13-v5
  - 6.2.5-v5
  ui-server: v0.2.0
  version: v2022.05.12
---

{{< notice type="warning" message="This is an Enterprise-only feature. Please install [Stash Enterprise Edition](/docs/v2022.05.12/setup/install/enterprise) to try this feature." >}}

# How Stash Backup & Restore MongoDB Database

Stash 0.9.0+ supports backup and restore operation of many databases. This guide will give you an overview of how MongoDB database backup and restore process works in Stash.

## How Backup Works

The following diagram shows how Stash takes backup of a MongoDB database. Open the image in a new tab to see the enlarged version.

<figure align="center">
 <img alt="MongoDB Backup Overview" src="/docs/v2022.05.12/addons/mongodb/overview/images/backup_overview.svg">
  <figcaption align="center">Fig: MongoDB Backup Overview</figcaption>
</figure>

The backup process consists of the following steps:

1. At first, a user creates a secret with access credentials of the backend where the backed up data will be stored.

2. Then, she creates a `Repository` crd that specifies the backend information along with the secret that holds the credentials to access the backend.

3. Then, she creates a `BackupConfiguration` crd targeting the [AppBinding](/docs/v2022.05.12/concepts/crds/appbinding) crd of the desired database. The `BackupConfiguration` object also specifies the `Task` to use to backup the database.

4. Stash operator watches for `BackupConfiguration` crd.

5. Once Stash operator finds a `BackupConfiguration` crd, it creates a CronJob with the schedule specified in `BackupConfiguration` object to trigger backup periodically.

6. On the next scheduled slot, the CronJob triggers a backup by creating a `BackupSession` crd.

7. Stash operator also watches for `BackupSession` crd.

8. When it finds a `BackupSession` object, it resolves the respective `Task` and `Function` and prepares a Job definition to backup.

9. Then, it creates the Job to backup the targeted database.

10. The backup Job reads necessary information to connect with the database from the `AppBinding` crd. It also reads backend information and access credentials from `Repository` crd and Storage Secret respectively.

11. Then, the Job dumps the targeted database and uploads the output to the backend. Stash pipes the output of dump command to uploading process. Hence, backup Job does not require a large volume to hold the entire dump output.

12. Finally, when the backup is complete, the Job sends Prometheus metrics to the Pushgateway running inside Stash operator pod. It also updates the `BackupSession` and `Repository` status to reflect the backup procedure.

### Backup Different MongoDB Configurations

This section will show you how backup works for different MongoDB configurations.

#### Standalone MongoDB

For a standalone MongoDB database, the backup job directly dumps the database using `mongodump` and pipe the output to the backup process.

<figure align="center">
 <img alt="Standalone MongoDB Backup Overview" src="/docs/v2022.05.12/addons/mongodb/overview/images/standalone_backup.svg">
  <figcaption align="center">Fig: Standalone MongoDB Backup</figcaption>
</figure>

#### MongoDB ReplicaSet Cluster

For MongoDB ReplicaSet cluster, Stash takes backup from one of the secondary replicas. The backup process consists of the following steps:

1. Identify a secondary replica.
2. Lock the secondary replica.
3. Backup the secondary replica.
4. Unlock the secondary replica.

<figure align="center">
 <img alt="MongoDB ReplicaSet Cluster Backup Overview" src="/docs/v2022.05.12/addons/mongodb/overview/images/replicaset_backup.svg">
  <figcaption align="center">Fig: MongoDB ReplicaSet Cluster Backup</figcaption>
</figure>

#### MongoDB Sharded Cluster

For MongoDB sharded cluster, Stash takes backup of the individual shards as well as the config server. Stash takes backup from a secondary replica of the shards and the config server. If there is no secondary replica then Stash will take backup from the primary replica. The backup process consists of the following steps:

1. Disable balancer.
2. Lock config server.
3. Identify a secondary replica for each shard.
4. Lock the secondary replica.
5. Run backup on the secondary replica.
6. Unlock the secondary replica.
7. Unlock config server.
8. Enable balancer.

<figure align="center">
 <img alt="MongoDB Sharded Cluster Backup Overview" src="/docs/v2022.05.12/addons/mongodb/overview/images/sharded_backup.svg">
  <figcaption align="center">Fig: MongoDB Sharded Cluster Backup</figcaption>
</figure>

## How Restore Process Works

The following diagram shows how Stash restores backed up data into a MongoDB database. Open the image in a new tab to see the enlarged version.

<figure align="center">
 <img alt="Database Restore Overview" src="/docs/v2022.05.12/addons/mongodb/overview/images/restore_overview.svg">
  <figcaption align="center">Fig: MongoDB Restore Process Overview</figcaption>
</figure>

The restore process consists of the following steps:

1. At first, a user creates a `RestoreSession` crd targeting the `AppBinding` of the desired database where the backed up data will be restored. It also specifies the `Repository` crd which holds the backend information and the `Task` to use to restore the target.

2. Stash operator watches for `RestoreSession` object.

3. Once it finds a `RestoreSession` object, it resolves the respective `Task` and `Function` and prepares a Job definition to restore.

4. Then, it creates the Job to restore the target.

5. The Job reads necessary information to connect with the database from respective `AppBinding` crd. It also reads backend information and access credentials from `Repository` crd and Storage Secret respectively.

6. Then, the job downloads the backed up data from the backend and injects into the desired database. Stash pipes the downloaded data to the respective database tool to inject into the database. Hence, restore job does not require a large volume to download entire backup data inside it.

7. Finally, when the restore process is complete, the Job sends Prometheus metrics to the Pushgateway and update the `RestoreSession` status to reflect restore completion.

### Restoring Different MongoDB Configurations

This section will show you restore process works for different MongoDB configurations.

#### Standalone MongoDB

For a standalone MongoDB database, the restore job downloads the backed up data from the backend and pipe the downloaded data to `mongorestore` command which inserts the data into the desired MongoDB database.

<figure align="center">
 <img alt="Standalone MongoDB Restore Overview" src="/docs/v2022.05.12/addons/mongodb/overview/images/standalone_restore.svg">
  <figcaption align="center">Fig: Standalone MongoDB Restore</figcaption>
</figure>

#### MongoDB ReplicaSet Cluster

For MongoDB ReplicaSet cluster, Stash identifies the primary replica and restore into it.

<figure align="center">
 <img alt="MongoDB ReplicaSet Cluster Restore Overview" src="/docs/v2022.05.12/addons/mongodb/overview/images/replicaset_restore.svg">
  <figcaption align="center">Fig: MongoDB ReplicaSet Cluster Restore</figcaption>
</figure>

#### MongoDB Sharded Cluster

For MongoDB sharded cluster, Stash identifies the primary replica of each shard as well as the config server and restore respective backed up data into them.

<figure align="center">
 <img alt="MongoDB Sharded Cluster Restore" src="/docs/v2022.05.12/addons/mongodb/overview/images/sharded_restore.svg">
  <figcaption align="center">Fig: MongoDB Sharded Cluster Restore</figcaption>
</figure>

## Next Steps

- Backup your standalone MongoDB database using Stash following the guide from [here](/docs/v2022.05.12/addons/mongodb/standalone/).
- Backup your MongoDB Replicaset using Stash following the guide from [here](/docs/v2022.05.12/addons/mongodb/replicaset/).
- Backup your sharded MongoDB cluster using Stash following the guide from [here](/docs/v2022.05.12/addons/mongodb/sharding/).
