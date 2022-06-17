---
title: Run-Backup
menu:
  docs_v2022.06.21:
    identifier: stash-run-backup
    name: Run-Backup
    parent: reference-operator
menu_name: docs_v2022.06.21
section_menu_id: reference
info:
  cli: v0.21.0
  community: v0.21.0
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
  enterprise: v0.21.0
  etcd:
  - 3.5.0-v5
  installer: v2022.06.21
  kubedump:
  - 0.1.0-v1
  mariadb:
  - 10.5.8-v11
  mongodb:
  - 3.4.17-v17
  - 3.4.22-v17
  - 3.6.13-v17
  - 3.6.8-v17
  - 4.0.11-v17
  - 4.0.3-v17
  - 4.0.5-v17
  - 4.1.13-v17
  - 4.1.4-v17
  - 4.1.7-v17
  - 4.2.3-v17
  - 4.4.6-v8
  - 5.0.3-v5
  mysql:
  - 5.7.25-v18
  - 8.0.14-v18
  - 8.0.21-v12
  - 8.0.3-v18
  nats:
  - 2.6.1-v6
  - 2.8.2-v1
  percona-xtradb:
  - 5.7-v13
  postgres:
  - 10.14-v16
  - 11.9-v16
  - 12.4-v16
  - 13.1-v13
  - 14.0-v5
  - 9.6.19-v16
  redis:
  - 5.0.13-v6
  - 6.2.5-v6
  ui-server: v0.3.0
  version: v2022.06.21
---

## stash run-backup

Take backup of workload paths

```
stash run-backup [flags]
```

### Options

```
      --enable-cache              Specify whether to enable caching for restic (default true)
  -h, --help                      help for run-backup
      --host string               Name of the host that will be backed up
      --invoker-kind string       Kind of the backup invoker
      --invoker-name string       Name of the respective backup invoker
      --kubeconfig string         Path to kubeconfig file with authorization information (the master location is set by the master flag).
      --master string             The address of the Kubernetes API server (overrides any value in kubeconfig)
      --max-connections int       Specify maximum concurrent connections for GCS, Azure and B2 backend
      --metrics-enabled           Specify whether to export Prometheus metrics
      --pushgateway-url string    URL of Prometheus pushgateway used to cache backup metrics
      --target-kind string        Kind of the Target
      --target-name string        Name of the Target
      --target-namespace string   Namespace of the Target
```

### Options inherited from parent commands

```
      --bypass-validating-webhook-xray   if true, bypasses validating webhook xray checks
      --use-kubeapiserver-fqdn-for-aks   if true, uses kube-apiserver FQDN for AKS cluster to workaround https://github.com/Azure/AKS/issues/522 (default true)
```

### SEE ALSO

* [stash](/docs/v2022.06.21/reference/operator/stash)	 - Stash by AppsCode - Backup your Kubernetes Volumes

