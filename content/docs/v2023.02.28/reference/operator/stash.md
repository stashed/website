---
title: Stash
menu:
  docs_v2023.02.28:
    identifier: stash
    name: Stash
    parent: reference-operator
    weight: 0
menu_name: docs_v2023.02.28
section_menu_id: reference
url: /docs/v2023.02.28/reference/operator/
aliases:
- /docs/v2023.02.28/reference/operator/stash/
info:
  cli: v0.26.0
  community: v0.26.0
  elasticsearch:
  - 5.6.4-v22
  - 6.2.4-v22
  - 6.3.0-v22
  - 6.4.0-v22
  - 6.5.3-v22
  - 6.8.0-v22
  - 7.14.0-v8
  - 7.2.0-v22
  - 7.3.2-v22
  - 8.2.0-v5
  enterprise: v0.26.0
  etcd:
  - 3.5.0-v9
  installer: v2023.02.28
  kubedump:
  - 0.1.0-v5
  mariadb:
  - 10.5.8-v15
  mongodb:
  - 3.4.17-v22
  - 3.4.22-v22
  - 3.6.13-v22
  - 3.6.8-v22
  - 4.0.11-v22
  - 4.0.3-v22
  - 4.0.5-v22
  - 4.1.13-v22
  - 4.1.4-v22
  - 4.1.7-v22
  - 4.2.3-v22
  - 4.4.6-v13
  - 5.0.3-v10
  mysql:
  - 5.7.25-v22
  - 8.0.14-v22
  - 8.0.21-v16
  - 8.0.3-v22
  nats:
  - 2.6.1-v10
  - 2.8.2-v5
  percona-xtradb:
  - 5.7-v17
  postgres:
  - 10.14-v21
  - 11.9-v21
  - 12.4-v21
  - 13.1-v18
  - 14.0-v10
  - 15.1-v2
  - 9.6.19-v21
  redis:
  - 5.0.13-v10
  - 6.2.5-v10
  - 7.0.5-v3
  ui-server: v0.8.0
  vault:
  - 1.10.3-v2
  version: v2023.02.28
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

* [stash backup-pvc](/docs/v2023.02.28/reference/operator/stash_backup-pvc)	 - Takes a backup of Persistent Volume Claim
* [stash create-backupsession](/docs/v2023.02.28/reference/operator/stash_create-backupsession)	 - create a BackupSession
* [stash create-vs](/docs/v2023.02.28/reference/operator/stash_create-vs)	 - Take snapshot of PersistentVolumeClaims
* [stash forget](/docs/v2023.02.28/reference/operator/stash_forget)	 - Delete snapshots from a restic repository
* [stash restore](/docs/v2023.02.28/reference/operator/stash_restore)	 - Restore from backup
* [stash restore-pvc](/docs/v2023.02.28/reference/operator/stash_restore-pvc)	 - Takes a restore of Persistent Volume Claim
* [stash restore-vs](/docs/v2023.02.28/reference/operator/stash_restore-vs)	 - Restore PVC from VolumeSnapshot
* [stash run](/docs/v2023.02.28/reference/operator/stash_run)	 - Launch Stash Controller
* [stash run-backup](/docs/v2023.02.28/reference/operator/stash_run-backup)	 - Take backup of workload paths
* [stash run-hook](/docs/v2023.02.28/reference/operator/stash_run-hook)	 - Execute Backup or Restore Hooks
* [stash snapshots](/docs/v2023.02.28/reference/operator/stash_snapshots)	 - Get snapshots of restic repo
* [stash update-status](/docs/v2023.02.28/reference/operator/stash_update-status)	 - Update status of Repository, Backup/Restore Session
* [stash version](/docs/v2023.02.28/reference/operator/stash_version)	 - Prints binary version number.
