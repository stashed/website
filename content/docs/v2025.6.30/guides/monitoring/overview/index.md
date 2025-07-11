---
title: Monitoring Overview | Stash
description: A general overview of monitoring Stash
menu:
  docs_v2025.6.30:
    identifier: monitoring-overview
    name: Overview
    parent: monitoring
    weight: 10
product_name: stash
menu_name: docs_v2025.6.30
section_menu_id: guides
info:
  cli: v0.40.0
  community: v0.40.0
  elasticsearch:
  - 5.6.4-v36
  - 6.2.4-v36
  - 6.3.0-v36
  - 6.4.0-v36
  - 6.5.3-v36
  - 6.8.0-v36
  - 7.14.0-v22
  - 7.2.0-v36
  - 7.3.2-v36
  - 8.2.0-v19
  enterprise: v0.40.0
  etcd:
  - 3.5.0-v23
  installer: v2025.6.30
  kubedump:
  - 0.2.0-v4
  mariadb:
  - 10.5.8-v30
  mongodb:
  - 3.4.17-v37
  - 3.4.22-v37
  - 3.6.13-v37
  - 3.6.8-v37
  - 4.0.11-v37
  - 4.0.3-v37
  - 4.0.5-v37
  - 4.1.13-v37
  - 4.1.4-v37
  - 4.1.7-v37
  - 4.2.3-v37
  - 4.4.6-v28
  - 5.0.15-v10
  - 5.0.3-v25
  - 6.0.5-v13
  mysql:
  - 5.7.25-v37
  - 8.0.14-v36
  - 8.0.21-v30
  - 8.0.3-v36
  nats:
  - 2.6.1-v24
  - 2.8.2-v19
  percona-xtradb:
  - 5.7-v31
  postgres:
  - 10.14-v35
  - 11.9-v35
  - 12.4-v35
  - 13.1-v32
  - 14.0-v24
  - 15.1-v16
  - 16.1-v5
  - 17.2-v3
  - 9.6.19-v35
  redis:
  - 5.0.13-v24
  - 6.2.5-v24
  - 7.0.5-v17
  ui-server: v0.21.0
  vault:
  - 1.10.3-v16
  version: v2025.6.30
---

> New to Stash? Please start [here](/docs/v2025.6.30/concepts/README).

# Monitoring Stash

