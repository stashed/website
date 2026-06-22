---
title: Stash
menu:
  docs_v2025.7.31:
    identifier: stash
    name: Stash
    parent: reference-operator
    weight: 0
menu_name: docs_v2025.7.31
section_menu_id: reference
url: /docs/v2025.7.31/reference/operator/
aliases:
- /docs/v2025.7.31/reference/operator/stash/
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

* [stash backup-pvc](/docs/v2025.7.31/reference/operator/stash_backup-pvc)	 - Takes a backup of Persistent Volume Claim
* [stash create-backupsession](/docs/v2025.7.31/reference/operator/stash_create-backupsession)	 - create a BackupSession
* [stash create-vs](/docs/v2025.7.31/reference/operator/stash_create-vs)	 - Take snapshot of PersistentVolumeClaims
* [stash forget](/docs/v2025.7.31/reference/operator/stash_forget)	 - Delete snapshots from a restic repository
* [stash restore](/docs/v2025.7.31/reference/operator/stash_restore)	 - Restore from backup
* [stash restore-pvc](/docs/v2025.7.31/reference/operator/stash_restore-pvc)	 - Takes a restore of Persistent Volume Claim
* [stash restore-vs](/docs/v2025.7.31/reference/operator/stash_restore-vs)	 - Restore PVC from VolumeSnapshot
* [stash run](/docs/v2025.7.31/reference/operator/stash_run)	 - Launch Stash Controller
* [stash run-backup](/docs/v2025.7.31/reference/operator/stash_run-backup)	 - Take backup of workload paths
* [stash run-hook](/docs/v2025.7.31/reference/operator/stash_run-hook)	 - Execute Backup or Restore Hooks
* [stash snapshots](/docs/v2025.7.31/reference/operator/stash_snapshots)	 - Get snapshots of restic repo
* [stash update-status](/docs/v2025.7.31/reference/operator/stash_update-status)	 - Update status of Repository, Backup/Restore Session
* [stash version](/docs/v2025.7.31/reference/operator/stash_version)	 - Prints binary version number.

