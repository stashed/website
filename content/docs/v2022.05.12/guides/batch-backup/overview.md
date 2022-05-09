---
title: Batch Backup & Restore Overview | Stash
description: An overview on how batch backup & restore works in Stash.
menu:
  docs_v2022.05.12:
    identifier: batch-backup-overview
    name: How Batch Backup & Restore works?
    parent: batch-backup
    weight: 10
product_name: stash
menu_name: docs_v2022.05.12
section_menu_id: guides
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

# Batch Backup and Restore Overview

Sometimes, an application may consist of multiple co-related components. For example, to deploy a WordPress, you will need a Deployment for the WordPress and another Deployment for the database. Now, it is sensible to want to backup or restore both of the deployments using a single configuration as they are parts of the same application.

Stash 0.9.0+ supports taking backup multiple co-related components using a single configuration known as [BackupBatch](/docs/v2022.05.12/concepts/crds/backupbatch). Stash 0.10.0+ supports restoring multiple co-related components together known as [RestoreBatch](/docs/v2022.05.12/concepts/crds/restorebatch) This guide will give you an overview of how batch backup and restore works in Stash.

## How Batch Backup Works

The following diagram shows how Stash takes backup of multiple co-related components in a single application. Open the image in a new tab to see the enlarged version.

<figure align="center">
  <img alt="Stash Batch Backup Flow" src="/docs/v2022.05.12/images/guides/batch-backup/batchbackup_overview.svg">
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
  <img alt="Stash Batch Backup Flow" src="/docs/v2022.05.12/images/guides/batch-backup/batch-restore.svg">
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

- See a step by step guide to backup application with multiple co-related components [here](/docs/v2022.05.12/guides/batch-backup/batch-backup).
