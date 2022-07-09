---
title: Update-Status
menu:
  docs_v2022.07.09:
    identifier: stash-update-status
    name: Update-Status
    parent: reference-operator
menu_name: docs_v2022.07.09
section_menu_id: reference
info:
  cli: v0.22.0
  community: v0.22.0
  elasticsearch:
  - 5.6.4-v18
  - 6.2.4-v18
  - 6.3.0-v18
  - 6.4.0-v18
  - 6.5.3-v18
  - 6.8.0-v18
  - 7.14.0-v4
  - 7.2.0-v18
  - 7.3.2-v18
  - 8.2.0-v1
  enterprise: v0.22.0
  etcd:
  - 3.5.0-v5
  installer: v2022.07.09
  kubedump:
  - 0.1.0-v1
  mariadb:
  - 10.5.8-v11
  mongodb:
  - 3.4.17-v18
  - 3.4.22-v18
  - 3.6.13-v18
  - 3.6.8-v18
  - 4.0.11-v18
  - 4.0.3-v18
  - 4.0.5-v18
  - 4.1.13-v18
  - 4.1.4-v18
  - 4.1.7-v18
  - 4.2.3-v18
  - 4.4.6-v9
  - 5.0.3-v6
  mysql:
  - 5.7.25-v18
  - 8.0.14-v18
  - 8.0.21-v12
  - 8.0.3-v18
  nats:
  - 2.6.1-v5
  - 2.8.2-v1
  percona-xtradb:
  - 5.7-v13
  postgres:
  - 10.14-v17
  - 11.9-v17
  - 12.4-v17
  - 13.1-v14
  - 14.0-v6
  - 9.6.19-v17
  redis:
  - 5.0.13-v6
  - 6.2.5-v6
  ui-server: v0.4.0
  version: v2022.07.09
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

* [stash](/docs/v2022.07.09/reference/operator/stash)	 - Stash by AppsCode - Backup your Kubernetes Volumes

