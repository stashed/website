---
title: Backup-Pvc
menu:
  docs_v2021.04.12:
    identifier: stash-backup-pvc
    name: Backup-Pvc
    parent: reference-operator
menu_name: docs_v2021.04.12
section_menu_id: reference
info:
  cli: v0.12.3
  community: v0.12.3
  elasticsearch:
  - 5.6.4-v9
  - 6.2.4-v9
  - 6.3.0-v9
  - 6.4.0-v9
  - 6.5.3-v9
  - 6.8.0-v9
  - 7.2.0-v9
  - 7.3.2-v9
  enterprise: v0.12.3
  installer: v2021.04.12
  mariadb:
  - 10.5.8-v3
  mongodb:
  - 3.4.17-v8
  - 3.4.22-v8
  - 3.6.13-v8
  - 3.6.8-v8
  - 4.0.11-v8
  - 4.0.3-v8
  - 4.0.5-v8
  - 4.1.13-v8
  - 4.1.4-v8
  - 4.1.7-v8
  - 4.2.3-v8
  mysql:
  - 5.7.25-v9
  - 8.0.14-v9
  - 8.0.21-v3
  - 8.0.3-v9
  percona-xtradb:
  - 5.7-v4
  postgres:
  - 10.14-v7
  - 11.9-v7
  - 12.4-v7
  - 13.1-v4
  - 9.6.19-v7
  version: v2021.04.12
---

## stash backup-pvc

Takes a backup of Persistent Volume Claim

```
stash backup-pvc [flags]
```

### Options

```
      --backup-paths strings          List of paths to backup
      --backupsession string          Name of the Backup Session
      --bucket string                 Name of the cloud bucket/container (keep empty for local backend)
      --enable-cache                  Specify whether to enable caching for restic
      --endpoint string               Endpoint for s3/s3 compatible backend or REST server URL
      --exclude strings               List of pattern for directory/file to ignore during backup. Stash will not backup those files that matches these patterns.
  -h, --help                          help for backup-pvc
      --hostname string               Name of the host machine (default "host-0")
      --invoker-kind string           Kind of the backup invoker
      --invoker-name string           Name of the respective backup invoker
      --kubeconfig string             Path to kubeconfig file with authorization information (the master location is set by the master flag).
      --master string                 The address of the Kubernetes API server (overrides any value in kubeconfig)
      --max-connections int           Specify maximum concurrent connections for GCS, Azure and B2 backend
      --output-dir string             Directory where output.json file will be written (keep empty if you don't need to write output in file)
      --path string                   Directory inside the bucket where backed up data will be stored
      --provider string               Backend provider (i.e. gcs, s3, azure etc)
      --region string                 Region for s3/s3 compatible backend
      --retention-dry-run             Specify whether to test retention policy without deleting actual data
      --retention-keep-daily int      Specify value for retention strategy
      --retention-keep-hourly int     Specify value for retention strategy
      --retention-keep-last int       Specify value for retention strategy
      --retention-keep-monthly int    Specify value for retention strategy
      --retention-keep-tags strings   Specify value for retention strategy
      --retention-keep-weekly int     Specify value for retention strategy
      --retention-keep-yearly int     Specify value for retention strategy
      --retention-prune               Specify whether to prune old snapshot data
      --scratch-dir string            Temporary directory (default "/tmp")
      --secret-dir string             Directory where storage secret has been mounted
      --target-kind string            Kind of the Target
      --target-name string            Name of the Target
```

### Options inherited from parent commands

```
      --alsologtostderr                  log to standard error as well as files
      --bypass-validating-webhook-xray   if true, bypasses validating webhook xray checks
      --enable-analytics                 Send analytical events to Google Analytics (default true)
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

* [stash](/docs/v2021.04.12/reference/operator/stash)	 - Stash by AppsCode - Backup your Kubernetes Volumes

