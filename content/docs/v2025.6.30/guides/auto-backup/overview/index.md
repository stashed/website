---
title: Auto Backup Overview | Stash
description: An overview on how auto backup works in Stash.
menu:
  docs_v2025.6.30:
    identifier: auto-backup-overview
    name: What is Auto Backup?
    parent: auto-backup
    weight: 10
product_name: stash
menu_name: docs_v2025.6.30
section_menu_id: guides
info:
  cli: v0.40.0
  community: v0.40.0
  elasticsearch:
  - 5.6.4-v36
  - 6.2.4-v36
  - 6.3.0-v36
  - 6.4.0-v36
  - 6.5.3-v36
  - 6.8.0-v36
  - 7.14.0-v22
  - 7.2.0-v36
  - 7.3.2-v36
  - 8.2.0-v19
  enterprise: v0.40.0
  etcd:
  - 3.5.0-v23
  installer: v2025.6.30
  kubedump:
  - 0.2.0-v4
  mariadb:
  - 10.5.8-v30
  mongodb:
  - 3.4.17-v37
  - 3.4.22-v37
  - 3.6.13-v37
  - 3.6.8-v37
  - 4.0.11-v37
  - 4.0.3-v37
  - 4.0.5-v37
  - 4.1.13-v37
  - 4.1.4-v37
  - 4.1.7-v37
  - 4.2.3-v37
  - 4.4.6-v28
  - 5.0.15-v10
  - 5.0.3-v25
  - 6.0.5-v13
  mysql:
  - 5.7.25-v37
  - 8.0.14-v36
  - 8.0.21-v30
  - 8.0.3-v36
  nats:
  - 2.6.1-v24
  - 2.8.2-v19
  percona-xtradb:
  - 5.7-v31
  postgres:
  - 10.14-v35
  - 11.9-v35
  - 12.4-v35
  - 13.1-v32
  - 14.0-v24
  - 15.1-v16
  - 16.1-v5
  - 17.2-v3
  - 9.6.19-v35
  redis:
  - 5.0.13-v24
  - 6.2.5-v24
  - 7.0.5-v17
  ui-server: v0.21.0
  vault:
  - 1.10.3-v16
  version: v2025.6.30
---

# Auto Backup with Stash

Stash can be configured to automatically backup of any stateful workloads in your cluster. Stash enables cluster administrators to deploy backup blueprints ahead of time so that application owners can easily backup any types of workload with a few annotations. This allows Enterprises to stay prepared for disaster scenarios and recover from offsite secure backups in case of a disaster on public cloud and on-premises datacenters.

## What is Auto Backup

Stash uses 1-1 mapping among `Repository`, `BackupConfiguration` and the target. So, whenever you want to backup a target(workload/PVC/database), you have to create a `Repository` and `BackupConfiguration` object. This could become tiresome when you are trying to backup similar types of target and the `Repository` and `BackupConfiguration` have only a slight difference. To mitigate this problem, Stash provides a way to specify a blueprint for these two objects via `BackupBlueprint` crd. In Stash parlance, we call this process as **Auto Backup**.

You have to create only one `BackupBlueprint` for all similar types of target. For example, you need only one `BackupBlueprint` for Deployment, DaemonSet, StatefulSet etc. Similarly, you have to create only one `BackupBlueprint` for all PostgreSQL databases. Then, you just need to add some annotations in the target. Stash will automatically create respective `Repository` and `BackupConfiguration` objects using the blueprint and perform backups on pre-defined schedule.

## How Auto Backup Works?

The following diagram shows how automatic backup works in Stash. Open the image in a new tab to see the enlarged version.

<figure align="center">
  <img alt="Auto Backup Overview" src="images/auto_backup_overview.svg">
  <figcaption align="center">Fig: Auto Backup Overview</figcaption>
</figure>

The automatic backup process consists of the following steps:

1. A user creates a storage secret with necessary credentials of the backend where the backed up data will be stored.
2. Then, he creates a `BackupBlueprint` crd that specifies a blueprint for `Repository` and `BackupConfiguration` object.
3. Then, he creates a workload with some specific annotations for automatic backup.
4. Stash operator watches for workloads. When it finds a workload with annotations for automatic backup, it finds out the respective `BackupBlueprint`.
5. Then, Stash operator resolves the blueprint by replacing variable fields of the blueprint with respective information from the workload.
6. Then, it creates a `Repository` and a `BackupConfiguration` object for the workload according to the resolved blueprint.
7. Finally, Stash starts rest of the standard backup process as discussed in [here](/docs/v2025.6.30/guides/workloads/overview/).

> Note: `BackupBlueprint` is a non-namespaced crd. So, you can use a `BackupBlueprint` to backup targets in multiple namespaces. However, Storage Secret is a namespaced object. So, you have to manually create the secret in each namespace where you have a target for backup. Please give us your feedback on how to improve the ux of this aspect of Stash on [GitHub](https://github.com/stashed/stash/issues/842).

## Frequently Asked Question regarding Auto-backup

In this guide, we are going to answer some frequently asked questions regarding Stash auto-backup. If your query isn't answer here, feel free to open an issue [here](https://github.com/stashed/stash/issues).

#### What will happen if I remove the auto-backup annotations?

If you remove the auto-backup annotation, the respective `BackupConfiguration` will be deleted. If you are taking backup of workload volume, the respective sidecar will be removed and your workload will restart. The backup will stop immediately. If there is any running `BackupSession`, it will be deleted and the backup may terminate in incomplete state.

The respective `Repository` object will be preserved as well as the backed up data. If you add the auto-backup annotation again, the previous `Repository` will be used until you modify/change the respective `BackupBlueprint`.

#### What if different target require different runtime settings?

Currently, you can't pass runtime settings via annotation. So, if different target needs different runtime settings, you have to create separate `BackupBlueprint` for them.

#### What if I want to have different retention policy for different targets?

Currently, you can't pass retention policy via annotation. So, if different target needs different retention policy, you have to create separate `BackupBlueprint` for them.

## Next Step

- Learn how to configure automatic backup for workloads from [here](/docs/v2025.6.30/guides/auto-backup/workload/).
- Learn how to configure automatic backup for PVCs from [here](/docs/v2025.6.30/guides/auto-backup/pvc/).
- Learn how to configure automatic backup for databases from [here](/docs/v2025.6.30/guides/auto-backup/database/).
