---
title: Stash
menu:
  docs_v2024.12.18:
    identifier: stash
    name: Stash
    parent: reference-operator
    weight: 0
menu_name: docs_v2024.12.18
section_menu_id: reference
url: /docs/v2024.12.18/reference/operator/
aliases:
- /docs/v2024.12.18/reference/operator/stash/
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

## stash

Stash by AppsCode - Backup your Kubernetes Volumes

### Synopsis

Stash is a Kubernetes operator for restic. For more information, visit here: https://stash.run

### Options

```
      --bypass-validating-webhook-xray   if true, bypasses validating webhook xray checks
  -h, --help                             help for stash
      --use-kubeapiserver-fqdn-for-aks   if true, uses kube-apiserver FQDN for AKS cluster to workaround https://github.com/Azure/AKS/issues/522 (default true)
```

### SEE ALSO

* [stash backup-pvc](/docs/v2024.12.18/reference/operator/stash_backup-pvc)	 - Takes a backup of Persistent Volume Claim
* [stash create-backupsession](/docs/v2024.12.18/reference/operator/stash_create-backupsession)	 - create a BackupSession
* [stash create-vs](/docs/v2024.12.18/reference/operator/stash_create-vs)	 - Take snapshot of PersistentVolumeClaims
* [stash forget](/docs/v2024.12.18/reference/operator/stash_forget)	 - Delete snapshots from a restic repository
* [stash restore](/docs/v2024.12.18/reference/operator/stash_restore)	 - Restore from backup
* [stash restore-pvc](/docs/v2024.12.18/reference/operator/stash_restore-pvc)	 - Takes a restore of Persistent Volume Claim
* [stash restore-vs](/docs/v2024.12.18/reference/operator/stash_restore-vs)	 - Restore PVC from VolumeSnapshot
* [stash run](/docs/v2024.12.18/reference/operator/stash_run)	 - Launch Stash Controller
* [stash run-backup](/docs/v2024.12.18/reference/operator/stash_run-backup)	 - Take backup of workload paths
* [stash run-hook](/docs/v2024.12.18/reference/operator/stash_run-hook)	 - Execute Backup or Restore Hooks
* [stash snapshots](/docs/v2024.12.18/reference/operator/stash_snapshots)	 - Get snapshots of restic repo
* [stash update-status](/docs/v2024.12.18/reference/operator/stash_update-status)	 - Update status of Repository, Backup/Restore Session
* [stash version](/docs/v2024.12.18/reference/operator/stash_version)	 - Prints binary version number.

