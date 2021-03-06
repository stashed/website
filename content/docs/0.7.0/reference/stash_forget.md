---
title: Stash Forget
menu:
  docs_0.7.0:
    identifier: stash-forget
    name: Stash Forget
    parent: reference
product_name: stash
menu_name: docs_0.7.0
section_menu_id: reference
info:
  version: 0.7.0
---

## stash forget

Delete snapshots from a restic repository

### Synopsis

Delete snapshots from a restic repository

```
stash forget [snapshotID ...] [flags]
```

### Options

```
  -h, --help                help for forget
      --kubeconfig string   Path to kubeconfig file with authorization information (the master location is set by the master flag).
      --master string       The address of the Kubernetes API server (overrides any value in kubeconfig)
      --repo-name string    Name of the Repository CRD.
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

* [stash](/docs/0.7.0/reference/stash)	 - Stash by AppsCode - Backup your Kubernetes Volumes

