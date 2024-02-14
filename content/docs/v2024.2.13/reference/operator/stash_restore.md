---
title: Restore
menu:
  docs_v2024.2.13:
    identifier: stash-restore
    name: Restore
    parent: reference-operator
menu_name: docs_v2024.2.13
section_menu_id: reference
info:
  cli: v0.33.0
  community: v0.33.0
  elasticsearch:
  - 5.6.4-v30
  - 6.2.4-v30
  - 6.3.0-v30
  - 6.4.0-v30
  - 6.5.3-v30
  - 6.8.0-v30
  - 7.14.0-v16
  - 7.2.0-v30
  - 7.3.2-v30
  - 8.2.0-v13
  enterprise: v0.33.0
  etcd:
  - 3.5.0-v17
  installer: v2024.2.13
  kubedump:
  - 0.1.0-v13
  mariadb:
  - 10.5.8-v24
  mongodb:
  - 3.4.17-v30
  - 3.4.22-v30
  - 3.6.13-v30
  - 3.6.8-v30
  - 4.0.11-v30
  - 4.0.3-v30
  - 4.0.5-v30
  - 4.1.13-v30
  - 4.1.4-v30
  - 4.1.7-v30
  - 4.2.3-v30
  - 4.4.6-v21
  - 5.0.15-v3
  - 5.0.3-v18
  - 6.0.5-v6
  mysql:
  - 5.7.25-v30
  - 8.0.14-v30
  - 8.0.21-v24
  - 8.0.3-v30
  nats:
  - 2.6.1-v18
  - 2.8.2-v13
  percona-xtradb:
  - 5.7-v25
  postgres:
  - 10.14-v29
  - 11.9-v29
  - 12.4-v29
  - 13.1-v26
  - 14.0-v18
  - 15.1-v10
  - 9.6.19-v29
  redis:
  - 5.0.13-v18
  - 6.2.5-v18
  - 7.0.5-v11
  ui-server: v0.14.0
  vault:
  - 1.10.3-v10
  version: v2024.2.13
---

## stash restore

Restore from backup

```
stash restore [flags]
```

### Options

```
      --backoff-max-wait duration   Maximum wait for initial response from kube apiserver; 0 disables the timeout
      --enable-cache                Specify whether to enable caching for restic (default true)
  -h, --help                        help for restore
      --invoker-kind string         Kind of the respective restore invoker
      --invoker-name string         Name of the respective restore invoker
      --kubeconfig string           Path to kubeconfig file with authorization information (the master location is set by the master flag).
      --master string               The address of the Kubernetes API server (overrides any value in kubeconfig)
      --max-connections int         Specify maximum concurrent connections for GCS, Azure and B2 backend
      --metrics-enabled             Specify whether to export Prometheus metrics
      --pushgateway-url string      Pushgateway URL where the metrics will be pushed
      --restore-model string        Specify whether using job or init-container to restore (default init-container) (default "init-container")
      --target-kind string          Kind of the Target
      --target-name string          Name of the Target
      --target-namespace string     Namespace of the Target
```

### Options inherited from parent commands

```
      --bypass-validating-webhook-xray   if true, bypasses validating webhook xray checks
      --use-kubeapiserver-fqdn-for-aks   if true, uses kube-apiserver FQDN for AKS cluster to workaround https://github.com/Azure/AKS/issues/522 (default true)
```

### SEE ALSO

* [stash](/docs/v2024.2.13/reference/operator/stash)	 - Stash by AppsCode - Backup your Kubernetes Volumes

