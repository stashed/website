---
title: Backup/Restore Customization | Stash
description: A guide on how to customize backup/restore process in Stash.
menu:
  docs_v2024.12.18:
    identifier: use-cases-customize-backup-restore
    name: Customize Backup/Restore
    parent: use-cases
    weight: 70
product_name: stash
menu_name: docs_v2024.12.18
section_menu_id: guides
info:
  cli: v0.37.0
  community: v0.37.0
  elasticsearch:
  - 5.6.4-v33
  - 6.2.4-v33
  - 6.3.0-v33
  - 6.4.0-v33
  - 6.5.3-v33
  - 6.8.0-v33
  - 7.14.0-v19
  - 7.2.0-v33
  - 7.3.2-v33
  - 8.2.0-v16
  enterprise: v0.37.0
  etcd:
  - 3.5.0-v20
  installer: v2024.12.18
  kubedump:
  - 0.1.0-v16
  mariadb:
  - 10.5.8-v27
  mongodb:
  - 3.4.17-v34
  - 3.4.22-v34
  - 3.6.13-v34
  - 3.6.8-v34
  - 4.0.11-v34
  - 4.0.3-v34
  - 4.0.5-v34
  - 4.1.13-v34
  - 4.1.4-v34
  - 4.1.7-v34
  - 4.2.3-v34
  - 4.4.6-v25
  - 5.0.15-v7
  - 5.0.3-v22
  - 6.0.5-v10
  mysql:
  - 5.7.25-v34
  - 8.0.14-v33
  - 8.0.21-v27
  - 8.0.3-v33
  nats:
  - 2.6.1-v21
  - 2.8.2-v16
  percona-xtradb:
  - 5.7-v28
  postgres:
  - 10.14-v32
  - 11.9-v32
  - 12.4-v32
  - 13.1-v29
  - 14.0-v21
  - 15.1-v13
  - 16.1-v2
  - "17.2"
  - 9.6.19-v32
  redis:
  - 5.0.13-v21
  - 6.2.5-v21
  - 7.0.5-v14
  ui-server: v0.18.0
  vault:
  - 1.10.3-v13
  version: v2024.12.18
---

# Customizing the Backup and Restore Process

This guide will show you how you can customize backup and restore processes in Stash according to your needs.

