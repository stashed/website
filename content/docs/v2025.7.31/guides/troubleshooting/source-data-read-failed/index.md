---
title: Source data read failed | Stash
description: Troubleshooting "failed to read all source data" issue
menu:
  docs_v2025.7.31:
    identifier: troubleshooting-source-data-read-failed
    name: Source data read failed
    parent: troubleshooting
    weight: 30
product_name: stash
menu_name: docs_v2025.7.31
section_menu_id: guides
info:
  cli: v0.41.0
  community: v0.41.0
  elasticsearch:
  - 5.6.4-v37
  - 6.2.4-v37
  - 6.3.0-v37
  - 6.4.0-v37
  - 6.5.3-v37
  - 6.8.0-v37
  - 7.14.0-v23
  - 7.2.0-v37
  - 7.3.2-v37
  - 8.2.0-v20
  enterprise: v0.41.0
  etcd:
  - 3.5.0-v24
  installer: v2025.7.31
  kubedump:
  - 0.2.0-v5
  mariadb:
  - 10.5.8-v31
  mongodb:
  - 3.4.17-v38
  - 3.4.22-v38
  - 3.6.13-v38
  - 3.6.8-v38
  - 4.0.11-v38
  - 4.0.3-v38
  - 4.0.5-v38
  - 4.1.13-v38
  - 4.1.4-v38
  - 4.1.7-v38
  - 4.2.3-v38
  - 4.4.6-v29
  - 5.0.15-v11
  - 5.0.3-v26
  - 6.0.5-v14
  mysql:
  - 5.7.25-v38
  - 8.0.14-v37
  - 8.0.21-v31
  - 8.0.3-v37
  nats:
  - 2.6.1-v25
  - 2.8.2-v20
  percona-xtradb:
  - 5.7-v32
  postgres:
  - 10.14-v36
  - 11.9-v36
  - 12.4-v36
  - 13.1-v33
  - 14.0-v25
  - 15.1-v17
  - 16.1-v6
  - 17.2-v4
  - 9.6.19-v36
  redis:
  - 5.0.13-v25
  - 6.2.5-v25
  - 7.0.5-v18
  ui-server: v0.22.0
  vault:
  - 1.10.3-v17
  version: v2025.7.31
---

# Troubleshooting `"failed to read all source data"` issue

Sometimes, the backup fails due to Stash being unable to read the targeted data. In this guide, we are going to explain the possible scenario when this error can happen and what you can do to solve the issue.

## Identifying the issue

If you describe the respective `BackupSession` or view the log from the respective backup sidecar/job, you should see the following error:

```bash
Warning: failed to read all source data during backup
```

## Possible reason

By default, Stash runs backup as a non-root user. If the target data directory is not readable to all users, then Stash will fail to read the targeted data.

## Solution

Run the backup process as the same user as the targeted application or run the backup process as the root user.

Here is an example of `BackupConfiguration` for running backup as the root user:

```yaml
apiVersion: stash.appscode.com/v1beta1
kind: BackupConfiguration
metadata:
  name: sample-statefulset-backup
  namespace: demo
spec:
  repository:
    name: s3-repo
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

If you don't want to run the backup as root user, then set the `runAsUser` and `runAsGroup` to match with your application user and group id.
