---
title: Update-Status
menu:
  docs_v2021.11.24:
    identifier: stash-update-status
    name: Update-Status
    parent: reference-operator
menu_name: docs_v2021.11.24
section_menu_id: reference
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

## stash update-status

Update status of Repository, Backup/Restore Session

```
stash update-status [flags]
```

### Options

```
      --backupsession string             Name of the Backup Session
      --bucket string                    Name of the cloud bucket/container (keep empty for local backend)
      --enable-cache                     Specify whether to enable caching for restic
      --endpoint string                  Endpoint for s3/s3 compatible backend or REST server URL
  -h, --help                             help for update-status
      --invoker-kind string              Type of the respective backup/restore invoker
      --invoker-name string              Name of the respective backup/restore invoker
      --kubeconfig string                Path to kubeconfig file with authorization information (the master location is set by the master flag).
      --master string                    The address of the Kubernetes API server (overrides any value in kubeconfig)
      --max-connections int              Specify maximum concurrent connections for GCS, Azure and B2 backend
      --metrics-enabled                  Specify whether to export Prometheus metrics
      --metrics-labels strings           Labels to apply in exported metrics
      --metrics-pushgateway-url string   Pushgateway URL where the metrics will be pushed
      --namespace string                 Namespace of Backup/Restore Session (default "default")
      --output-dir string                Directory where output.json file will be written (keep empty if you don't need to write output in file)
      --path string                      Directory inside the bucket where backed up data will be stored
      --provider string                  Backend provider (i.e. gcs, s3, azure etc)
      --region string                    Region for s3/s3 compatible backend
      --repository string                Name of the Repository
      --scratch-dir string               Temporary directory
      --secret-dir string                Directory where storage secret has been mounted
      --target-kind string               Kind of the target
      --target-name string               Name of the target
```

### Options inherited from parent commands

```
      --bypass-validating-webhook-xray   if true, bypasses validating webhook xray checks
      --service-name string              Stash service name. (default "stash-operator")
      --use-kubeapiserver-fqdn-for-aks   if true, uses kube-apiserver FQDN for AKS cluster to workaround https://github.com/Azure/AKS/issues/522 (default true)
```

### SEE ALSO

* [stash](/docs/v2021.11.24/reference/operator/stash)	 - Stash by AppsCode - Backup your Kubernetes Volumes

