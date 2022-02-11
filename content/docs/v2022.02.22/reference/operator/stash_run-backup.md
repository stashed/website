---
title: Run-Backup
menu:
  docs_v2022.02.22:
    identifier: stash-run-backup
    name: Run-Backup
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

## stash run-backup

Take backup of workload paths

```
stash run-backup [flags]
```

### Options

```
      --enable-cache             Specify whether to enable caching for restic (default true)
  -h, --help                     help for run-backup
      --host string              Name of the host that will be backed up
      --invoker-kind string      Kind of the backup invoker
      --invoker-name string      Name of the respective backup invoker
      --kubeconfig string        Path to kubeconfig file with authorization information (the master location is set by the master flag).
      --master string            The address of the Kubernetes API server (overrides any value in kubeconfig)
      --max-connections int      Specify maximum concurrent connections for GCS, Azure and B2 backend
      --metrics-enabled          Specify whether to export Prometheus metrics
      --pushgateway-url string   URL of Prometheus pushgateway used to cache backup metrics
      --target-kind string       Kind of the Target
      --target-name string       Name of the Target
```

### Options inherited from parent commands

```
      --bypass-validating-webhook-xray   if true, bypasses validating webhook xray checks
      --use-kubeapiserver-fqdn-for-aks   if true, uses kube-apiserver FQDN for AKS cluster to workaround https://github.com/Azure/AKS/issues/522 (default true)
```

### SEE ALSO

* [stash](/docs/v2022.02.22/reference/operator/stash)	 - Stash by AppsCode - Backup your Kubernetes Volumes

