---
title: MariaDB Backup Customization | Stash
description: Customizing MariaDB Backup and Restore process with Stash
menu:
  docs_v2024.9.30:
    identifier: stash-mariadb-customization
    name: Customizing Backup & Restore Process
    parent: stash-mariadb
    weight: 40
product_name: stash
menu_name: docs_v2024.9.30
section_menu_id: stash-addons
info:
  cli: v0.36.0
  community: v0.36.0
  elasticsearch:
  - 5.6.4-v32
  - 6.2.4-v32
  - 6.3.0-v32
  - 6.4.0-v32
  - 6.5.3-v32
  - 6.8.0-v32
  - 7.14.0-v18
  - 7.2.0-v32
  - 7.3.2-v32
  - 8.2.0-v15
  enterprise: v0.36.0
  etcd:
  - 3.5.0-v19
  installer: v2024.9.30
  kubedump:
  - 0.1.0-v15
  mariadb:
  - 10.5.8-v26
  mongodb:
  - 3.4.17-v33
  - 3.4.22-v33
  - 3.6.13-v33
  - 3.6.8-v33
  - 4.0.11-v33
  - 4.0.3-v33
  - 4.0.5-v33
  - 4.1.13-v33
  - 4.1.4-v33
  - 4.1.7-v33
  - 4.2.3-v33
  - 4.4.6-v24
  - 5.0.15-v6
  - 5.0.3-v21
  - 6.0.5-v9
  mysql:
  - 5.7.25-v32
  - 8.0.14-v32
  - 8.0.21-v26
  - 8.0.3-v32
  nats:
  - 2.6.1-v20
  - 2.8.2-v15
  percona-xtradb:
  - 5.7-v27
  postgres:
  - 10.14-v31
  - 11.9-v31
  - 12.4-v31
  - 13.1-v28
  - 14.0-v20
  - 15.1-v12
  - 16.1-v1
  - 9.6.19-v31
  redis:
  - 5.0.13-v20
  - 6.2.5-v20
  - 7.0.5-v13
  ui-server: v0.17.0
  vault:
  - 1.10.3-v12
  version: v2024.9.30
---

# Customizing Backup and Restore Process

Stash provides rich customization supports for the backup and restore process to meet the requirements of various cluster configurations. This guide will show you some examples of these customizations.

