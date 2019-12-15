---
title: Stash Backup
menu:
  docs_0.7.0-rc.2:
    identifier: stash-backup
    name: Stash Backup
    parent: reference
product_name: stash
menu_name: docs_0.7.0-rc.2
section_menu_id: reference
info:
  version: 0.7.0-rc.2
---

## stash backup

Run Stash Backup

### Synopsis

Run Stash Backup

```
stash backup [flags]
```

### Options

```
      --docker-registry string   Check job image registry. (default "appscode")
      --enable-rbac              Enable RBAC
  -h, --help                     help for backup
      --image-tag string         Check job image tag.
      --kubeconfig string        Path to kubeconfig file with authorization information (the master location is set by the master flag).
      --master string            The address of the Kubernetes API server (overrides any value in kubeconfig)
      --pushgateway-url string   URL of Prometheus pushgateway used to cache backup metrics
      --restic-name string       Name of the Restic used as configuration.
      --resync-period duration   If non-zero, will re-list this often. Otherwise, re-list will be delayed aslong as possible (until the upstream source closes the watch or times out. (default 5m0s)
      --run-via-cron             Run backup periodically via cron.
      --scratch-dir emptyDir     Directory used to store temporary files. Use an emptyDir in Kubernetes. (default "/tmp")
      --workload-kind string     Kind of workload where sidecar pod is added.
      --workload-name string     Name of workload where sidecar pod is added.
```

### Options inherited from parent commands

```
      --alsologtostderr                  log to standard error as well as files
      --enable-analytics                 Send analytical events to Google Analytics (default true)
      --log_backtrace_at traceLocation   when logging hits line file:N, emit a stack trace (default :0)
      --log_dir string                   If non-empty, write log files in this directory
      --logtostderr                      log to standard error instead of files
      --stderrthreshold severity         logs at or above this threshold go to stderr
  -v, --v Level                          log level for V logs
      --vmodule moduleSpec               comma-separated list of pattern=N settings for file-filtered logging
```

### SEE ALSO

* [stash](/docs/0.7.0-rc.2/reference/stash)	 - Stash by AppsCode - Backup your Kubernetes Volumes

