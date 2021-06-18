---
title: Stash
menu:
  docs_v2021.06.23:
    identifier: stash
    name: Stash
    parent: reference-operator
    weight: 0
menu_name: docs_v2021.06.23
section_menu_id: reference
url: /docs/v2021.06.23/reference/operator/
aliases:
- /docs/v2021.06.23/reference/operator/stash/
info:
  cli: v0.14.1
  community: v0.14.1
  elasticsearch:
  - 5.6.4-v11
  - 6.2.4-v11
  - 6.3.0-v11
  - 6.4.0-v11
  - 6.5.3-v11
  - 6.8.0-v11
  - 7.2.0-v11
  - 7.3.2-v11
  enterprise: v0.14.1
  installer: v2021.06.23
  mariadb:
  - 10.5.8-v4
  mongodb:
  - 3.4.17-v10
  - 3.4.22-v10
  - 3.6.13-v10
  - 3.6.8-v10
  - 4.0.11-v10
  - 4.0.3-v10
  - 4.0.5-v10
  - 4.1.13-v10
  - 4.1.4-v10
  - 4.1.7-v10
  - 4.2.3-v10
  - 4.4.6-v1
  mysql:
  - 5.7.25-v11
  - 8.0.14-v11
  - 8.0.21-v5
  - 8.0.3-v11
  percona-xtradb:
  - 5.7-v6
  postgres:
  - 10.14-v9
  - 11.9-v9
  - 12.4-v9
  - 13.1-v6
  - 9.6.19-v9
  version: v2021.06.23
---

## stash

Stash by AppsCode - Backup your Kubernetes Volumes

### Synopsis

Stash is a Kubernetes operator for restic. For more information, visit here: https://appscode.com/products/stash

### Options

```
      --bypass-validating-webhook-xray   if true, bypasses validating webhook xray checks
      --enable-analytics                 Send analytical events to Google Analytics (default true)
  -h, --help                             help for stash
      --service-name string              Stash service name. (default "stash-operator")
      --use-kubeapiserver-fqdn-for-aks   if true, uses kube-apiserver FQDN for AKS cluster to workaround https://github.com/Azure/AKS/issues/522 (default true)
```

### SEE ALSO

* [stash backup](/docs/v2021.06.23/reference/operator/stash_backup)	 - Run Stash Backup
* [stash backup-pvc](/docs/v2021.06.23/reference/operator/stash_backup-pvc)	 - Takes a backup of Persistent Volume Claim
* [stash check](/docs/v2021.06.23/reference/operator/stash_check)	 - Check restic backup
* [stash create-backupsession](/docs/v2021.06.23/reference/operator/stash_create-backupsession)	 - create a BackupSession
* [stash create-vs](/docs/v2021.06.23/reference/operator/stash_create-vs)	 - Take snapshot of PersistentVolumeClaims
* [stash forget](/docs/v2021.06.23/reference/operator/stash_forget)	 - Delete snapshots from a restic repository
* [stash recover](/docs/v2021.06.23/reference/operator/stash_recover)	 - Recover restic backup
* [stash restore](/docs/v2021.06.23/reference/operator/stash_restore)	 - Restore from backup
* [stash restore-pvc](/docs/v2021.06.23/reference/operator/stash_restore-pvc)	 - Takes a restore of Persistent Volume Claim
* [stash restore-vs](/docs/v2021.06.23/reference/operator/stash_restore-vs)	 - Restore PVC from VolumeSnapshot
* [stash run](/docs/v2021.06.23/reference/operator/stash_run)	 - Launch Stash Controller
* [stash run-backup](/docs/v2021.06.23/reference/operator/stash_run-backup)	 - Take backup of workload paths
* [stash run-hook](/docs/v2021.06.23/reference/operator/stash_run-hook)	 - Execute Backup or Restore Hooks
* [stash scaledown](/docs/v2021.06.23/reference/operator/stash_scaledown)	 - Scale down workload
* [stash snapshots](/docs/v2021.06.23/reference/operator/stash_snapshots)	 - Get snapshots of restic repo
* [stash update-status](/docs/v2021.06.23/reference/operator/stash_update-status)	 - Update status of Repository, Backup/Restore Session
* [stash version](/docs/v2021.06.23/reference/operator/stash_version)	 - Prints binary version number.

