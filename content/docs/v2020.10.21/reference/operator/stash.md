---
title: Stash
menu:
  docs_v2020.10.21:
    identifier: stash
    name: Stash
    parent: reference-operator
    weight: 0
menu_name: docs_v2020.10.21
section_menu_id: reference
url: /docs/v2020.10.21/reference/operator/
aliases:
- /docs/v2020.10.21/reference/operator/stash/
info:
  catalog: v2020.10.21
  cli: v0.11.3
  community: v0.11.3
  elasticsearch:
  - 5.6.4-v3
  - 6.2.4-v3
  - 6.3.0-v3
  - 6.4.0-v3
  - 6.5.3-v3
  - 6.8.0-v3
  - 7.2.0-v3
  - 7.3.2-v3
  enterprise: v0.11.3
  installer: v0.11.3
  mongodb:
  - 3.4.17-v3
  - 3.4.22-v3
  - 3.6.13-v3
  - 3.6.8-v3
  - 4.0.11-v3
  - 4.0.3-v3
  - 4.0.5-v3
  - 4.1.13-v3
  - 4.1.4-v3
  - 4.1.7-v3
  - 4.2.3-v3
  mysql:
  - 5.7.25-v3
  - 8.0.14-v3
  - 8.0.3-v3
  percona-xtradb:
  - 5.7-v3
  postgres:
  - 10.14.0-v1
  - 11.9.0-v1
  - 12.4.0-v1
  - 9.6.19-v1
  version: v2020.10.21
---

## stash

Stash by AppsCode - Backup your Kubernetes Volumes

### Synopsis

Stash is a Kubernetes operator for restic. For more information, visit here: https://appscode.com/products/stash

### Options

```
      --alsologtostderr                  log to standard error as well as files
      --bypass-validating-webhook-xray   if true, bypasses validating webhook xray checks
      --enable-analytics                 Send analytical events to Google Analytics (default true)
  -h, --help                             help for stash
      --log-flush-frequency duration     Maximum number of seconds between log flushes (default 5s)
      --log_backtrace_at traceLocation   when logging hits line file:N, emit a stack trace (default :0)
      --log_dir string                   If non-empty, write log files in this directory
      --logtostderr                      log to standard error instead of files (default true)
      --service-name string              Stash service name. (default "stash-operator")
      --stderrthreshold severity         logs at or above this threshold go to stderr
      --use-kubeapiserver-fqdn-for-aks   if true, uses kube-apiserver FQDN for AKS cluster to workaround https://github.com/Azure/AKS/issues/522 (default true)
  -v, --v Level                          log level for V logs
      --vmodule moduleSpec               comma-separated list of pattern=N settings for file-filtered logging
```

### SEE ALSO

* [stash backup](/docs/v2020.10.21/reference/operator/stash_backup)	 - Run Stash Backup
* [stash backup-pvc](/docs/v2020.10.21/reference/operator/stash_backup-pvc)	 - Takes a backup of Persistent Volume Claim
* [stash check](/docs/v2020.10.21/reference/operator/stash_check)	 - Check restic backup
* [stash create-backupsession](/docs/v2020.10.21/reference/operator/stash_create-backupsession)	 - create a BackupSession
* [stash create-vs](/docs/v2020.10.21/reference/operator/stash_create-vs)	 - Take snapshot of PersistentVolumeClaims
* [stash forget](/docs/v2020.10.21/reference/operator/stash_forget)	 - Delete snapshots from a restic repository
* [stash recover](/docs/v2020.10.21/reference/operator/stash_recover)	 - Recover restic backup
* [stash restore](/docs/v2020.10.21/reference/operator/stash_restore)	 - Restore from backup
* [stash restore-pvc](/docs/v2020.10.21/reference/operator/stash_restore-pvc)	 - Takes a restore of Persistent Volume Claim
* [stash restore-vs](/docs/v2020.10.21/reference/operator/stash_restore-vs)	 - Restore PVC from VolumeSnapshot
* [stash run](/docs/v2020.10.21/reference/operator/stash_run)	 - Launch Stash Controller
* [stash run-backup](/docs/v2020.10.21/reference/operator/stash_run-backup)	 - Take backup of workload paths
* [stash run-hook](/docs/v2020.10.21/reference/operator/stash_run-hook)	 - Execute Backup or Restore Hooks
* [stash scaledown](/docs/v2020.10.21/reference/operator/stash_scaledown)	 - Scale down workload
* [stash snapshots](/docs/v2020.10.21/reference/operator/stash_snapshots)	 - Get snapshots of restic repo
* [stash update-status](/docs/v2020.10.21/reference/operator/stash_update-status)	 - Update status of Repository, Backup/Restore Session
* [stash version](/docs/v2020.10.21/reference/operator/stash_version)	 - Prints binary version number.

