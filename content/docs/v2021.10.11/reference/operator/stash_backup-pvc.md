---
title: Backup-Pvc
menu:
  docs_v2021.10.11:
    identifier: stash-backup-pvc
    name: Backup-Pvc
    parent: reference-operator
menu_name: docs_v2021.10.11
section_menu_id: reference
info:
  cli: v0.16.0
  community: v0.16.0
  elasticsearch:
  - 5.6.4-v13
  - 6.2.4-v13
  - 6.3.0-v13
  - 6.4.0-v13
  - 6.5.3-v13
  - 6.8.0-v13
  - 7.2.0-v13
  - 7.3.2-v13
  enterprise: v0.16.0
  etcd:
  - 3.5.0
  installer: v2021.10.11
  mariadb:
  - 10.5.8-v6
  mongodb:
  - 3.4.17-v12
  - 3.4.22-v12
  - 3.6.13-v12
  - 3.6.8-v12
  - 4.0.11-v12
  - 4.0.3-v12
  - 4.0.5-v12
  - 4.1.13-v12
  - 4.1.4-v12
  - 4.1.7-v12
  - 4.2.3-v12
  - 4.4.6-v3
  - 5.0.3
  mysql:
  - 5.7.25-v13
  - 8.0.14-v13
  - 8.0.21-v7
  - 8.0.3-v13
  nats:
  - 2.6.1
  percona-xtradb:
  - 5.7-v8
  postgres:
  - 10.14-v11
  - 11.9-v11
  - 12.4-v11
  - 13.1-v8
  - "14.0"
  - 9.6.19-v11
  redis:
  - 5.0.13-v1
  - 6.2.5-v1
  version: v2021.10.11
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
      --bypass-validating-webhook-xray   if true, bypasses validating webhook xray checks
      --enable-analytics                 Send analytical events to Google Analytics (default true)
      --service-name string              Stash service name. (default "stash-operator")
      --use-kubeapiserver-fqdn-for-aks   if true, uses kube-apiserver FQDN for AKS cluster to workaround https://github.com/Azure/AKS/issues/522 (default true)
```

### SEE ALSO

* [stash](/docs/v2021.10.11/reference/operator/stash)	 - Stash by AppsCode - Backup your Kubernetes Volumes

