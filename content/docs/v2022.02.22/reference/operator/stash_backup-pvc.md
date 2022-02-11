---
title: Backup-Pvc
menu:
  docs_v2022.02.22:
    identifier: stash-backup-pvc
    name: Backup-Pvc
    parent: reference-operator
menu_name: docs_v2022.02.22
section_menu_id: reference
info:
  cli: v0.18.0
  community: v0.18.0
  elasticsearch:
  - 5.6.4-v15
  - 6.2.4-v15
  - 6.3.0-v15
  - 6.4.0-v15
  - 6.5.3-v15
  - 6.8.0-v15
  - 7.14.0-v1
  - 7.2.0-v15
  - 7.3.2-v15
  enterprise: v0.18.0
  etcd:
  - 3.5.0-v2
  installer: v2022.02.22
  mariadb:
  - 10.5.8-v8
  mongodb:
  - 3.4.17-v14
  - 3.4.22-v14
  - 3.6.13-v14
  - 3.6.8-v14
  - 4.0.11-v14
  - 4.0.3-v14
  - 4.0.5-v14
  - 4.1.13-v14
  - 4.1.4-v14
  - 4.1.7-v14
  - 4.2.3-v14
  - 4.4.6-v5
  - 5.0.3-v2
  mysql:
  - 5.7.25-v15
  - 8.0.14-v15
  - 8.0.21-v9
  - 8.0.3-v15
  nats:
  - 2.6.1-v2
  percona-xtradb:
  - 5.7-v10
  postgres:
  - 10.14-v13
  - 11.9-v13
  - 12.4-v13
  - 13.1-v10
  - 14.0-v2
  - 9.6.19-v13
  redis:
  - 5.0.13-v3
  - 6.2.5-v3
  ui-server: v0.1.0
  version: v2022.02.22
---

## stash backup-pvc

Takes a backup of Persistent Volume Claim

```
stash backup-pvc [flags]
```

### Options

```
      --backup-paths strings              List of paths to backup
      --backupsession string              Name of the Backup Session
      --bucket string                     Name of the cloud bucket/container (keep empty for local backend)
      --enable-cache                      Specify whether to enable caching for restic
      --endpoint string                   Endpoint for s3/s3 compatible backend or REST server URL
      --exclude strings                   List of pattern for directory/file to ignore during backup. Stash will not backup those files that matches these patterns.
  -h, --help                              help for backup-pvc
      --hostname string                   Name of the host machine (default "host-0")
      --invoker-kind string               Kind of the backup invoker
      --invoker-name string               Name of the respective backup invoker
      --kubeconfig string                 Path to kubeconfig file with authorization information (the master location is set by the master flag).
      --master string                     The address of the Kubernetes API server (overrides any value in kubeconfig)
      --max-connections int               Specify maximum concurrent connections for GCS, Azure and B2 backend
      --output-dir string                 Directory where output.json file will be written (keep empty if you don't need to write output in file)
      --path string                       Directory inside the bucket where backed up data will be stored
      --provider string                   Backend provider (i.e. gcs, s3, azure etc)
      --region string                     Region for s3/s3 compatible backend
      --retention-dry-run                 Specify whether to test retention policy without deleting actual data
      --retention-keep-daily int          Specify value for retention strategy
      --retention-keep-hourly int         Specify value for retention strategy
      --retention-keep-last int           Specify value for retention strategy
      --retention-keep-monthly int        Specify value for retention strategy
      --retention-keep-tags strings       Specify value for retention strategy
      --retention-keep-weekly int         Specify value for retention strategy
      --retention-keep-yearly int         Specify value for retention strategy
      --retention-prune                   Specify whether to prune old snapshot data
      --scratch-dir string                Temporary directory (default "/tmp")
      --storage-secret-name string        Name of the StorageSecret
      --storage-secret-namespace string   Namespace of the StorageSecret
      --target-kind string                Kind of the Target
      --target-name string                Name of the Target
```

### Options inherited from parent commands

```
      --bypass-validating-webhook-xray   if true, bypasses validating webhook xray checks
      --use-kubeapiserver-fqdn-for-aks   if true, uses kube-apiserver FQDN for AKS cluster to workaround https://github.com/Azure/AKS/issues/522 (default true)
```

### SEE ALSO

* [stash](/docs/v2022.02.22/reference/operator/stash)	 - Stash by AppsCode - Backup your Kubernetes Volumes

