---
title: Run-Hook
menu:
  docs_v2021.03.11:
    identifier: stash-run-hook
    name: Run-Hook
    parent: reference-operator
menu_name: docs_v2021.03.11
section_menu_id: reference
info:
  catalog: v2021.03.11
  cli: v0.11.11
  community: v0.11.11
  elasticsearch:
  - 5.6.4-v7
  - 6.2.4-v7
  - 6.3.0-v7
  - 6.4.0-v7
  - 6.5.3-v7
  - 6.8.0-v7
  - 7.2.0-v7
  - 7.3.2-v7
  enterprise: v0.11.11
  installer: v0.11.11
  mariadb:
  - 10.5.8-v1
  mongodb:
  - 3.4.17-v6
  - 3.4.22-v6
  - 3.6.13-v6
  - 3.6.8-v6
  - 4.0.11-v6
  - 4.0.3-v6
  - 4.0.5-v6
  - 4.1.13-v6
  - 4.1.4-v6
  - 4.1.7-v6
  - 4.2.3-v6
  mysql:
  - 5.7.25-v7
  - 8.0.14-v7
  - 8.0.21-v1
  - 8.0.3-v7
  percona-xtradb:
  - 5.7.0-v2
  postgres:
  - 10.14.0-v5
  - 11.9.0-v5
  - 12.4.0-v5
  - 13.1.0-v2
  - 9.6.19-v5
  version: v2021.03.11
---

## stash run-hook

Execute Backup or Restore Hooks

```
stash run-hook [flags]
```

### Options

```
      --backupsession string             Name of the respective BackupSession object
  -h, --help                             help for run-hook
      --hook-type string                 Type of hook to execute
      --hostname string                  Name of the host that is being backed up or restored (default "host-0")
      --invoker-kind string              Type of the backup invoker
      --invoker-name string              Name of the respective backup invoker
      --kubeconfig string                Path to kubeconfig file with authorization information (the master location is set by the master flag).
      --master string                    The address of the Kubernetes API server (overrides any value in kubeconfig)
      --metrics-enabled                  Specify whether to export Prometheus metrics
      --metrics-labels strings           Labels to apply in exported metrics
      --metrics-pushgateway-url string   Pushgateway URL where the metrics will be pushed
      --output-dir string                Directory where output.json file will be written (keep empty if you don't need to write output in file)
      --target-kind string               Kind of the Target
      --target-name string               Name of the Target
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

* [stash](/docs/v2021.03.11/reference/operator/stash)	 - Stash by AppsCode - Backup your Kubernetes Volumes