> Note: YAML files used in this tutorial are stored [here](https://github.com/stashed/docs/tree/{{< param "info.version" >}}/docs/guides/use-cases/customize-backup-restore/examples).

## Customizing Backup Process

In this section, we are going to show you how to customize the backup process. Here, we are going to show some examples of providing arguments to the backup process, running the backup process as a specific user, etc.

### Passing arguments to the backup process

Stash uses `restic` during the backup process. You can pass optional arguments to the restic backup process via `spec.target.args` field of BackupConfiguration.

Here is an example of passing `--ignore-inode` and `--tag=t1,t2` arguments to a Deployment backup process of Stash. 

```yaml
apiVersion: stash.appscode.com/v1beta1
kind: BackupConfiguration
metadata:
  name: deployment-backup
  namespace: demo
spec:
  repository:
    name: gcs-repo
  schedule: "*/5 * * * *"
  target:
    ref:
      apiVersion: apps/v1
      kind: Deployment
      name: sample-deployment
    volumeMounts:
      - name: source-data
        mountPath: /source/data
    paths:
      - /source/data
    args: ["--ignore-inode", "--tag=t1,t2"]
  retentionPolicy:
    name: "keep-last-5"
    keepLast: 5
    prune: true
```

> **WARNING**: Make sure that you have the specific workload created before taking backup. In this case, Deployment `sample-deployment` should exist before the backup sidecar starts.

### Running backup Container as a specific user

If your cluster requires running the backup job as a specific user, you can provide `securityContext` under `runtimeSettings.container` section. The below example shows how you can run the backup sidecar for a Deployment as the root user.

```yaml
apiVersion: stash.appscode.com/v1beta1
kind: BackupConfiguration
metadata:
  name: deployment-backup
  namespace: demo
spec:
  repository:
    name: gcs-repo
  schedule: "*/5 * * * *"
  target:
    ref:
      apiVersion: apps/v1
      kind: Deployment
      name: sample-deployment
    volumeMounts:
    - name: source-data
      mountPath: /source/data
    paths:
    - /source/data
  runtimeSettings:
    container:
      securityContext:
        runAsUser: 0
        runAsGroup: 0
  retentionPolicy:
    name: 'keep-last-5'
    keepLast: 5
    prune: true
```

### Specifying Memory/CPU limit/request for the backup 

If you want to specify the Memory/CPU limit/request for your backup process, you can specify `resources` field under `runtimeSettings.container` section.

```yaml
apiVersion: stash.appscode.com/v1beta1
kind: BackupConfiguration
metadata:
  name: deployment-backup
  namespace: demo
spec:
  repository:
    name: gcs-repo
  schedule: "*/5 * * * *"
  target:
    ref:
      apiVersion: apps/v1
      kind: Deployment
      name: sample-deployment
    volumeMounts:
    - name: source-data
      mountPath: /source/data
    paths:
    - /source/data
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

You can also specify multiple retention policies for your backed-up data. For example, you may want to keep few daily snapshots, few weekly snapshots, and few monthly snapshots, etc. You just need to pass the desired number with the respective key under the `retentionPolicy` section.

```yaml
apiVersion: stash.appscode.com/v1beta1
kind: BackupConfiguration
metadata:
  name: deployment-backup
  namespace: demo
spec:
  repository:
    name: gcs-repo
  schedule: "*/5 * * * *"
  target:
    ref:
      apiVersion: apps/v1
      kind: Deployment
      name: stash-demo
    volumeMounts:
    - name: source-data
      mountPath: /source/data
    paths:
    - /source/data
  retentionPolicy:
    name: sample-deployment-retention
    keepLast: 5
    keepDaily: 10
    keepWeekly: 20
    keepMonthly: 50
    keepYearly: 100
    prune: true
```

To know more about the available options for retention policies, please visit [here](/docs/v2024.12.18/concepts/crds/backupconfiguration/#specretentionpolicy).

## Customizing Restore Process

Stash uses `restic` during the restore process also. In this section, we are going to show how you can pass arguments to the restore process, restore a specific snapshot, run restore job as a specific user, etc.

### Passing arguments to the restore process

Similar to the backup process, you can pass optional arguments to the restic via `spec.target.args` in the restore process. 

```yaml
apiVersion: stash.appscode.com/v1beta1
kind: RestoreSession
metadata:
  name: deployment-restore
  namespace: demo
spec:
  repository:
    name: gcs-repo
  target:
    ref:
      apiVersion: apps/v1
      kind: Deployment
      name: sample-deployment
    volumeMounts:
      - name: source-data
        mountPath: /source/data
    rules:
      - paths:
          - /source/data/s
    args: ["--tag=t1,t2"]
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

The below example shows how you can pass a specific snapshot ID through the `snapshots` field of `spec.target.rules` section.

```yaml
apiVersion: stash.appscode.com/v1beta1
kind: RestoreSession
metadata:
  name: deployment-restore
  namespace: demo
spec:
  repository:
    name: gcs-repo
  target: # target indicates where the recovered data will be stored
    ref:
      apiVersion: apps/v1
      kind: Deployment
      name: stash-recovered
    volumeMounts:
    - name:  source-data
      mountPath:  /source/data
    rules:
    - snapshots: [4bc21d6f]
```

>Please, do not specify multiple snapshots here. Each snapshot represents a complete backup of your database. Multiple snapshots are only usable during file/directory restore.

### Running restore init-container as a specific user

You can provide `securityContext` under `runtimeSettings.container` section to run the restore init-container as a specific user during workload restore.

```yaml
apiVersion: stash.appscode.com/v1beta1
kind: RestoreSession
metadata:
  name: sample-deployment
  namespace: demo
spec:
  repository:
    name: gcs-repo
  target: # target indicates where the recovered data will be stored
    ref:
      apiVersion: apps/v1
      kind: Deployment
      name: stash-recovered
    volumeMounts:
    - name:  source-data
      mountPath:  /source/data
  runtimeSettings:
    container:
      securityContext:
        runAsUser: 0
        runAsGroup: 0
  rules:
  - snapshots: [latest]
```

### Specifying Memory/CPU limit/request for the restore process

Similar to the backup process, you can also provide `resources` field under the `runtimeSettings.container` section to limit the Memory/CPU for your restore process.

```yaml
apiVersion: stash.appscode.com/v1beta1
kind: RestoreSession
metadata:
  name: sample-deployment
  namespace: demo
spec:
  repository:
    name: gcs-repo
  target: # target indicates where the recovered data will be stored
    ref:
      apiVersion: apps/v1
      kind: Deployment
      name: stash-recovered
    volumeMounts:
    - name:  source-data
      mountPath:  /source/data
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
