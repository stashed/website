---
title: Stash
menu:
  docs_v2021.11.24:
    identifier: stash
    name: Stash
    parent: reference-operator
    weight: 0
menu_name: docs_v2021.11.24
section_menu_id: reference
url: /docs/v2021.11.24/reference/operator/
aliases:
- /docs/v2021.11.24/reference/operator/stash/
info:
  cli: v0.17.0
  community: v0.17.0
  elasticsearch:
  - 5.6.4-v14
  - 6.2.4-v14
  - 6.3.0-v14
  - 6.4.0-v14
  - 6.5.3-v14
  - 6.8.0-v14
  - 7.14.0
  - 7.2.0-v14
  - 7.3.2-v14
  enterprise: v0.17.0
  etcd:
  - 3.5.0-v1
  installer: v2021.11.24
  mariadb:
  - 10.5.8-v7
  mongodb:
  - 3.4.17-v13
  - 3.4.22-v13
  - 3.6.13-v13
  - 3.6.8-v13
  - 4.0.11-v13
  - 4.0.3-v13
  - 4.0.5-v13
  - 4.1.13-v13
  - 4.1.4-v13
  - 4.1.7-v13
  - 4.2.3-v13
  - 4.4.6-v4
  - 5.0.3-v1
  mysql:
  - 5.7.25-v14
  - 8.0.14-v14
  - 8.0.21-v8
  - 8.0.3-v14
  nats:
  - 2.6.1-v1
  percona-xtradb:
  - 5.7-v9
  postgres:
  - 10.14-v12
  - 11.9-v12
  - 12.4-v12
  - 13.1-v9
  - 14.0-v1
  - 9.6.19-v12
  redis:
  - 5.0.13-v2
  - 6.2.5-v2
  version: v2021.11.24
---

## stash

Stash by AppsCode - Backup your Kubernetes Volumes

### Synopsis

Stash is a Kubernetes operator for restic. For more information, visit here: https://appscode.com/products/stash

### Options

```
      --bypass-validating-webhook-xray   if true, bypasses validating webhook xray checks
  -h, --help                             help for stash
      --service-name string              Stash service name. (default "stash-operator")
      --use-kubeapiserver-fqdn-for-aks   if true, uses kube-apiserver FQDN for AKS cluster to workaround https://github.com/Azure/AKS/issues/522 (default true)
```

### SEE ALSO

* [stash backup](/docs/v2021.11.24/reference/operator/stash_backup)	 - Run Stash Backup
* [stash backup-pvc](/docs/v2021.11.24/reference/operator/stash_backup-pvc)	 - Takes a backup of Persistent Volume Claim
* [stash check](/docs/v2021.11.24/reference/operator/stash_check)	 - Check restic backup
* [stash create-backupsession](/docs/v2021.11.24/reference/operator/stash_create-backupsession)	 - create a BackupSession
* [stash create-vs](/docs/v2021.11.24/reference/operator/stash_create-vs)	 - Take snapshot of PersistentVolumeClaims
* [stash forget](/docs/v2021.11.24/reference/operator/stash_forget)	 - Delete snapshots from a restic repository
* [stash recover](/docs/v2021.11.24/reference/operator/stash_recover)	 - Recover restic backup
* [stash restore](/docs/v2021.11.24/reference/operator/stash_restore)	 - Restore from backup
* [stash restore-pvc](/docs/v2021.11.24/reference/operator/stash_restore-pvc)	 - Takes a restore of Persistent Volume Claim
* [stash restore-vs](/docs/v2021.11.24/reference/operator/stash_restore-vs)	 - Restore PVC from VolumeSnapshot
* [stash run](/docs/v2021.11.24/reference/operator/stash_run)	 - Launch Stash Controller
* [stash run-backup](/docs/v2021.11.24/reference/operator/stash_run-backup)	 - Take backup of workload paths
* [stash run-hook](/docs/v2021.11.24/reference/operator/stash_run-hook)	 - Execute Backup or Restore Hooks
* [stash scaledown](/docs/v2021.11.24/reference/operator/stash_scaledown)	 - Scale down workload
* [stash snapshots](/docs/v2021.11.24/reference/operator/stash_snapshots)	 - Get snapshots of restic repo
* [stash update-status](/docs/v2021.11.24/reference/operator/stash_update-status)	 - Update status of Repository, Backup/Restore Session
* [stash version](/docs/v2021.11.24/reference/operator/stash_version)	 - Prints binary version number.

