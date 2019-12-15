---
title: Monitoring | Stash
description: monitoring of Stash
menu:
  docs_0.6.1:
    identifier: monitoring-stash
    name: Monitoring
    parent: guides
    weight: 45
product_name: stash
menu_name: docs_0.6.1
section_menu_id: guides
info:
  version: 0.6.1
---

> New to Stash? Please start [here](/docs/0.6.1/concepts/README).

# Monitoring Stash

Stash has native support for monitoring via Prometheus.

## Monitoring Stash Operator
Stash operator exposes Prometheus native monitoring data via `/metrics` endpoint on `:56790` port. You can setup a [CoreOS Prometheus ServiceMonitor](https://github.com/coreos/prometheus-operator) using `stash-operator` service.

## Monitoring Backup Operation
Since backup operations are run as cron jobs, Stash can use [Prometheus Pushgateway](https://github.com/prometheus/pushgateway) cache metrics for backup operation. The installation scripts for Stash operator deploys a Prometheus Pushgateway as a sidecar container. You can configure a Prometheus server to scrape this Pushgateway via `stash-operator` service on port `:56789`. Backup operations send the following metrics to this Pushgateway:

 - `restic_session_success{job="<restic.namespace>-<restic.name>", app="<workload>"}`: Indicates if session was successfully completed
 - `restic_session_fail{job="<restic.namespace>-<restic.name>", app="<workload>"}`: Indicates if session failed
 - `restic_session_duration_seconds_total{job="<restic.namespace>-<restic.name>", app="<workload>"}`: Total seconds taken to complete restic session
 - `restic_session_duration_seconds{job="<restic.namespace>-<restic.name>", app="<workload>", filegroup="dir1", op="backup|forget"}`: Total seconds taken to complete restic session

## Next Steps

- Learn how to use Stash to backup a Kubernetes deployment [here](/docs/0.6.1/guides/backup).
- Learn about the details of Restic CRD [here](/docs/0.6.1/concepts/crds/restic).
- To restore a backup see [here](/docs/0.6.1/guides/restore).
- Learn about the details of Recovery CRD [here](/docs/0.6.1/concepts/crds/recovery).
- To run backup in offline mode see [here](/docs/0.6.1/guides/offline_backup)
- See the list of supported backends and how to configure them [here](/docs/0.6.1/guides/backends).
- See working examples for supported workload types [here](/docs/0.6.1/guides/workloads).
- Learn about how to configure [RBAC roles](/docs/0.6.1/guides/rbac).
- Learn about how to configure Stash operator as workload initializer [here](/docs/0.6.1/guides/initializer).
- Want to hack on Stash? Check our [contribution guidelines](/docs/0.6.1/CONTRIBUTING).