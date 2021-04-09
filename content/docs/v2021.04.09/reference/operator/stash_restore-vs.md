---
title: Restore-Vs
menu:
  docs_v2021.04.09:
    identifier: stash-restore-vs
    name: Restore-Vs
    parent: reference-operator
menu_name: docs_v2021.04.09
section_menu_id: reference
info:
  cli: v0.12.2
  community: v0.12.2
  elasticsearch:
  - 5.6.4-v8
  - 6.2.4-v8
  - 6.3.0-v8
  - 6.4.0-v8
  - 6.5.3-v8
  - 6.8.0-v8
  - 7.2.0-v8
  - 7.3.2-v8
  enterprise: v0.12.2
  installer: v2021.04.09
  mariadb:
  - 10.5.8-v2
  mongodb:
  - 3.4.17-v7
  - 3.4.22-v7
  - 3.6.13-v7
  - 3.6.8-v7
  - 4.0.11-v7
  - 4.0.3-v7
  - 4.0.5-v7
  - 4.1.13-v7
  - 4.1.4-v7
  - 4.1.7-v7
  - 4.2.3-v7
  mysql:
  - 5.7.25-v8
  - 8.0.14-v8
  - 8.0.21-v2
  - 8.0.3-v8
  percona-xtradb:
  - 5.7-v3
  postgres:
  - 10.14-v6
  - 11.9-v6
  - 12.4-v6
  - 13.1-v3
  - 9.6.19-v6
  version: v2021.04.09
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
      --alsologtostderr                  log to standard error as well as files
      --bypass-validating-webhook-xray   if true, bypasses validating webhook xray checks
      --enable-analytics                 Send analytical events to Google Analytics (default true)
      --log-flush-frequency duration     Maximum number of seconds between log flushes (default 5s)
      --log_backtrace_at traceLocation   when logging hits line file:N, emit a stack trace (default :0)
      --log_dir string                   If non-empty, write log files in this directory
      --logtostderr                      log to standard error instead of files (default true)
      --service-name string              Stash service name. (default "stash-operator")
      --stderrthreshold severity         logs at or above this threshold go to stderr
      --use-kubeapiserver-fqdn-for-aks   if true, uses kube-apiserver FQDN for AKS cluster to workaround https://github.com/Azure/AKS/issues/522 (default true)
  -v, --v Level                          log level for V logs
      --vmodule moduleSpec               comma-separated list of pattern=N settings for file-filtered logging
```

### SEE ALSO

* [stash](/docs/v2021.04.09/reference/operator/stash)	 - Stash by AppsCode - Backup your Kubernetes Volumes

