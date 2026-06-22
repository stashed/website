---
title: Update-Status
menu:
  docs_v2024.8.27:
    identifier: stash-update-status
    name: Update-Status
    parent: reference-operator
menu_name: docs_v2024.8.27
section_menu_id: reference
info:
  cli: v0.35.0
  community: v0.35.0
  elasticsearch:
  - 5.6.4-v32
  - 6.2.4-v32
  - 6.3.0-v32
  - 6.4.0-v32
  - 6.5.3-v32
  - 6.8.0-v32
  - 7.14.0-v18
  - 7.2.0-v32
  - 7.3.2-v32
  - 8.2.0-v15
  enterprise: v0.35.0
  etcd:
  - 3.5.0-v19
  installer: v2024.8.27
  kubedump:
  - 0.1.0-v15
  mariadb:
  - 10.5.8-v26
  mongodb:
  - 3.4.17-v32
  - 3.4.22-v32
  - 3.6.13-v32
  - 3.6.8-v32
  - 4.0.11-v32
  - 4.0.3-v32
  - 4.0.5-v32
  - 4.1.13-v32
  - 4.1.4-v32
  - 4.1.7-v32
  - 4.2.3-v32
  - 4.4.6-v23
  - 5.0.15-v5
  - 5.0.3-v20
  - 6.0.5-v8
  mysql:
  - 5.7.25-v32
  - 8.0.14-v32
  - 8.0.21-v26
  - 8.0.3-v32
  nats:
  - 2.6.1-v20
  - 2.8.2-v15
  percona-xtradb:
  - 5.7-v27
  postgres:
  - 10.14-v31
  - 11.9-v31
  - 12.4-v31
  - 13.1-v28
  - 14.0-v20
  - 15.1-v12
  - 16.1-v1
  - 9.6.19-v31
  redis:
  - 5.0.13-v20
  - 6.2.5-v20
  - 7.0.5-v13
  ui-server: v0.16.0
  vault:
  - 1.10.3-v12
  version: v2024.8.27
---

## stash update-status

Update status of Repository, Backup/Restore Session

```
stash update-status [flags]
```

### Options

```
      --backupsession string              Name of the Backup Session
      --bucket string                     Name of the cloud bucket/container (keep empty for local backend)
      --enable-cache                      Specify whether to enable caching for restic
      --endpoint string                   Endpoint for s3/s3 compatible backend or REST server URL
  -h, --help                              help for update-status
      --invoker-kind string               Type of the respective backup/restore invoker
      --invoker-name string               Name of the respective backup/restore invoker
      --kubeconfig string                 Path to kubeconfig file with authorization information (the master location is set by the master flag).
      --master string                     The address of the Kubernetes API server (overrides any value in kubeconfig)
      --max-connections int               Specify maximum concurrent connections for GCS, Azure and B2 backend
      --metrics-enabled                   Specify whether to export Prometheus metrics
      --metrics-labels strings            Labels to apply in exported metrics
      --metrics-pushgateway-url string    Pushgateway URL where the metrics will be pushed
      --namespace string                  Namespace of Backup/Restore Session (default "default")
      --output-dir string                 Directory where output.json file will be written (keep empty if you don't need to write output in file)
      --path string                       Directory inside the bucket where backed up data will be stored
      --provider string                   Backend provider (i.e. gcs, s3, azure etc)
      --region string                     Region for s3/s3 compatible backend
      --scratch-dir string                Temporary directory
      --storage-secret-name string        Name of the Repository
      --storage-secret-namespace string   Namespace of the Repository
      --target-kind string                Kind of the target
      --target-name string                Name of the target
      --target-namespace string           Namespace of the target
```

### Options inherited from parent commands

```
      --bypass-validating-webhook-xray   if true, bypasses validating webhook xray checks
      --use-kubeapiserver-fqdn-for-aks   if true, uses kube-apiserver FQDN for AKS cluster to workaround https://github.com/Azure/AKS/issues/522 (default true)
```

### SEE ALSO

* [stash](/docs/v2024.8.27/reference/operator/stash)	 - Stash by AppsCode - Backup your Kubernetes Volumes

