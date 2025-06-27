---
title: Permission denied | Stash
description: Troubleshooting "Permission denied" issue
menu:
  docs_v2025.6.30:
    identifier: troubleshooting-permission-denied
    name: Permission denied
    parent: troubleshooting
    weight: 20
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

# Troubleshooting `"permission denied"` issue

Sometimes the backup or restore fails due to permission issues. This can happen for various reasons. In this guide, we are going to explain the known scenarios when this issue can arise and what you can do to solve it.

## Identifying the issue

If you describe the respective `BackupSession` / `RestoreSession` or view the log from the respective backup/restore sidecar/job, you should see a message pointing to the `permission denied` error.

## Possible reasons

The issue can happen during both backup and restore. Here, are a few possible scenarios when you can face the issue.

### During Backup

You may see the permission issue during backup in the following cases.

#### Using Local Backend

If you are using local volumes such as `NFS`, `hostPath`, `PVC` etc. as your backend, you may face this issue.

### Using InterimVolume during backup

If you are using an addon that needs `interimVolume` for storing the data temporarily during backup (i.e. Elasticsearch addon), you may face this issue.

### During Restore

You may see the permission issue during the restore process in the following scenarios.

### Backup was taken as a particular user

By default, Stash runs the backup as user `nobody`. However, you may need to run the backup as a particular (i.e. `root` user) user in various scenarios. In this case, if the user id of the restore process does not match with the user id that was used during backup, the restore process will fail to read data from the backend repository.

### Using InterimVolume during restore

If you are using an addon that needs `interimVolume` for storing the data temporarily during restore (i.e. Elasticsearch addon), you may face this issue.

## Solutions

Here, are a few actions you can take to solve the issue in the scenarios mentioned above.

### For local volume as backend

If you are facing the issue while using local volume as a backend, you can take any of the following actions to solve the issue.

#### Run the backup/restore as `root` user

You can run the backup process as `root` by adding `runtimeSettings.container.securityContext` in the `BackupConfiguration` or `RestoreSession` spec.

Here, is an example of running backup as `root` user:

```yaml
apiVersion: stash.appscode.com/v1beta1
kind: BackupConfiguration
metadata:
  name: ss-backup
  namespace: demo
spec:
  repository:
    name: local-repo
  schedule: "*/3 * * * *"
  target:
    ref:
      apiVersion: apps/v1
      kind: StatefulSet
      name: stash-demo
    volumeMounts:
    - name: data-volume
      mountPath: /my/data
    paths:
    - /my/data
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

Here, is an example of running restores as `root` user:

```yaml
apiVersion: stash.appscode.com/v1beta1
kind: RestoreSession
metadata:
  name: sample-mongodb-restore
  namespace: kubedb
spec:
  repository:
    name: local-repo-with-hostpath
  target:
    ref:
      apiVersion: appcatalog.appscode.com/v1alpha1
      kind: AppBinding
      name: restored-mongodb
  runtimeSettings:
    container:
      securityContext:
        runAsUser: 0
        runAsGroup: 0
  rules:
  - snapshots: [latest]
```

#### Provide `fsGroup` during backup/restore

If you don't want to run the backup/restore as `root` user, you can provide `fsGroup` of Stash default user `65534` in `runtimeSettings.pod.securityContext` section.

Here, is an example of setting `fsGroup`:

```yaml
apiVersion: stash.appscode.com/v1beta1
kind: BackupConfiguration
metadata:
  name: sample-mongodb-backup
  namespace: kubedb
spec:
  schedule: "*/5 * * * *"
  repository:
    name: local-repo-with-pvc
  target:
    ref:
      apiVersion: appcatalog.appscode.com/v1alpha1
      kind: AppBinding
      name: sample-mongodb
  runtimeSettings:
    pod:
      securityContext:
        fsGroup: 65534
  retentionPolicy:
    name: keep-last-5
    keepLast: 5
    prune: true
```

> If you are taking backup of workload (i.e. StatefulSet, Deployment, etc.) volumes, you have to provide the `fsGroup` in your workload spec instead of `BackupConfiguration` / `RestoreSession`.

#### Give read, write permissions to all users

You can also use `chmod` to give read, write permissions to all users for the directory you are using as backend.

### For using InterimVolume

If you are facing the issue because of using `interimVolume` in your backup/restore process, you can either run the backup/restore process as `root` user or you can provide the storage access permission to Stash using `fsGroup`.

Here, is an example of running backup as `root` user:

```yaml
apiVersion: stash.appscode.com/v1beta1
kind: BackupConfiguration
metadata:
  name: sample-elasticsearch-backup
  namespace: demo
spec:
  schedule: "*/5 * * * *"
  repository:
    name: gcs-repo
  target:
    ref:
      apiVersion: appcatalog.appscode.com/v1alpha1
      kind: AppBinding
      name: sample-elasticsearch
  interimVolumeTemplate:
    metadata:
      name: stash-tmp-backup-storage
    spec:
      accessModes: [ "ReadWriteOnce" ]
      storageClassName: "standard"
      resources:
        requests:
          storage: 1Gi
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

Here, is an example of using `fsGroup`:

```yaml
apiVersion: stash.appscode.com/v1beta1
kind: BackupConfiguration
metadata:
  name: sample-elasticsearch-backup
  namespace: demo
spec:
  schedule: "*/2 * * * *"
  repository:
    name: gcs-repo
  target:
    ref:
      apiVersion: appcatalog.appscode.com/v1alpha1
      kind: AppBinding
      name: sample-elasticsearch
  interimVolumeTemplate:
    metadata:
      name: stash-tmp-backup-storage
    spec:
      accessModes: [ "ReadWriteOnce" ]
      storageClassName: "standard"
      resources:
        requests:
          storage: 1Gi
  runtimeSettings:
    pod:
      securityContext:
        fsGroup: 65534
  retentionPolicy:
    name: keep-last-5
    keepLast: 5
    prune: true
```

### For user id mismatch during restore

If your restore fails because it does not have the necessary permission to read backed up data from the repository, you have to run the restore process as the same user as the backup process or `root` user using the `runtimeSettings.container.securityContext` section.

Here, is an example of running restore as a particular user:

```yaml
apiVersion: stash.appscode.com/v1beta1
kind: RestoreSession
metadata:
  name: sample-statefulset-restore
  namespace: demo
spec:
  repository:
    name: gcs-repo
  target:
    ref:
      apiVersion: apps/v1
      kind: StatefulSet
      name: stash-recovered
    volumeMounts:
    - name:  source-data
      mountPath:  /source/data
    rules:
    - paths:
      - /source/data
  runtimeSettings:
    container:
      securityContext:
        runAsUser: 2000
        runAsGroup: 2000
```
