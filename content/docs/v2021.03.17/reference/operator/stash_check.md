---
title: Check
menu:
  docs_v2021.03.17:
    identifier: stash-check
    name: Check
    parent: reference-operator
menu_name: docs_v2021.03.17
section_menu_id: reference
info:
  cli: v0.12.0
  community: v0.12.0
  elasticsearch:
  - 5.6.4-v7
  - 6.2.4-v7
  - 6.3.0-v7
  - 6.4.0-v7
  - 6.5.3-v7
  - 6.8.0-v7
  - 7.2.0-v7
  - 7.3.2-v7
  enterprise: v0.12.0
  installer: v0.12.0
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
  - 5.7-v2
  postgres:
  - 10.14-v5
  - 11.9-v5
  - 12.4-v5
  - 13.1-v2
  - 9.6.19-v5
  version: v2021.03.17
---

## stash check

Check restic backup

```
stash check [flags]
```

### Options

```
  -h, --help                  help for check
      --host-name string      Host name for workload.
      --kubeconfig string     Path to kubeconfig file with authorization information (the master location is set by the master flag).
      --master string         The address of the Kubernetes API server (overrides any value in kubeconfig)
      --restic-name string    Name of the Restic CRD.
      --smart-prefix string   Smart prefix for workload
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

* [stash](/docs/v2021.03.17/reference/operator/stash)	 - Stash by AppsCode - Backup your Kubernetes Volumes

