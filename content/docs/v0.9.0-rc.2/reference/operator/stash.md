---
title: Stash
menu:
  docs_v0.9.0-rc.2:
    identifier: stash
    name: Stash
    parent: operator
    weight: 0
product_name: stash
section_menu_id: reference
menu_name: docs_v0.9.0-rc.2
url: /docs/v0.9.0-rc.2/reference/operator/
aliases:
- /docs/v0.9.0-rc.2/reference/operator/operator/
info:
  catalog: v0.1.0
  cli: v0.2.0
  version: v0.9.0-rc.2
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
      --enable-status-subresource        If true, uses sub resource for crds.
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

* [stash backup](/docs/v0.9.0-rc.2/reference/operator/stash_backup)	 - Run Stash Backup
* [stash backup-pvc](/docs/v0.9.0-rc.2/reference/operator/stash_backup-pvc)	 - Takes a backup of Persistent Volume Claim
* [stash check](/docs/v0.9.0-rc.2/reference/operator/stash_check)	 - Check restic backup
* [stash create-backupsession](/docs/v0.9.0-rc.2/reference/operator/stash_create-backupsession)	 - create a BackupSession
* [stash create-vs](/docs/v0.9.0-rc.2/reference/operator/stash_create-vs)	 - Take snapshot of PersistentVolumeClaims
* [stash forget](/docs/v0.9.0-rc.2/reference/operator/stash_forget)	 - Delete snapshots from a restic repository
* [stash recover](/docs/v0.9.0-rc.2/reference/operator/stash_recover)	 - Recover restic backup
* [stash restore](/docs/v0.9.0-rc.2/reference/operator/stash_restore)	 - Restore from backup
* [stash restore-pvc](/docs/v0.9.0-rc.2/reference/operator/stash_restore-pvc)	 - Takes a restore of Persistent Volume Claim
* [stash restore-vs](/docs/v0.9.0-rc.2/reference/operator/stash_restore-vs)	 - Restore PVC from VolumeSnapshot
* [stash run](/docs/v0.9.0-rc.2/reference/operator/stash_run)	 - Launch Stash Controller
* [stash run-backup](/docs/v0.9.0-rc.2/reference/operator/stash_run-backup)	 - Take backup of workload directories
* [stash scaledown](/docs/v0.9.0-rc.2/reference/operator/stash_scaledown)	 - Scale down workload
* [stash snapshots](/docs/v0.9.0-rc.2/reference/operator/stash_snapshots)	 - Get snapshots of restic repo
* [stash update-status](/docs/v0.9.0-rc.2/reference/operator/stash_update-status)	 - Update status of Repository, Backup/Restore Session
* [stash version](/docs/v0.9.0-rc.2/reference/operator/stash_version)	 - Prints binary version number.

