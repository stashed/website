---
title: Batch Backup & Restore Overview | Stash
description: An overview on how batch backup & restore works in Stash.
menu:
  docs_v2025.10.17:
    identifier: batch-backup-overview
    name: How Batch Backup & Restore works?
    parent: batch-backup
    weight: 10
product_name: stash
menu_name: docs_v2025.10.17
section_menu_id: guides
info:
  cli: v0.42.0
  community: v0.42.0
  elasticsearch:
  - 5.6.4-v38
  - 6.2.4-v38
  - 6.3.0-v38
  - 6.4.0-v38
  - 6.5.3-v38
  - 6.8.0-v38
  - 7.14.0-v24
  - 7.2.0-v38
  - 7.3.2-v38
  - 8.2.0-v21
  enterprise: v0.42.0
  etcd:
  - 3.5.0-v25
  installer: v2025.10.17
  kubedump:
  - 0.2.0-v6
  mariadb:
  - 10.6.23-v1
  mongodb:
  - 3.4.17-v39
  - 3.4.22-v39
  - 3.6.13-v39
  - 3.6.8-v39
  - 4.0.11-v39
  - 4.0.3-v39
  - 4.0.5-v39
  - 4.1.13-v39
  - 4.1.4-v39
  - 4.1.7-v39
  - 4.2.3-v39
  - 4.4.6-v30
  - 5.0.15-v12
  - 5.0.3-v27
  - 6.0.5-v15
  mysql:
  - 5.7.25-v39
  - 8.0.14-v38
  - 8.0.21-v32
  - 8.0.3-v38
  nats:
  - 2.6.1-v26
  - 2.8.2-v21
  percona-xtradb:
  - 5.7-v33
  postgres:
  - 10.14-v37
  - 11.9-v37
  - 12.4-v37
  - 13.1-v34
  - 14.0-v26
  - 15.1-v18
  - 16.1-v7
  - 17.2-v5
  - 9.6.19-v37
  redis:
  - 5.0.13-v26
  - 6.2.5-v26
  - 7.0.5-v19
  ui-server: v0.23.0
  vault:
  - 1.10.3-v18
  version: v2025.10.17
---

# Batch Backup and Restore Overview

Sometimes, an application may consist of multiple co-related components. For example, to deploy a WordPress, you will need a Deployment for the WordPress and another Deployment for the database. Now, it is sensible to want to backup or restore both of the deployments using a single configuration as they are parts of the same application.

Stash 0.9.0+ supports taking backup multiple co-related components using a single configuration known as [BackupBatch](/docs/v2025.10.17/concepts/crds/backupbatch/). Stash 0.10.0+ supports restoring multiple co-related components together known as [RestoreBatch](/docs/v2025.10.17/concepts/crds/restorebatch/) This guide will give you an overview of how batch backup and restore works in Stash.

## How Batch Backup Works

The following diagram shows how Stash takes backup of multiple co-related components in a single application. Open the image in a new tab to see the enlarged version.

<figure align="center">
  <img alt="Stash Batch Backup Flow" src="images/batchbackup_overview.svg">
<figcaption align="center">Fig: Batch backup flow in Stash</figcaption>
</figure>

The backup process consists of the following steps:

1. At first, a user creates a backend Secret. This secret holds the credentials to access the backend where the backed up data will be stored.

2. Then, she creates a `Repository` crd which represents the original repository in the backend.

3. Then, she creates a `BackupBatch` crd which specifies multiple targets(workload, volume, and database). It also specifies the `Repository` object that holds the backend information where the backed up data will be stored.

4. Stash operator watches for `BackupBatch` objects.

5. When it finds a `BackupBatch` object, it checks if there is any workload as a target. If there any, it injects a sidecar named `stash` into the workloads.

6. It also creates a `CronJob` to trigger backups periodically.

7. The`CronJob` triggers backup on each scheduled slot by creating a `BackupSession` crd.

8. The BackupSession controller (inside sidecar for sidecar model or inside the operator itself for job model) watches for `BackupSession` crd.

9. When it finds a `BackupSession` it starts the backup process immediately(for job model a job is created for taking backup) for the individual targets. Stash operator enforces the backup order if the `executionOrder` is set to `Sequential`.

10. The individual targets complete their backup process independently and update their respective fields in `BackupSession` status.

## How Batch Restore Works

The following diagram shows the batch restore process. Please, open image in new tab to view the enlarged image.

<figure align="center">
  <img alt="Stash Batch Restore Flow" src="images/batch-restore.svg">
<figcaption align="center">Fig: Batch restore flow in Stash</figcaption>
</figure>

The batch restore process consists of the following steps:

1. At first, the user creates a `RestoreBatch` CR specifying the targets and the respective Repository where the backed up data has been stored.
2. The Stash operator watches for the `RestoreBatch` CR.
3. When the Stash operator finds a `RestoreBatch` CR, it executes the global `PreRestore` hooks. If there is no global `PreRestore` hook, Stash will skip this step.
4. Then, it injects an init-container into the target that follows the sidecar model and creates a restore job for the targets that follow the job model. Stash operator enforces the restore order in this step if the `executionOrder` is set to `Sequential`.
5. The restore init-container/job first execute their local `PreRestore` hooks. Then, restore their data and finally execute their `PostRestore` hooks.
6. Finally, Stash operator executes the global `PostRestore` hooks. If there is not global `PostRestore` hook configured for this RestoreBatch, Stash will skip this step.

## Next Steps

- See a step by step guide to backup application with multiple co-related components [here](/docs/v2025.10.17/guides/batch-backup/wordpress-backup/).
