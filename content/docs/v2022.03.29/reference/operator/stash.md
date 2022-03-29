---
title: Stash
menu:
  docs_v2022.03.29:
    identifier: stash
    name: Stash
    parent: reference-operator
    weight: 0
menu_name: docs_v2022.03.29
section_menu_id: reference
url: /docs/v2022.03.29/reference/operator/
aliases:
- /docs/v2022.03.29/reference/operator/stash/
info:
  cli: v0.19.0
  community: v0.19.0
  elasticsearch:
  - 5.6.4-v16
  - 6.2.4-v16
  - 6.3.0-v16
  - 6.4.0-v16
  - 6.5.3-v16
  - 6.8.0-v16
  - 7.14.0-v2
  - 7.2.0-v16
  - 7.3.2-v16
  enterprise: v0.19.0
  etcd:
  - 3.5.0-v3
  installer: v2022.03.29
  mariadb:
  - 10.5.8-v9
  mongodb:
  - 3.4.17-v15
  - 3.4.22-v15
  - 3.6.13-v15
  - 3.6.8-v15
  - 4.0.11-v15
  - 4.0.3-v15
  - 4.0.5-v15
  - 4.1.13-v15
  - 4.1.4-v15
  - 4.1.7-v15
  - 4.2.3-v15
  - 4.4.6-v6
  - 5.0.3-v3
  mysql:
  - 5.7.25-v16
  - 8.0.14-v16
  - 8.0.21-v10
  - 8.0.3-v16
  nats:
  - 2.6.1-v3
  percona-xtradb:
  - 5.7-v11
  postgres:
  - 10.14-v14
  - 11.9-v14
  - 12.4-v14
  - 13.1-v11
  - 14.0-v3
  - 9.6.19-v14
  redis:
  - 5.0.13-v4
  - 6.2.5-v4
  ui-server: v0.2.0
  version: v2022.03.29
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

* [stash backup-pvc](/docs/v2022.03.29/reference/operator/stash_backup-pvc)	 - Takes a backup of Persistent Volume Claim
* [stash create-backupsession](/docs/v2022.03.29/reference/operator/stash_create-backupsession)	 - create a BackupSession
* [stash create-vs](/docs/v2022.03.29/reference/operator/stash_create-vs)	 - Take snapshot of PersistentVolumeClaims
* [stash forget](/docs/v2022.03.29/reference/operator/stash_forget)	 - Delete snapshots from a restic repository
* [stash restore](/docs/v2022.03.29/reference/operator/stash_restore)	 - Restore from backup
* [stash restore-pvc](/docs/v2022.03.29/reference/operator/stash_restore-pvc)	 - Takes a restore of Persistent Volume Claim
* [stash restore-vs](/docs/v2022.03.29/reference/operator/stash_restore-vs)	 - Restore PVC from VolumeSnapshot
* [stash run](/docs/v2022.03.29/reference/operator/stash_run)	 - Launch Stash Controller
* [stash run-backup](/docs/v2022.03.29/reference/operator/stash_run-backup)	 - Take backup of workload paths
* [stash run-hook](/docs/v2022.03.29/reference/operator/stash_run-hook)	 - Execute Backup or Restore Hooks
* [stash snapshots](/docs/v2022.03.29/reference/operator/stash_snapshots)	 - Get snapshots of restic repo
* [stash update-status](/docs/v2022.03.29/reference/operator/stash_update-status)	 - Update status of Repository, Backup/Restore Session
* [stash version](/docs/v2022.03.29/reference/operator/stash_version)	 - Prints binary version number.

