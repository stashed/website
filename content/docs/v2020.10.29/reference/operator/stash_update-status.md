---
title: Update-Status
menu:
  docs_v2020.10.29:
    identifier: stash-update-status
    name: Update-Status
    parent: reference-operator
menu_name: docs_v2020.10.29
section_menu_id: reference
info:
  catalog: v2020.10.29
  cli: v0.11.4
  community: v0.11.4
  elasticsearch:
  - 5.6.4-v3
  - 6.2.4-v3
  - 6.3.0-v3
  - 6.4.0-v3
  - 6.5.3-v3
  - 6.8.0-v3
  - 7.2.0-v3
  - 7.3.2-v3
  enterprise: v0.11.4
  installer: v0.11.4
  mongodb:
  - 3.4.17-v3
  - 3.4.22-v3
  - 3.6.13-v3
  - 3.6.8-v3
  - 4.0.11-v3
  - 4.0.3-v3
  - 4.0.5-v3
  - 4.1.13-v3
  - 4.1.4-v3
  - 4.1.7-v3
  - 4.2.3-v3
  mysql:
  - 5.7.25-v3
  - 8.0.14-v3
  - 8.0.3-v3
  percona-xtradb:
  - 5.7-v3
  postgres:
  - 10.14.0-v1
  - 11.9.0-v1
  - 12.4.0-v1
  - 9.6.19-v1
  version: v2020.10.29
---

## stash update-status

Update status of Repository, Backup/Restore Session

### Synopsis

Update status of Repository, Backup/Restore Session

```
stash update-status [flags]
```

### Options

```
      --backupsession string             Name of the Backup Session
      --bucket string                    Name of the cloud bucket/container (keep empty for local backend)
      --enable-cache                     Specify whether to enable caching for restic
      --endpoint string                  Endpoint for s3/s3 compatible backend or REST server URL
  -h, --help                             help for update-status
      --invoker-kind string              Type of the respective backup/restore invoker
      --invoker-name string              Name of the respective backup/restore invoker
      --kubeconfig string                Path to kubeconfig file with authorization information (the master location is set by the master flag).
      --master string                    The address of the Kubernetes API server (overrides any value in kubeconfig)
      --max-connections int              Specify maximum concurrent connections for GCS, Azure and B2 backend
      --metrics-enabled                  Specify whether to export Prometheus metrics
      --metrics-labels strings           Labels to apply in exported metrics
      --metrics-pushgateway-url string   Pushgateway URL where the metrics will be pushed
      --namespace string                 Namespace of Backup/Restore Session (default "default")
      --output-dir string                Directory where output.json file will be written (keep empty if you don't need to write output in file)
      --path string                      Directory inside the bucket where backed up data will be stored
      --prom-job-name string             Metrics job name (default "stash-prom-metrics")
      --provider string                  Backend provider (i.e. gcs, s3, azure etc)
      --region string                    Region for s3/s3 compatible backend
      --repository string                Name of the Repository
      --scratch-dir string               Temporary directory
      --secret-dir string                Directory where storage secret has been mounted
      --target-kind string               Kind of the target
      --target-name string               Name of the target
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

* [stash](/docs/v2020.10.29/reference/operator/stash)	 - Stash by AppsCode - Backup your Kubernetes Volumes

