---
title: Elasticsearch Backup Overview | Stash
description: How Elasticsearch Backup Works in Stash
menu:
  docs_v2022.03.29:
    identifier: stash-elasticsearch-overview
    name: How does it work?
    parent: stash-elasticsearch
    weight: 10
product_name: stash
menu_name: docs_v2022.03.29
section_menu_id: stash-addons
info:
  cli: v0.19.0
  community: v0.19.0
  elasticsearch:
  - 5.6.4-v16
  - 6.2.4-v16
  - 6.3.0-v16
  - 6.4.0-v16
  - 6.5.3-v16
  - 6.8.0-v16
  - 7.14.0-v2
  - 7.2.0-v16
  - 7.3.2-v16
  enterprise: v0.19.0
  etcd:
  - 3.5.0-v3
  installer: v2022.03.29
  mariadb:
  - 10.5.8-v9
  mongodb:
  - 3.4.17-v15
  - 3.4.22-v15
  - 3.6.13-v15
  - 3.6.8-v15
  - 4.0.11-v15
  - 4.0.3-v15
  - 4.0.5-v15
  - 4.1.13-v15
  - 4.1.4-v15
  - 4.1.7-v15
  - 4.2.3-v15
  - 4.4.6-v6
  - 5.0.3-v3
  mysql:
  - 5.7.25-v16
  - 8.0.14-v16
  - 8.0.21-v10
  - 8.0.3-v16
  nats:
  - 2.6.1-v3
  percona-xtradb:
  - 5.7-v11
  postgres:
  - 10.14-v14
  - 11.9-v14
  - 12.4-v14
  - 13.1-v11
  - 14.0-v3
  - 9.6.19-v14
  redis:
  - 5.0.13-v4
  - 6.2.5-v4
  ui-server: v0.2.0
  version: v2022.03.29
---

{{< notice type="warning" message="This is an Enterprise-only feature. Please install [Stash Enterprise Edition](/docs/v2022.03.29/setup/install/enterprise) to try this feature." >}}

# How Stash Backup & Restore Elasticsearch Database

Stash 0.9.0+ supports backup and restore operation of many databases. This guide will give you an overview of how Elasticsearch database backup and restore process works in Stash.

## How Backup Works

The following diagram shows how Stash takes a backup of an Elasticsearch database. Open the image in a new tab to see the enlarged version.

<figure align="center">
 <img alt="Elasticsearch Backup Overview" src="/docs/v2022.03.29/addons/elasticsearch/overview/images/backup_overview.svg">
  <figcaption align="center">Fig: Elasticsearch Backup Overview</figcaption>
</figure>

The backup process consists of the following steps:

1. At first, a user creates a secret with access credentials of the backend where the backed up data will be stored.

2. Then, she creates a `Repository` crd that specifies the backend information along with the secret that holds the credentials to access the backend.

3. Then, she creates a `BackupConfiguration` crd targeting the [AppBinding](/docs/v2022.03.29/concepts/crds/appbinding) crd of the desired database. The `BackupConfiguration` object also specifies the `Task` to use to backup the database.

4. Stash operator watches for `BackupConfiguration` crd.

5. Once Stash operator finds a `BackupConfiguration` crd, it creates a CronJob with the schedule specified in `BackupConfiguration` object to trigger backup periodically.

6. On the next scheduled slot, the CronJob triggers a backup by creating a `BackupSession` crd.

7. Stash operator also watches for `BackupSession` crd.

8. When it finds a `BackupSession` object, it resolves the respective `Task` and `Function` and prepares a Job definition to backup.

9. Then, it creates the Job to backup the targeted database.

10. The backup Job reads necessary information to connect with the database from the `AppBinding` crd. It also reads backend information and access credentials from `Repository` crd and Storage Secret respectively.

11. Then, the Job dumps the targeted database and uploads the output to the backend. Stash stores the dumped files temporarily before uploading into the backend. Hence, you should provide a PVC template using `spec.interimVolumeTemplate` field of `BackupConfiguration` crd to use to store those dumped files temporarily.

12. Finally, when the backup is completed, the Job sends Prometheus metrics to the Pushgateway running inside Stash operator pod. It also updates the `BackupSession` and `Repository` status to reflect the backup procedure.

## How Restore Process Works

The following diagram shows how Stash restores backed up data into an Elasticsearch database. Open the image in a new tab to see the enlarged version.

<figure align="center">
 <img alt="Database Restore Overview" src="/docs/v2022.03.29/addons/elasticsearch/overview/images/restore_overview.svg">
  <figcaption align="center">Fig: Elasticsearch Restore Process</figcaption>
</figure>

The restore process consists of the following steps:

1. At first, a user creates a `RestoreSession` crd targeting the `AppBinding` of the desired database where the backed up data will be restored. It also specifies the `Repository` crd which holds the backend information and the `Task` to use to restore the target.

2. Stash operator watches for `RestoreSession` object.

3. Once it finds a `RestoreSession` object, it resolves the respective `Task` and `Function` and prepares a Job definition to restore.

4. Then, it creates the Job to restore the target.

5. The Job reads necessary information to connect with the database from respective `AppBinding` crd. It also reads backend information and access credentials from `Repository` crd and Storage Secret respectively.

6. Then, the job downloads the backed up data from the backend and insert into the desired database. Stash stores the downloaded files temporarily before inserting into the targeted database. Hence, you should provide a PVC template using `spec.interimVolumeTemplate` field of `RestoreSession` crd to use to store those restored files temporarily.

7. Finally, when the restore process is completed, the Job sends Prometheus metrics to the Pushgateway and update the `RestoreSession` status to reflect restore completion.

## Next Steps

- Backup your Elasticsearch databases using Stash by following the guide from [here](/docs/v2022.03.29/addons/elasticsearch/kubedb/).
