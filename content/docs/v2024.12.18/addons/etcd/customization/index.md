---
title: Etcd Backup Customization | Stash
description: Customizing Backup and Restore process with Stash
menu:
  docs_v2024.12.18:
    identifier: stash-etcd-customization
    name: Customizing Backup & Restore Process
    parent: stash-etcd
    weight: 40
product_name: stash
menu_name: docs_v2024.12.18
section_menu_id: stash-addons
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
  perconaxtradb:
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

# Customizing Backup and Restore Process

Stash provides rich customization supports for the backup and restore process to meet the requirements of various cluster configurations. This guide will show you some examples of these customizations.

> Note: YAML files used in this tutorial are stored [here](https://github.com/stashed/docs/tree/{{< param "info.version" >}}/docs/addons/etcd/customization/examples).

## Customizing Backup Process

In this section, we are going to show you how to customize the backup process. Here, we are going to show some examples of providing arguments to the backup process, running the backup process as a specific user, ignoring some indexes during the backup process, etc.

### Passing arguments to the backup process
Stash Etcd addon uses [etcdctl](https://github.com/etcd-io/etcd/tree/main/etcdctl) for backup. You can pass arguments to the `etcdctl` through `args` param under `task.params` section.

The below example shows how you can pass the `--insecure-skip-tls-verify` to skip server certificate verification (`CAUTION`: this option should be enabled only for testing purposes)
```yaml
apiVersion: stash.appscode.com/v1beta1
kind: BackupConfiguration
metadata:
  name: etcd-backup
  namespace: demo
spec:
  schedule: "*/5 * * * *"
  task:
    name: etcd-backup-3.5.0
    params:
    - name: args
      value: --insecure-skip-tls-verify=true	
  repository:
    name: gcs-repo
  target:
    ref:
      apiVersion: appcatalog.appscode.com/v1alpha1
      kind: AppBinding
      name: etcd-appbinding 
  retentionPolicy:
    name: keep-last-5
    keepLast: 5
    prune: true
```

### Running backup job as a specific user

If your cluster requires running the backup job as a specific user, you can provide `securityContext` under `runtimeSettings.pod` section. The below example shows how you can run the backup job as the root user.

```yaml
apiVersion: stash.appscode.com/v1beta1
kind: BackupConfiguration
metadata:
  name: etcd-backup
  namespace: demo
spec:
  schedule: "*/5 * * * *"
  task:
    name: etcd-backup-3.5.0
  repository:
    name: gcs-repo
  target:
    ref:
      apiVersion: appcatalog.appscode.com/v1alpha1
      kind: AppBinding
      name: etcd-appbinding
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
  name: etcd-backup
  namespace: demo
spec:
  schedule: "*/5 * * * *"
  task:
    name: etcd-backup-3.5.0
  repository:
    name: gcs-repo
  target:
    ref:
      apiVersion: appcatalog.appscode.com/v1alpha1
      kind: AppBinding
      name: etcd-appbinding
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
  name: etcd-backup
  namespace: demo
spec:
  schedule: "*/5 * * * *"
  task:
    name: etcd-backup-3.5.0
  repository:
    name: gcs-repo
  target:
    ref:
      apiVersion: appcatalog.appscode.com/v1alpha1
      kind: AppBinding
      name: etcd-appbinding
  retentionPolicy:
    name: sample-etcd-retention
    keepLast: 5
    keepDaily: 10
    keepWeekly: 20
    keepMonthly: 50
    keepYearly: 100
    prune: true
```

To know more about the available options for retention policies, please visit [here](/docs/v2024.12.18/concepts/crds/backupconfiguration/#specretentionpolicy).

## Customizing Restore Process

Stash uses `etcdctl` during the restore process as well. In this section, we are going to show how you can pass arguments to the restore process, restore a specific snapshot, run restore job as a specific user, etc.

### Passing arguments to the restore process

Similar to the backup process, you can pass additional arguments to the restore process alongside with the reqiored arguements through the `args` params under `task.params` section. Here, we have passed `--insecure-skip-tls-verify` argument to the `etcdctl` to skip server certificate verification (CAUTION: this option should be enabled only for testing purposes).

```yaml
apiVersion: stash.appscode.com/v1beta1
kind: RestoreSession
metadata:
  name: etcd-restore
  namespace: demo
spec:
  task:
    name: etcd-restore-3.5.0 
    params:
      - name: initialCluster
        value:  "etcd-0=http://etcd-0.etcd:2380,etcd-1=http://etcd-1.etcd:2380,etcd-2=http://etcd-2.etcd:2380"
      - name: initialClusterToken
        value: "etcd-cluster-1"
      - name: dataDir
        value: "/var/run/etcd"
      - name: workloadKind
        value: "StatefulSet"
      - name: workloadName
        value: "etcd"
      - name: args
        value: --insecure-skip-tls-verify=true
  repository:
    name: gcs-repo
  target:
    ref:
      apiVersion: appcatalog.appscode.com/v1alpha1
      kind: AppBinding
      name: etcd-appbinding 
  runtimeSettings:
    container:
      securityContext:
        runAsUser: 0
        runAsGroup: 0
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
  name: etcd-restore
  namespace: demo
spec:
  task:
    name: etcd-restore-3.5.0 
    params:
      - name: initialCluster
        value:  "etcd-0=http://etcd-0.etcd:2380,etcd-1=http://etcd-1.etcd:2380,etcd-2=http://etcd-2.etcd:2380"
      - name: initialClusterToken
        value: "etcd-cluster-1"
      - name: dataDir
        value: "/var/run/etcd"
      - name: workloadKind
        value: "StatefulSet"
      - name: workloadName
        value: "etcd"
  repository:
    name: gcs-repo
  target:
    ref:
      apiVersion: appcatalog.appscode.com/v1alpha1
      kind: AppBinding
      name: etcd-appbinding 
  runtimeSettings:
    container:
      securityContext:
        runAsUser: 0
        runAsGroup: 0
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
  name: etcd-restore
  namespace: demo
spec:
  task:
    name: etcd-restore-3.5.0 
    params:
      - name: initialCluster
        value:  "etcd-0=http://etcd-0.etcd:2380,etcd-1=http://etcd-1.etcd:2380,etcd-2=http://etcd-2.etcd:2380"
      - name: initialClusterToken
        value: "etcd-cluster-1"
      - name: dataDir
        value: "/var/run/etcd"
      - name: workloadKind
        value: "StatefulSet"
      - name: workloadName
        value: "etcd"
  repository:
    name: gcs-repo
  target:
    ref:
      apiVersion: appcatalog.appscode.com/v1alpha1
      kind: AppBinding
      name: etcd-appbinding 
  runtimeSettings:
    container:
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
  name: etcd-restore
  namespace: demo
spec:
  task:
    name: etcd-restore-3.5.0 
    params:
      - name: initialCluster
        value:  "etcd-0=http://etcd-0.etcd:2380,etcd-1=http://etcd-1.etcd:2380,etcd-2=http://etcd-2.etcd:2380"
      - name: initialClusterToken
        value: "etcd-cluster-1"
      - name: dataDir
        value: "/var/run/etcd"
      - name: workloadKind
        value: "StatefulSet"
      - name: workloadName
        value: "etcd"
  repository:
    name: gcs-repo
  target:
    ref:
      apiVersion: appcatalog.appscode.com/v1alpha1
      kind: AppBinding
      name: etcd-appbinding 
  runtimeSettings:
    container:
      securityContext:
        runAsUser: 0
        runAsGroup: 0
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
