---
title: Stash
menu:
  docs_v2023.10.9:
    identifier: stash
    name: Stash
    parent: reference-operator
    weight: 0
menu_name: docs_v2023.10.9
section_menu_id: reference
url: /docs/v2023.10.9/reference/operator/
aliases:
- /docs/v2023.10.9/reference/operator/stash/
info:
  cli: v0.32.0
  community: v0.32.0
  elasticsearch:
  - 5.6.4-v28
  - 6.2.4-v28
  - 6.3.0-v28
  - 6.4.0-v28
  - 6.5.3-v28
  - 6.8.0-v28
  - 7.14.0-v14
  - 7.2.0-v28
  - 7.3.2-v28
  - 8.2.0-v11
  enterprise: v0.32.0
  etcd:
  - 3.5.0-v15
  installer: v2023.10.9
  kubedump:
  - 0.1.0-v11
  mariadb:
  - 10.5.8-v21
  mongodb:
  - 3.4.17-v28
  - 3.4.22-v28
  - 3.6.13-v28
  - 3.6.8-v28
  - 4.0.11-v28
  - 4.0.3-v28
  - 4.0.5-v28
  - 4.1.13-v28
  - 4.1.4-v28
  - 4.1.7-v28
  - 4.2.3-v28
  - 4.4.6-v19
  - 5.0.15-v1
  - 5.0.3-v16
  - 6.0.5-v4
  mysql:
  - 5.7.25-v28
  - 8.0.14-v28
  - 8.0.21-v22
  - 8.0.3-v28
  nats:
  - 2.6.1-v16
  - 2.8.2-v11
  percona-xtradb:
  - 5.7-v23
  postgres:
  - 10.14-v27
  - 11.9-v27
  - 12.4-v27
  - 13.1-v24
  - 14.0-v16
  - 15.1-v8
  - 9.6.19-v27
  redis:
  - 5.0.13-v16
  - 6.2.5-v16
  - 7.0.5-v9
  ui-server: v0.13.0
  vault:
  - 1.10.3-v8
  version: v2023.10.9
---

## stash

Stash by AppsCode - Backup your Kubernetes Volumes

### Synopsis

Stash is a Kubernetes operator for restic. For more information, visit here: https://appscode.com/products/stash

### Options

```
      --bypass-validating-webhook-xray   if true, bypasses validating webhook xray checks
  -h, --help                             help for stash
      --use-kubeapiserver-fqdn-for-aks   if true, uses kube-apiserver FQDN for AKS cluster to workaround https://github.com/Azure/AKS/issues/522 (default true)
```

### SEE ALSO

* [stash backup-pvc](/docs/v2023.10.9/reference/operator/stash_backup-pvc)	 - Takes a backup of Persistent Volume Claim
* [stash create-backupsession](/docs/v2023.10.9/reference/operator/stash_create-backupsession)	 - create a BackupSession
* [stash create-vs](/docs/v2023.10.9/reference/operator/stash_create-vs)	 - Take snapshot of PersistentVolumeClaims
* [stash forget](/docs/v2023.10.9/reference/operator/stash_forget)	 - Delete snapshots from a restic repository
* [stash restore](/docs/v2023.10.9/reference/operator/stash_restore)	 - Restore from backup
* [stash restore-pvc](/docs/v2023.10.9/reference/operator/stash_restore-pvc)	 - Takes a restore of Persistent Volume Claim
* [stash restore-vs](/docs/v2023.10.9/reference/operator/stash_restore-vs)	 - Restore PVC from VolumeSnapshot
* [stash run](/docs/v2023.10.9/reference/operator/stash_run)	 - Launch Stash Controller
* [stash run-backup](/docs/v2023.10.9/reference/operator/stash_run-backup)	 - Take backup of workload paths
* [stash run-hook](/docs/v2023.10.9/reference/operator/stash_run-hook)	 - Execute Backup or Restore Hooks
* [stash snapshots](/docs/v2023.10.9/reference/operator/stash_snapshots)	 - Get snapshots of restic repo
* [stash update-status](/docs/v2023.10.9/reference/operator/stash_update-status)	 - Update status of Repository, Backup/Restore Session
* [stash version](/docs/v2023.10.9/reference/operator/stash_version)	 - Prints binary version number.