Stash has native support for monitoring via [Prometheus](https://prometheus.io/). You can use builtin [Prometheus](https://github.com/prometheus/prometheus) scraper or [prometheus-operator](https://github.com/prometheus-operator/prometheus-operator) to monitor Stash. This tutorial will show you how Prometheus monitoring works with Stash, what metrics Stash exports, and how to enable monitoring.

## How Prometheus monitoring works

Stash monitoring metrics comes from two sources. The first one is [Prometheus PushGateway](https://github.com/prometheus/pushgateway) that running as sidecar of Stash operator pod. The backup and restore processes pushes their metrics in this pushgateway. The second metrics source is [Panopticon](https://blog.byte.builders/post/introducing-panopticon/) which is a generic state metric exporter for Kubernetes developed by AppsCode. It watches Stash CRDs and export necessary metrics.

The following diagram shows the logical structure of the Stash monitoring flow.

<figure align="center">
  <img alt="Stash Monitoring Flow" src="/docs/v2025.6.30/guides/monitoring/overview/images/monitoring-structure.svg">
<figcaption align="center">Fig: Monitoring process in Stash</figcaption>
</figure>

Stash operator runs two containers. The `operator` container runs controllers and other necessary stuff and the `pushgateway` container runs [prom/pushgateway](https://hub.docker.com/r/prom/pushgateway) image. Stash sidecar from different workloads and backup/restore jobs pushes its metrics to this pushgateway. The pushgateway exposes the metrics at `/metrics` path of `:56789` port. Then, a Prometheus server scrapes these metrics through `stash` or `stash-enterprise` Service and acts as a data source of [Grafana](https://grafana.com/) dashboard. Stash operator itself also provides some valuable metrics at `/metrics` path of `:8443` port.

The Panopticon tool runs as a separate workload. It watches for Stash CRDs and exports relevant metrics.

## Available Metrics

Stash exports metrics for the backup process, restore process, repository status, etc. This section will list the metrics exported by Stash for different processes.

### Backup Metrics

This section lists the metrics available for Stash.

**Backup Session Metrics:**

A backup session represents a backup run. Stash exports the following metrics regarding the overall backup session.

| Metric Name                              | Usage                                                                            | Community | Enterprise |
| ---------------------------------------- | -------------------------------------------------------------------------------- | --------- | ---------- |
| `stash_backupsession_created`            | Indicates the timestamp when the BackupSession was created                       | &#10007;  | &#10003;   |
| `stash_backupsession_info`               | Metrics about the BackupSession owner, phase etc.                                | &#10007;  | &#10003;   |
| `stash_backup_session_success`           | Indicates whether the entire backup session was succeeded or not                 | &#10003;  | &#10003;   |
| `stash_backup_target_count_total`        | Indicates the total number of targets that were backed up in this backup session | &#10003;  | &#10003;   |
| `stash_backup_session_duration_seconds`  | Indicates total time taken to complete the entire backup session                 | &#10003;  | &#10003;   |
| `stash_backup_last_success_time_seconds` | Indicates the time(in Unix epoch) when the last backup session was succeeded     | &#10003;  | &#10003;   |

**Backup Target Metrics:**
In each backup session, Stash takes backup of one or more targets. Stash exports the following metrics for the individual backup target.

| Metric Name                                     | Usage                                                                                  | Community | Enterprise |
| ----------------------------------------------- | -------------------------------------------------------------------------------------- | --------- | ---------- |
| `stash_backupconfiguration_created`             | Indicates the timestamp when the BackupConfiguration was created                       | &#10007;  | &#10003;   |
| `stash_backupconfiguration_info`                | Metrics about backup target, schedule, driver etc.                                     | &#10007;  | &#10003;   |
| `stash_backupconfiguration_conditions`          | Metric about condition of backup setup                                                 | &#10007;  | &#10003;   |
| `stash_backup_target_success`                   | Indicates whether the backup for a target has succeeded or not                         | &#10003;  | &#10003;   |
| `stash_backup_target_host_count_total`          | Indicates the total number of hosts that was backed up for this target                 | &#10003;  | &#10003;   |
| `stash_backup_target_last_success_time_seconds` | Indicates the time (in Unix epoch) when the last backup was successful for this target | &#10003;  | &#10003;   |


**Backup Host Metrics:**

Stash may take a backup of multiple hosts for a single target. The following metrics are available for the individual backup hosts.

| Metric Name                                      | Usage                                                                        | Community | Enterprise |
| ------------------------------------------------ | ---------------------------------------------------------------------------- | --------- | ---------- |
| `stash_backup_host_backup_success`               | Indicates whether the backup for a host succeeded or not                     | &#10003;  | &#10003;   |
| `stash_backup_host_data_size_bytes`              | Total size of the target data to backup for a host (in bytes)                | &#10003;  | &#10003;   |
| `stash_backup_host_data_uploaded_bytes`          | Amount of data uploaded to the repository for a host (in bytes)              | &#10003;  | &#10003;   |
| `stash_backup_host_files_total`                  | Total number of files that has been backed up for a host                     | &#10003;  | &#10003;   |
| `stash_backup_host_files_new`                    | Total number of new files that has been created since last backup for a host | &#10003;  | &#10003;   |
| `stash_backup_host_files_modified`               | Total number of files that has been modified since last backup for a host    | &#10003;  | &#10003;   |
| `stash_backup_host_files_unmodified`             | Total number of files that has not been changed since last backup for a host | &#10003;  | &#10003;   |
| `stash_backup_host_backup_duration_seconds`      | Indicates total time taken to complete the backup process for a host         | &#10003;  | &#10003;   |
| `stash_backup_host_data_processing_time_seconds` | Total time taken to process the target data for a host                       | &#10003;  | &#10003;   |

### Repository Metrics

Stash exports the following metrics for a repository.

| Metric Name                         | Usage                                                                                                 | Community | Enterprise |
| ----------------------------------- | ----------------------------------------------------------------------------------------------------- | --------- | ---------- |
| `stash_repository_created`          | Indicates the timestamp when the Repository has been created                                          | &#10007;  | &#10003;   |
| `stash_repository_integrity`        | Result of repository integrity check after the last backup                                            | &#10003;  | &#10003;   |
| `stash_repository_size_bytes`       | Indicates size of repository after last backup (in bytes)                                             | &#10003;  | &#10003;   |
| `stash_repository_snapshot_count`   | Indicates the number of snapshots stored in the repository                                            | &#10003;  | &#10003;   |
| `stash_repository_snapshot_cleaned` | Indicates the number of old snapshots cleaned up according to retention policy on last backup session | &#10003;  | &#10003;   |

### Restore Metrics

This section lists the metrics Stash exports for the restore process.

**Restore Session Metrics:**

A restore session represents a restore run. Stash exports the following metrics regarding the overall restore process.

| Metric Name                              | Usage                                                                            | Community | Enterprise |
| ---------------------------------------- | -------------------------------------------------------------------------------- | --------- | ---------- |
| `stash_restoresession_created`           | Indicates the timestamp when the RestoreSession has been created                 | &#10007;  | &#10003;   |
| `stash_restoresession_info`              | Metrics about RestoreSession's target, phase etc                                 | &#10007;  | &#10003;   |
| `stash_restore_session_success`          | Indicates whether the entire restore session was succeeded or not                | &#10003;  | &#10003;   |
| `stash_restore_session_duration_seconds` | Indicates the total time taken to complete the entire restore session            | &#10003;  | &#10003;   |
| `stash_restore_target_count_total`       | Indicates the total number of targets that were restored in this restore session | &#10003;  | &#10003;   |

**Restore Target Metrics:**

Stash restore one or more targets in each restore run. Stash exports the following metrics regarding a restore target.

| Metric Name                             | Usage                                                                          | Community | Enterprise |
| --------------------------------------- | ------------------------------------------------------------------------------ | --------- | ---------- |
| `stash_restore_target_success`          | Indicates whether the restore for a target has succeeded or not                | &#10003;  | &#10003;   |
| `stash_restore_target_host_count_total` | Indicates the total number of hosts that were restored for this restore target | &#10003;  | &#10003;   |

**Restore Host Metrics:**

Stash may restore multiple hosts for a single target. The following metrics are available for the individual restore host.

| Metric Name                                   | Usage                                                                     | Community | Enterprise |
| --------------------------------------------- | ------------------------------------------------------------------------- | --------- | ---------- |
| `stash_restore_host_restore_success`          | Indicates whether the restore process was succeeded for a host            | &#10003;  | &#10003;   |
| `stash_restore_host_restore_duration_seconds` | Indicates the total time taken to complete the restore process for a host | &#10003;  | &#10003;   |

### Operator Metrics

Following metrics are available for the Stash operator. These metrics are accessible through `api` endpoint of `stash` service.

| Metric Name                                       | Usage                                                                                                                 |
| ------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| `apiserver_audit_event_total`                     | Counter of audit events generated and sent to the audit backend.                                                      |
| `apiserver_client_certificate_expiration_seconds` | Distribution of the remaining lifetime on the certificate used to authenticate a request.                             |
| `apiserver_current_inflight_requests`             | Maximal number of currently used inflight request limit of this apiserver per request kind in last second.            |
| `apiserver_request_count`                         | Counter of apiserver requests broken out for each verb, API resource, client, and HTTP response contentType and code. |
| `apiserver_request_latencies`                     | Response latency distribution in microseconds for each verb, resource, and subresource.                               |
| `apiserver_request_latencies_summary`             | Response latency summary in microseconds for each verb, resource, and subresource.                                    |
| `authenticated_user_requests`                     | Counter of authenticated requests broken out by username.                                                             |

### Pushgateway Metrics

The Pushgateway itself also exports some metrics related to Pushgateway build info, HTTP requests handled by it, Go process that running inside it, and CPU & Memory consumed by it, etc.

**Build and Last Activity:**

| Metric Name              | Usage                                                                                                                    |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------ |
| `pushgateway_build_info` | A metric with a constant '1' value labeled by version, revision, branch, and goversion from which pushgateway was built. |
| `push_time_seconds`      | Last Unix time when this group was changed in the Pushgateway.                                                           |

**CPU & Memory Related Metrics:**

| Metric Name                     | Usage                                                  |
| ------------------------------- | ------------------------------------------------------ |
| `process_cpu_seconds_total`     | Total user and system CPU time spent in seconds.       |
| `process_max_fds`               | Maximum number of open file descriptors.               |
| `process_open_fds`              | Number of open file descriptors.                       |
| `process_resident_memory_bytes` | Resident memory size in bytes.                         |
| `process_start_time_seconds`    | Start time of the process since unix epoch in seconds. |
| `process_virtual_memory_bytes`  | Virtual memory size in bytes.                          |

**Go Environment Related Metrics:**

| Metric Name                             | Usage                                                                                       |
| --------------------------------------- | ------------------------------------------------------------------------------------------- |
| `go_gc_duration_seconds`                | A summary of the GC invocation durations.                                                   |
| `go_goroutines`                         | Number of goroutines that currently exist.                                                  |
| `go_info`                               | Information about the Go environment.                                                       |
| `go_memstats_alloc_bytes`               | Number of bytes allocated and still in use.                                                 |
| `go_memstats_alloc_bytes_total`         | Total number of bytes allocated, even if freed.                                             |
| `go_memstats_buck_hash_sys_bytes`       | Number of bytes used by the profiling bucket hash table.                                    |
| `go_memstats_frees_total`               | Total number of frees.                                                                      |
| `go_memstats_gc_cpu_fraction`           | The fraction of this program's available CPU time used by the GC since the program started. |
| `go_memstats_gc_sys_bytes`              | Number of bytes used for garbage collection system metadata.                                |
| `go_memstats_heap_alloc_bytes`          | Number of heap bytes allocated and still in use.                                            |
| `go_memstats_heap_idle_bytes`           | Number of heap bytes waiting to be used.                                                    |
| `go_memstats_heap_inuse_bytes`          | Number of heap bytes that are in use.                                                       |
| `go_memstats_heap_objects`              | Number of allocated objects.                                                                |
| `go_memstats_heap_released_bytes_total` | Total number of heap bytes released to OS.                                                  |
| `go_memstats_heap_sys_bytes`            | Number of heap bytes obtained from system.                                                  |
| `go_memstats_last_gc_time_seconds`      | Number of seconds since 1970 of last garbage collection.                                    |
| `go_memstats_lookups_total`             | Total number of pointer lookups.                                                            |
| `go_memstats_mallocs_total`             | Total number of mallocs.                                                                    |
| `go_memstats_mcache_inuse_bytes`        | Number of bytes in use by mcache structures.                                                |
| `go_memstats_mcache_sys_bytes`          | Number of bytes used for mcache structures obtained from system.                            |
| `go_memstats_mspan_inuse_bytes`         | Number of bytes in use by mspan structures.                                                 |
| `go_memstats_mspan_sys_bytes`           | Number of bytes used for mspan structures obtained from system.                             |
| `go_memstats_next_gc_bytes`             | Number of heap bytes when next garbage collection will take place.                          |
| `go_memstats_other_sys_bytes`           | Number of bytes used for other system allocations.                                          |
| `go_memstats_stack_inuse_bytes`         | Number of bytes in use by the stack allocator.                                              |
| `go_memstats_stack_sys_bytes`           | Number of bytes obtained from system for stack allocator.                                   |
| `go_memstats_sys_bytes`                 | Number of bytes obtained by system. Sum of all system allocations.                          |
| `go_threads`                            | Number of OS threads created.                                                               |

**HTTP Request Related Metrics:**

| Metric Name                          | Usage                                       |
| ------------------------------------ | ------------------------------------------- |
| `http_request_duration_microseconds` | The HTTP request latencies in microseconds. |
| `http_request_size_bytes`            | The HTTP request sizes in bytes.            |
| `http_requests_total`                | Total number of HTTP requests made.         |
| `http_response_size_bytes`           | The HTTP response sizes in bytes.           |

## How to Enable Monitoring

You have to enable Prometheus monitoring during installing / upgrading Stash. The following parameters are available to configure monitoring in Stash.

| Helm Values                                         | Acceptable Values                                   | Default                                                    | Usage                                                                                                                                                               |
| --------------------------------------------------- | --------------------------------------------------- | ---------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `stash-enterprise.monitoring.agent`                 | `prometheus.io/builtin` or `prometheus.io/operator` | `none`                                                     | Specify which monitoring agent to use for monitoring Stash.                                                                                                         |
| `stash-enterprise.monitoring.backup`                | `true` or `false`                                   | `false`                                                    | Specify whether to monitor Stash backup and restore.                                                                                                                |
| `stash-enterprise.monitoring.operator`              | `true` or `false`                                   | `false`                                                    | Specify whether to monitor Stash operator.                                                                                                                          |
| `stash-enterprise.monitoring.serviceMonitor.labels` | any label                                           | `app: <generated app name>` and `release: <release name>`. | Specify the labels for ServiceMonitor. Prometheus crd will select ServiceMonitor using these labels. Only usable when monitoring agent is `prometheus.io/operator`. |

>Use `stash-community` instead of `stash-enterprise` if you are using Stash Community edition.

The following instruction show example of enabling monitoring in Stash for the Prometheus server deployed with Prometheus Operator. You can check the [Builtin Prometheus](/docs/v2025.6.30/guides/monitoring/prom-builtin/) scraper guide if you are managing your Prometheus server manually.

<ul class="nav nav-tabs" id="installerTab" role="tablist">
  <li class="nav-item">
    <a class="nav-link active" id="new-installer-tab" data-toggle="tab" href="#new-installation-tab" role="tab" aria-controls="new-installation-tab" aria-selected="true">New Installation</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" id="existing-installation" data-toggle="tab" href="#existing-installation-tab" role="tab" aria-controls="existing-installation-tab" aria-selected="false">Existing Installation</a>
  </li>
</ul>
<div class="tab-content" id="installerTabContent">
  <div class="tab-pane fade show active" id="new-installation-tab" role="tabpanel" aria-labelledby="new-installation-tab">

### New Installation

If you haven't installed Stash yet, run the following command to enable Prometheus monitoring during installation

**Helm 3:**

```bash
$ helm install stash oci://ghcr.io/appscode-charts/stash \
  --version {{< param "info.version" >}} \
  --namespace stash --create-namespace \
  --set features.enterprise=true \
  --set stash-enterprise.monitoring.agent=prometheus.io/operator \
  --set stash-enterprise.monitoring.backup=true \
  --set stash-enterprise. monitoring.operator=true \
  --set stash-enterprise.monitoring.serviceMonitor.labels.release=prometheus-stack \
  --set-file global.license=/path/to/license-file.txt \
  --wait --burst-limit=10000 --debug
```

**YAML (with Helm 3):**

```bash
$ helm template stash oci://ghcr.io/appscode-charts/stash \
  --version {{< param "info.version" >}} \
  --namespace stash --create-namespace \
  --no-hooks \
  --set features.enterprise=true \
  --set stash-enterprise.monitoring.agent=prometheus.io/operator \
  --set stash-enterprise.monitoring.backup=true \
  --set stash-enterprise.monitoring.operator=true \
  --set stash-enterprise.monitoring.serviceMonitor.labels.release=prometheus-stack \
  --set-file global.license=/path/to/license-file.txt | kubectl apply -f -
```

</div>
<div class="tab-pane fade" id="existing-installation-tab" role="tabpanel" aria-labelledby="existing-installation-tab">

### Existing Installation

If you have installed Stash already in your cluster but didn't enable monitoring during installation, you can use `helm upgrade` command to enable monitoring in the existing installation.

**Helm 3:**

```bash
$ helm upgrade -i stash oci://ghcr.io/appscode-charts/stash \
  --version {{< param "info.version" >}} \
  --namespace stash --create-namespace \
  --reuse-values \
  --set features.enterprise=true \
  --set stash-enterprise.monitoring.agent=prometheus.io/operator \
  --set stash-enterprise.monitoring.backup=true \
  --set stash-enterprise.monitoring.operator=true \
  --set stash-enterprise.monitoring.serviceMonitor.labels.release=prometheus-stack \
  --wait --burst-limit=10000 --debug
```

**YAML (with Helm 3):**

```bash
$ helm template stash oci://ghcr.io/appscode-charts/stash \
  --version {{< param "info.version" >}} \
  --namespace stash --create-namespace \
  --no-hooks \
  --reuse-values \
  --set features.enterprise=true \
  --set stash-enterprise.monitoring.agent=prometheus.io/operator \
  --set stash-enterprise.monitoring.backup=true \
  --set stash-enterprise.monitoring.operator=true \
  --set stash-enterprise.monitoring.serviceMonitor.labels.release=prometheus-stack | kubectl apply -f -
```

</div>
</div>
