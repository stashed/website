---
title: NATS Backup & Restore Overview | Stash
description: How NATS Backup & Restore Works in Stash
menu:
  docs_v2022.06.21:
    identifier: stash-nats-overview
    name: How does it work?
    parent: stash-nats
    weight: 10
product_name: stash
menu_name: docs_v2022.06.21
section_menu_id: stash-addons
info:
  cli: v0.21.0
  community: v0.21.0
  elasticsearch:
  - 5.6.4-v18
  - 6.2.4-v18
  - 6.3.0-v18
  - 6.4.0-v18
  - 6.5.3-v18
  - 6.8.0-v18
  - 7.14.0-v4
  - 7.2.0-v18
  - 7.3.2-v18
  - 8.2.0-v1
  enterprise: v0.21.0
  etcd:
  - 3.5.0-v5
  installer: v2022.06.21
  kubedump:
  - 0.1.0-v1
  mariadb:
  - 10.5.8-v11
  mongodb:
  - 3.4.17-v17
  - 3.4.22-v17
  - 3.6.13-v17
  - 3.6.8-v17
  - 4.0.11-v17
  - 4.0.3-v17
  - 4.0.5-v17
  - 4.1.13-v17
  - 4.1.4-v17
  - 4.1.7-v17
  - 4.2.3-v17
  - 4.4.6-v8
  - 5.0.3-v5
  mysql:
  - 5.7.25-v18
  - 8.0.14-v18
  - 8.0.21-v12
  - 8.0.3-v18
  nats:
  - 2.6.1-v6
  - 2.8.2-v1
  percona-xtradb:
  - 5.7-v13
  postgres:
  - 10.14-v16
  - 11.9-v16
  - 12.4-v16
  - 13.1-v13
  - 14.0-v5
  - 9.6.19-v16
  redis:
  - 5.0.13-v6
  - 6.2.5-v6
  ui-server: v0.3.0
  version: v2022.06.21
---

{{< notice type="warning" message="This is an Enterprise-only feature. Please install [Stash Enterprise Edition](/docs/v2022.06.21/setup/install/enterprise) to try this feature." >}}

# How Stash Backups & Restores NATS Streams

Stash `{{< param "info.version" >}}` supports backup and restore operation of NATS streams. This guide will give you an overview of how NATS stream backup and restore process works in Stash.

## How Backup Works

The following diagram shows how Stash takes a backup of NATS streams. Open the image in a new tab to see the enlarged version.

<figure align="center">
 <img alt="NATS Backup Overview" src="/docs/v2022.06.21/addons/nats/overview/images/backup_overview.svg">
  <figcaption align="center">Fig: NATS Backup Overview</figcaption>
</figure>

The backup process consists of the following steps:

1. At first, a user creates a secret with access credentials of the backend where the backed up data will be stored.

2. Then, she creates a `Repository` crd that specifies the backend information along with the secret that holds the credentials to access the backend.

3. Then, she creates a `BackupConfiguration` crd targeting the [AppBinding](/docs/v2022.06.21/concepts/crds/appbinding) crd of the respective NATS server. The `BackupConfiguration` object also specifies the `Task` to use to backup the NATS streams.

4. Stash operator watches for `BackupConfiguration` crd.

5. Once Stash operator finds a `BackupConfiguration` crd, it creates a CronJob with the schedule specified in `BackupConfiguration` object to trigger backup periodically.

6. On the next scheduled slot, the CronJob triggers a backup by creating a `BackupSession` crd.

7. Stash operator also watches for `BackupSession` crd.

8. When it finds a `BackupSession` object, it resolves the respective `Task` and `Function` and prepares a Job definition to backup.

9. Then, it creates the Job to backup the targeted NATS server.

10. The backup Job reads necessary information to connect with the NATS server from the `AppBinding` crd. It also reads backend information and access credentials from `Repository` crd and Storage Secret respectively.

11. Then, the Job dumps the targeted streams and uploads the output to the backend. Stash stores the dumped files temporarily before uploading into the backend. Hence, you should provide a PVC template using `spec.interimVolumeTemplate` field of `BackupConfiguration` crd to use to store those dumped files temporarily. Make sure that the provided PVC size is capable of storing all (or, specified) the NATS streams.

12. Finally, when the backup is completed, the Job sends Prometheus metrics to the Pushgateway running inside Stash operator pod. It also updates the `BackupSession` and `Repository` status to reflect the backup procedure.

## How Restore Process Works

The following diagram shows how Stash restores backed up data into a NATS streaming server. Open the image in a new tab to see the enlarged version.

<figure align="center">
 <img alt="NATS Restore Overview" src="/docs/v2022.06.21/addons/nats/overview/images/restore_overview.svg">
  <figcaption align="center">Fig: NATS Restore Process</figcaption>
</figure>

The restore process consists of the following steps:

1. At first, a user creates a `RestoreSession` crd targeting the `AppBinding` of the desired NATS server where the backed up data will be restored. It also specifies the `Repository` crd which holds the backend information and the `Task` to use to restore the target.

2. Stash operator watches for `RestoreSession` object.

3. Once it finds a `RestoreSession` object, it resolves the respective `Task` and `Function` and prepares a Job definition to restore.

4. Then, it creates the Job to restore the target.

5. The Job reads necessary information to connect with the NATS server from respective `AppBinding` crd. It also reads backend information and access credentials from `Repository` crd and Storage Secret respectively.

6. Then, the job downloads the backed up data from the backend and restore the streams. Stash stores the downloaded files temporarily before inserting into the targeted NATS server. Hence, you should provide a PVC template using `spec.interimVolumeTemplate` field of `RestoreSession` crd to use to store those restored files temporarily. Make sure that the provided PVC size is capable of storing all the backed up NATS streams.

7. Finally, when the restore process is completed, the Job sends Prometheus metrics to the Pushgateway and update the `RestoreSession` status to reflect restore completion.

## Next Steps

- Backup your NATS using Stash following the guide from [here](/docs/v2022.06.21/addons/nats/helm/).