---
title: Restore-Pvc
menu:
  docs_v2023.10.9:
    identifier: stash-restore-pvc
    name: Restore-Pvc
    parent: reference-operator
menu_name: docs_v2023.10.9
section_menu_id: reference
info:
  cli: v0.32.0
  community: v0.32.0
  elasticsearch:
  - 5.6.4-v28
  - 6.2.4-v28
  - 6.3.0-v28
  - 6.4.0-v28
  - 6.5.3-v28
  - 6.8.0-v28
  - 7.14.0-v14
  - 7.2.0-v28
  - 7.3.2-v28
  - 8.2.0-v11
  enterprise: v0.32.0
  etcd:
  - 3.5.0-v15
  installer: v2023.10.9
  kubedump:
  - 0.1.0-v11
  mariadb:
  - 10.5.8-v21
  mongodb:
  - 3.4.17-v28
  - 3.4.22-v28
  - 3.6.13-v28
  - 3.6.8-v28
  - 4.0.11-v28
  - 4.0.3-v28
  - 4.0.5-v28
  - 4.1.13-v28
  - 4.1.4-v28
  - 4.1.7-v28
  - 4.2.3-v28
  - 4.4.6-v19
  - 5.0.15-v1
  - 5.0.3-v16
  - 6.0.5-v4
  mysql:
  - 5.7.25-v28
  - 8.0.14-v28
  - 8.0.21-v22
  - 8.0.3-v28
  nats:
  - 2.6.1-v16
  - 2.8.2-v11
  percona-xtradb:
  - 5.7-v23
  postgres:
  - 10.14-v27
  - 11.9-v27
  - 12.4-v27
  - 13.1-v24
  - 14.0-v16
  - 15.1-v8
  - 9.6.19-v27
  redis:
  - 5.0.13-v16
  - 6.2.5-v16
  - 7.0.5-v9
  ui-server: v0.13.0
  vault:
  - 1.10.3-v8
  version: v2023.10.9
---

## stash restore-pvc

Takes a restore of Persistent Volume Claim

```
stash restore-pvc [flags]
```

### Options

```
      --args strings                      Arguments to pass to the restore command.
      --bucket string                     Name of the cloud bucket/container (keep empty for local backend)
      --enable-cache                      Specify whether to enable caching for restic
      --endpoint string                   Endpoint for s3/s3 compatible backend or REST server URL
      --exclude strings                   List of pattern for directory/file to ignore during restore. Stash will not restore those files that matches these patterns.
  -h, --help                              help for restore-pvc
      --hostname string                   Name of the host machine (default "host-0")
      --include strings                   List of pattern for directory/file to restore. Stash will restore only those files that matches these patterns.
      --invoker-kind string               Kind of the backup invoker
      --invoker-name string               Name of the respective backup invoker
      --kubeconfig string                 Path to kubeconfig file with authorization information (the master location is set by the master flag).
      --master string                     The address of the Kubernetes API server (overrides any value in kubeconfig)
      --max-connections int               Specify maximum concurrent connections for GCS, Azure and B2 backend
      --output-dir string                 Directory where output.json file will be written (keep empty if you don't need to write output in file)
      --path string                       Directory inside the bucket where backed up data will be stored
      --provider string                   Backend provider (i.e. gcs, s3, azure etc)
      --region string                     Region for s3/s3 compatible backend
      --restore-paths strings             List of paths to restore
      --scratch-dir string                Temporary directory (default "/tmp")
      --snapshots strings                 List of snapshots to be restored
      --storage-secret-name string        Name of the StorageSecret
      --storage-secret-namespace string   Namespace of the StorageSecret
      --target-kind string                Kind of the Target
      --target-name string                Name of the Target
      --target-namespace string           Namespace of the Target
```

### Options inherited from parent commands

```
      --bypass-validating-webhook-xray   if true, bypasses validating webhook xray checks
      --use-kubeapiserver-fqdn-for-aks   if true, uses kube-apiserver FQDN for AKS cluster to workaround https://github.com/Azure/AKS/issues/522 (default true)
```

### SEE ALSO

* [stash](/docs/v2023.10.9/reference/operator/stash)	 - Stash by AppsCode - Backup your Kubernetes Volumes

