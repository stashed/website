---
title: How to Troubleshoot Stash issues | Stash
description: Troubleshooting backup/restore issues
menu:
  docs_v2022.02.22:
    identifier: troubleshooting-how-to
    name: How to Troubleshoot
    parent: troubleshooting
    weight: 5
product_name: stash
menu_name: docs_v2022.02.22
section_menu_id: guides
info:
  cli: v0.18.0
  community: v0.18.0
  elasticsearch:
  - 5.6.4-v15
  - 6.2.4-v15
  - 6.3.0-v15
  - 6.4.0-v15
  - 6.5.3-v15
  - 6.8.0-v15
  - 7.14.0-v1
  - 7.2.0-v15
  - 7.3.2-v15
  enterprise: v0.18.0
  etcd:
  - 3.5.0-v2
  installer: v2022.02.22
  mariadb:
  - 10.5.8-v8
  mongodb:
  - 3.4.17-v14
  - 3.4.22-v14
  - 3.6.13-v14
  - 3.6.8-v14
  - 4.0.11-v14
  - 4.0.3-v14
  - 4.0.5-v14
  - 4.1.13-v14
  - 4.1.4-v14
  - 4.1.7-v14
  - 4.2.3-v14
  - 4.4.6-v5
  - 5.0.3-v2
  mysql:
  - 5.7.25-v15
  - 8.0.14-v15
  - 8.0.21-v9
  - 8.0.3-v15
  nats:
  - 2.6.1-v2
  percona-xtradb:
  - 5.7-v10
  postgres:
  - 10.14-v13
  - 11.9-v13
  - 12.4-v13
  - 13.1-v10
  - 14.0-v2
  - 9.6.19-v13
  redis:
  - 5.0.13-v3
  - 6.2.5-v3
  ui-server: v0.1.0
  version: v2022.02.22
---

# How to Troubleshoot Stash Issues

This guide will give you an overview of how you can gather the necessary information to identify the issue that causes the backup/restore failure.

## Troubleshoot Backup Issues

In this section, we are going to explain how to troubleshoot backup issues.

### Backup was Never Triggered

If you have created the desired `BackupConfiguration` but the respective backup triggering CronJob was never created or any `BackupSession` was not created in the scheduled time, in this case, follow the following steps:

#### Describe the `BackupConfiguration`

At first, you should describe the respective `BackupConfiguration` as below:

```bash
kubectl describe backupconfiguration <backupconfiguration name> -n <namespace>
```

Now, check the `Status` section of `BackupConfiguration`. Make sure all the `conditions` are `True`. If there is any issue during backup setup, you should see the error in the respective condition.

Also, check the event to see if there is any indication of an error.

#### Check Stash operator log

If you don't notice any error in the previous step, you should check the Stash operator log.

Run the following command to view the Stash operator log:

```bash
# Identify the Stash operator pod
kubectl get pods --all-namespaces | grep stash

# View log of Stash operator pod
kubectl logs -n <operator namespace> <stash operator pod name> -c operator
```

Now, inspect the log carefully. You should see the respective error in the log.

### Backup Failed

If a backup fails, follow the following steps to identify the root cause.

#### Describe the `BackupSession`

At first, describe the failing `BackupSession`. You should see the respective error in the `Status` section.

```bash
kubectl describe backupsession <backupsession name> -n <namespace>
```

Also, check the `Events` section. Sometimes, it can be helpful to identify the issue.

#### View Backup Job/Sidecar log

If you don't see any error in the previous step, you should try checking the log of the respective backup job/sidecar.

If you are trying to backup a workload, run the following command to inspect the log:

```bash
kubectl logs -n <namespace> <workload pod name> -c stash
```

If you are trying to backup using Stash addons, run the following command to inspect the log:

```bash
# Identify the backup pod
kubectl get pods -n <namespace> | grep stash-backup

# View the log of the backup pod
kubectl logs -n <namespace> <backup pod name> --all-containers
```

Inspect the log carefully. You should notice the respective error that leads to backup failure.

## Troubleshoot Restore Issues

If a restore fails, follow the following steps to identify the root cause.

#### Describe the `RestoreSession`

At first, describe the failing `RestoreSession`. You should see the respective error in the `Status` section.

```bash
kubectl describe restoresession <restoresession name> -n <namespace>
```

Also, check the `Events` section. Sometimes, it can be helpful to identify the issue.

#### View Restore Job/Init-Container log

If you don't see any error in the previous step, you should try checking the log of the respective restore job / init-container.

If you are trying to restore a workload, run the following command to inspect the log:

```bash
kubectl logs -n <namespace> <workload pod name> -c stash-init
```

If you are trying to restore using Stash addons, run the following command to inspect the log:

```bash
# Identify the restore pod
kubectl get pods -n <namespace> | grep stash-restore

# View the log of the restore pod
kubectl logs -n <namespace> <restore pod name> --all-containers
```

Inspect the log carefully. You should notice the respective error that leads to restore failure.