> Note: YAML files used in this tutorial are stored [here](https://github.com/stashed/docs/tree/{{< param "info.version" >}}/docs/addons/mariadb/customization/examples).

## Customizing Backup Process

In this section, we are going to show you how to customize the backup process. Here, we are going to show some examples of providing arguments to the backup process, running the backup process as a specific user, ignoring some indexes during the backup process, etc.

### Passing arguments to the backup process

Stash MariaDB addon uses [mysqldump](https://mariadb.com/kb/en/mysqldump) for backup. You can pass arguments to the `mysqldump` through `args` param under `task.params` section.

The below example shows how you can pass the `--databases testdb` to take backup for a specific mariadb databases named `testdb`.

```yaml
apiVersion: stash.appscode.com/v1beta1
kind: BackupConfiguration
metadata:
  name: sample-mariadb-backup
  namespace: demo
spec:
  schedule: "*/5 * * * *"
  task:
    name: mariadb-backup-10.5.8
    params:
    - name: args
      value: --databases testdb
  repository:
    name: gcs-repo
  target:
    ref:
      apiVersion: appcatalog.appscode.com/v1alpha1
      kind: AppBinding
      name: sample-mariadb
  retentionPolicy:
    name: keep-last-5
    keepLast: 5
    prune: true
```

> **WARNING**: Make sure that you have the specific database created before taking backup. In this case, Database `testdb` should exist before the backup job starts.

### Running backup job as a specific user

If your cluster requires running the backup job as a specific user, you can provide `securityContext` under `runtimeSettings.pod` section. The below example shows how you can run the backup job as the root user.

```yaml
apiVersion: stash.appscode.com/v1beta1
kind: BackupConfiguration
metadata:
  name: sample-mariadb-backup
  namespace: demo
spec:
  schedule: "*/2 * * * *"
  task:
    name: mariadb-backup-10.5.8
  repository:
    name: gcs-repo
  target:
    ref:
      apiVersion: appcatalog.appscode.com/v1alpha1
      kind: AppBinding
      name: sample-mariadb
  runtimeSettings:
    pod:
      securityContext:
        runAsUser: 0
        runAsGroup: 0
  retentionPolicy:
    name: keep-last-5
    keepLast: 5
    prune: true
```

### Specifying Memory/CPU limit/request for the backup job

If you want to specify the Memory/CPU limit/request for your backup job, you can specify `resources` field under `runtimeSettings.container` section.

```yaml
apiVersion: stash.appscode.com/v1beta1
kind: BackupConfiguration
metadata:
  name: sample-mariadb-backup
  namespace: demo
spec:
  schedule: "*/2 * * * *"
  task:
    name: mariadb-backup-10.5.8
  repository:
    name: gcs-repo
  target:
    ref:
      apiVersion: appcatalog.appscode.com/v1alpha1
      kind: AppBinding
      name: sample-mariadb
  runtimeSettings:
    container:
      resources:
        requests:
          cpu: "200m"
          memory: "1Gi"
        limits:
          cpu: "200m"
          memory: "1Gi"
  retentionPolicy:
    name: keep-last-5
    keepLast: 5
    prune: true
```

### Using multiple retention policies

You can also specify multiple retention policies for your backed up data. For example, you may want to keep few daily snapshots, few weekly snapshots, and few monthly snapshots, etc. You just need to pass the desired number with the respective key under the `retentionPolicy` section.

```yaml
apiVersion: stash.appscode.com/v1beta1
kind: BackupConfiguration
metadata:
  name: sample-mariadb-backup
  namespace: demo
spec:
  schedule: "*/5 * * * *"
  task:
    name: mariadb-backup-10.5.8
  repository:
    name: gcs-repo
  target:
    ref:
      apiVersion: appcatalog.appscode.com/v1alpha1
      kind: AppBinding
      name: sample-mariadb
  retentionPolicy:
    name: sample-mariadb-retention
    keepLast: 5
    keepDaily: 10
    keepWeekly: 20
    keepMonthly: 50
    keepYearly: 100
    prune: true
```

To know more about the available options for retention policies, please visit [here](/docs/v2024.9.30/concepts/crds/backupconfiguration/#specretentionpolicy).

## Customizing Restore Process

Stash also uses `mysql` during the restore process. In this section, we are going to show how you can pass arguments to the restore process, restore a specific snapshot, run restore job as a specific user, etc.

### Passing arguments to the restore process

Similar to the backup process, you can pass arguments to the restore process through the `args` params under `task.params` section. This example will restore data from database `testdb` only.

```yaml
apiVersion: stash.appscode.com/v1beta1
kind: RestoreSession
metadata:
  name: sample-mariadb-restore
  namespace: demo
  labels:
    app.kubernetes.io/name: mariadb.kubedb.com
spec:
  task:
    name: mariadb-restore-10.5.8
    params:
    - name: args
      value: --one-database testdb
  repository:
    name: gcs-repo
  target:
    ref:
      apiVersion: appcatalog.appscode.com/v1alpha1
      kind: AppBinding
      name: sample-mariadb
  rules:
  - snapshots: [latest]
```

### Restore specific snapshot

You can also restore a specific snapshot. At first, list the available snapshots as below,

```bash
❯ kubectl get snapshots -n demo
NAME                ID         REPOSITORY   HOSTNAME   CREATED AT
gcs-repo-4bc21d6f   4bc21d6f   gcs-repo     host-0     2022-01-12T14:54:27Z
gcs-repo-f0ac7cbd   f0ac7cbd   gcs-repo     host-0     2022-01-12T14:56:26Z
gcs-repo-9210ebb6   9210ebb6   gcs-repo     host-0     2022-01-12T14:58:27Z
gcs-repo-0aff8890   0aff8890   gcs-repo     host-0     2022-01-12T15:00:28Z
```

>You can also filter the snapshots as shown in the guide [here](https://stash.run/docs/{{< param "info.version" >}}/concepts/crds/snapshot/#working-with-snapshot).

You can use the respective ID of the snapshot to restore a specific snapshot.

The below example shows how you can pass a specific snapshot ID through the `snapshots` field of `rules` section.

```yaml
apiVersion: stash.appscode.com/v1beta1
kind: RestoreSession
metadata:
  name: sample-mariadb-restore
  namespace: demo
  labels:
    app.kubernetes.io/name: mariadb.kubedb.com
spec:
  task:
    name: mariadb-restore-10.5.8
  repository:
    name: gcs-repo
  target:
    ref:
      apiVersion: appcatalog.appscode.com/v1alpha1
      kind: AppBinding
      name: sample-mariadb
  rules:
  - snapshots: [4bc21d6f]
```

>Please, do not specify multiple snapshots here. Each snapshot represents a complete backup of your database. Multiple snapshots are only usable during file/directory restore.

### Running restore job as a specific user

You can provide `securityContext` under `runtimeSettings.pod` section to run the restore job as a specific user.

```yaml
apiVersion: stash.appscode.com/v1beta1
kind: RestoreSession
metadata:
  name: sample-mariadb-restore
  namespace: demo
  labels:
    app.kubernetes.io/name: mariadb.kubedb.com
spec:
  task:
    name: mariadb-restore-10.5.8
  repository:
    name: gcs-repo
  target:
    ref:
      apiVersion: appcatalog.appscode.com/v1alpha1
      kind: AppBinding
      name: sample-mariadb
  runtimeSettings:
    pod:
      securityContext:
        runAsUser: 0
        runAsGroup: 0
  rules:
  - snapshots: [latest]
```

### Specifying Memory/CPU limit/request for the restore job

Similar to the backup process, you can also provide `resources` field under the `runtimeSettings.container` section to limit the Memory/CPU for your restore job.

```yaml
apiVersion: stash.appscode.com/v1beta1
kind: RestoreSession
metadata:
  name: sample-mariadb-restore
  namespace: demo
  labels:
    app.kubernetes.io/name: mariadb.kubedb.com
spec:
  task:
    name: mariadb-restore-10.5.8
  repository:
    name: gcs-repo
  target:
    ref:
      apiVersion: appcatalog.appscode.com/v1alpha1
      kind: AppBinding
      name: sample-mariadb
  runtimeSettings:
    container:
      resources:
        requests:
          cpu: "200m"
          memory: "1Gi"
        limits:
          cpu: "200m"
          memory: "1Gi"
  rules:
  - snapshots: [latest]
```
