---
title: Run-Backup
menu:
  docs_v2025.10.17:
    identifier: stash-run-backup
    name: Run-Backup
    parent: reference-operator
menu_name: docs_v2025.10.17
section_menu_id: reference
info:
  cli: v0.42.0
  community: v0.42.0
  elasticsearch:
  - 5.6.4-v38
  - 6.2.4-v38
  - 6.3.0-v38
  - 6.4.0-v38
  - 6.5.3-v38
  - 6.8.0-v38
  - 7.14.0-v24
  - 7.2.0-v38
  - 7.3.2-v38
  - 8.2.0-v21
  enterprise: v0.42.0
  etcd:
  - 3.5.0-v25
  installer: v2025.10.17
  kubedump:
  - 0.2.0-v6
  mariadb:
  - 10.6.23-v1
  mongodb:
  - 3.4.17-v39
  - 3.4.22-v39
  - 3.6.13-v39
  - 3.6.8-v39
  - 4.0.11-v39
  - 4.0.3-v39
  - 4.0.5-v39
  - 4.1.13-v39
  - 4.1.4-v39
  - 4.1.7-v39
  - 4.2.3-v39
  - 4.4.6-v30
  - 5.0.15-v12
  - 5.0.3-v27
  - 6.0.5-v15
  mysql:
  - 5.7.25-v39
  - 8.0.14-v38
  - 8.0.21-v32
  - 8.0.3-v38
  nats:
  - 2.6.1-v26
  - 2.8.2-v21
  percona-xtradb:
  - 5.7-v33
  postgres:
  - 10.14-v37
  - 11.9-v37
  - 12.4-v37
  - 13.1-v34
  - 14.0-v26
  - 15.1-v18
  - 16.1-v7
  - 17.2-v5
  - 9.6.19-v37
  redis:
  - 5.0.13-v26
  - 6.2.5-v26
  - 7.0.5-v19
  ui-server: v0.23.0
  vault:
  - 1.10.3-v18
  version: v2025.10.17
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

* [stash](/docs/v2025.10.17/reference/operator/stash)	 - Stash by AppsCode - Backup your Kubernetes Volumes

