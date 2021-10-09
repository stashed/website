---
title: Restore-Vs
menu:
  docs_v2021.10.11:
    identifier: stash-restore-vs
    name: Restore-Vs
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

## stash restore-vs

Restore PVC from VolumeSnapshot

```
stash restore-vs [flags]
```

### Options

```
  -h, --help                     help for restore-vs
      --invoker-kind string      Kind of the restore invoker
      --invoker-name string      Name of the respective restore invoker
      --kubeconfig string        Path to kubeconfig file with authorization information (the master location is set by the master flag).
      --master string            The address of the Kubernetes API server (overrides any value in kubeconfig)
      --metrics-enabled          Specify whether to export Prometheus metrics (default true)
      --pushgateway-url string   Pushgateway URL where the metrics will be pushed
      --target-kind string       Kind of the Target
      --target-name string       Name of the Target
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

