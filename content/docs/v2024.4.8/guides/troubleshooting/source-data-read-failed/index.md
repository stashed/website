---
title: Source data read failed | Stash
description: Troubleshooting "failed to read all source data" issue
menu:
  docs_v2024.4.8:
    identifier: troubleshooting-source-data-read-failed
    name: Source data read failed
    parent: troubleshooting
    weight: 30
product_name: stash
menu_name: docs_v2024.4.8
section_menu_id: guides
info:
  cli: v0.34.0
  community: v0.34.0
  elasticsearch:
  - 5.6.4-v31
  - 6.2.4-v31
  - 6.3.0-v31
  - 6.4.0-v31
  - 6.5.3-v31
  - 6.8.0-v31
  - 7.14.0-v17
  - 7.2.0-v31
  - 7.3.2-v31
  - 8.2.0-v14
  enterprise: v0.34.0
  etcd:
  - 3.5.0-v18
  installer: v2024.4.8
  kubedump:
  - 0.1.0-v14
  mariadb:
  - 10.5.8-v25
  mongodb:
  - 3.4.17-v31
  - 3.4.22-v31
  - 3.6.13-v31
  - 3.6.8-v31
  - 4.0.11-v31
  - 4.0.3-v31
  - 4.0.5-v31
  - 4.1.13-v31
  - 4.1.4-v31
  - 4.1.7-v31
  - 4.2.3-v31
  - 4.4.6-v22
  - 5.0.15-v4
  - 5.0.3-v19
  - 6.0.5-v7
  mysql:
  - 5.7.25-v31
  - 8.0.14-v31
  - 8.0.21-v25
  - 8.0.3-v31
  nats:
  - 2.6.1-v19
  - 2.8.2-v14
  percona-xtradb:
  - 5.7-v26
  postgres:
  - 10.14-v30
  - 11.9-v30
  - 12.4-v30
  - 13.1-v27
  - 14.0-v19
  - 15.1-v11
  - "16.1"
  - 9.6.19-v30
  redis:
  - 5.0.13-v19
  - 6.2.5-v19
  - 7.0.5-v12
  ui-server: v0.15.0
  vault:
  - 1.10.3-v11
  version: v2024.4.8
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
