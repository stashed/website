---
title: Restore
menu:
  docs_v2022.12.11:
    identifier: stash-restore
    name: Restore
    parent: reference-operator
menu_name: docs_v2022.12.11
section_menu_id: reference
info:
  cli: v0.24.0
  community: v0.24.0
  elasticsearch:
  - 5.6.4-v20
  - 6.2.4-v20
  - 6.3.0-v20
  - 6.4.0-v20
  - 6.5.3-v20
  - 6.8.0-v20
  - 7.14.0-v6
  - 7.2.0-v20
  - 7.3.2-v20
  - 8.2.0-v3
  enterprise: v0.24.0
  etcd:
  - 3.5.0-v7
  installer: v2022.12.11
  kubedump:
  - 0.1.0-v3
  mariadb:
  - 10.5.8-v13
  mongodb:
  - 3.4.17-v20
  - 3.4.22-v20
  - 3.6.13-v20
  - 3.6.8-v20
  - 4.0.11-v20
  - 4.0.3-v20
  - 4.0.5-v20
  - 4.1.13-v20
  - 4.1.4-v20
  - 4.1.7-v20
  - 4.2.3-v20
  - 4.4.6-v11
  - 5.0.3-v8
  mysql:
  - 5.7.25-v20
  - 8.0.14-v20
  - 8.0.21-v14
  - 8.0.3-v20
  nats:
  - 2.6.1-v8
  - 2.8.2-v3
  percona-xtradb:
  - 5.7-v15
  postgres:
  - 10.14-v19
  - 11.9-v19
  - 12.4-v19
  - 13.1-v16
  - 14.0-v8
  - "15.1"
  - 9.6.19-v19
  redis:
  - 5.0.13-v8
  - 6.2.5-v8
  - 7.0.5-v1
  ui-server: v0.6.0
  vault:
  - 1.10.3
  version: v2022.12.11
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

* [stash](/docs/v2022.12.11/reference/operator/stash)	 - Stash by AppsCode - Backup your Kubernetes Volumes

