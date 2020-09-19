---
title: Stash Overview
description: Stash Overview
menu:
  docs_v2020.09.16:
    identifier: overview-concepts
    name: Overview
    parent: what-is-stash
    weight: 10
product_name: stash
menu_name: docs_v2020.09.16
section_menu_id: concepts
info:
  catalog: v2020.09.16
  cli: v0.11.0
  community: v0.11.0
  elasticsearch:
  - 5.6.4-v2
  - 6.2.4-v2
  - 6.3.0-v2
  - 6.4.0-v2
  - 6.5.3-v2
  - 6.8.0-v2
  - 7.2.0-v2
  - 7.3.2-v2
  enterprise: v0.11.0
  mongodb:
  - 3.4.17-v2
  - 3.4.22-v2
  - 3.6.13-v2
  - 3.6.8-v2
  - 4.0.11-v2
  - 4.0.3-v2
  - 4.0.5-v2
  - 4.1.13-v2
  - 4.1.4-v2
  - 4.1.7-v2
  - 4.2.3-v2
  mysql:
  - 5.7.25-v2
  - 8.0.14-v2
  - 8.0.3-v2
  percona-xtradb:
  - 5.7-v2
  postgres:
  - 10.14.0
  - 11.9.0
  - 12.4.0
  - 9.6.19
  version: v2020.09.16
---

# Stash

[Stash](https://stash.run) by AppsCode is a cloud native data backup and recovery solution for Kubernetes workloads. If you are running production workloads in Kubernetes, you might want to take backup of your disks, databases etc. Traditional tools are too complex to setup and maintain in a dynamic compute environment like Kubernetes. Stash is a Kubernetes operator that uses [restic](https://github.com/restic/restic) or Kubernetes CSI Driver VolumeSnapshotter functionality to address these issues. Using Stash, you can backup Kubernetes volumes mounted in workloads, stand-alone volumes and databases. User may even extend Stash via [addons](https://stash.run/docs/latest/guides/latest/addons/overview/) for any custom workload.

## Features

| Features                                                                                | Community Edition | Enterprise Edition | Scope                                                                                                                                                               |
| --------------------------------------------------------------------------------------- | :---------------: | :----------------: | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Backup & Restore Workload Data                                                          |     &#10003;      |      &#10003;      | Deployment, DaemonSet, StatefulSet, ReplicaSet, ReplicationController, OpenShift DeploymentConfig                                                                   |
| Backup & Restore Stand-alone Volume (PVC)                                               |     &#10003;      |      &#10003;      | PersistentVolumeClaim, PersistentVolume                                                                                                                             |
| Schedule Backup, Instant Backup                                                         |     &#10003;      |      &#10003;      | Schedule through [cron expression](https://en.wikipedia.org/wiki/Cron) or trigger instant backup using Stash Kubernetes plugin                                      |
| Pause Backup                                                                            |     &#10003;      |      &#10003;      | No new backup when paused.                                                                                                                                          |
| Backup & Restore subset of files                                                        |     &#10003;      |      &#10003;      | Only backup/restore the files that matches the provided patterns                                                                                                    |
| Cleanup old snapshots automatically                                                     |     &#10003;      |      &#10003;      | Cleanup old snapshots according to different [retention policies](https://restic.readthedocs.io/en/stable/060_forget.html#removing-snapshots-according-to-a-policy) |
| Encryption, Deduplication (send only diff)                                              |     &#10003;      |      &#10003;      | Encrypt backed up data with AES-256. Stash only sends the changes since last backup.                                                                                |
| [CSI Driver Integration](https://kubernetes.io/docs/concepts/storage/volume-snapshots/) |     &#10003;      |      &#10003;      | VolumeSnapshot for Kubernetes workloads. Supported for Kubernetes v1.17.0+.                                                                                         |
| Prometheus Metrics                                                                      |     &#10003;      |      &#10003;      | Rich backup metrics, restore metrics and Stash operator metrics.                                                                                                    |
| Security                                                                                |     &#10003;      |      &#10003;      | Built-in support for RBAC, PSP and Network Policy                                                                                                                   |
| CLI                                                                                     |     &#10003;      |      &#10003;      | `kubectl` plugin (for Kubernetes 1.12+)                                                                                                                             |
| Extensibility and Customizability                                                       |     &#10003;      |      &#10003;      | Write addons for bespoke applications and customize currently supported workloads                                                                                   |
| Hooks                                                                                   |     &#10003;      |      &#10003;      | Execute `httpGet`, `httpPost`, `tcpSocket` and `exec` hooks before and after of backup or restore process.                                                          |
| Cloud Storage as Backend                                                                |     &#10003;      |      &#10003;      | Stores backup data in AWS S3, Minio, Rook, GCS, Azure, OpenStack Swift, Backblaze B2 and Rest Server                                                                |
| On-prem Storage as Backend                                                              |     &#10007;      |      &#10003;      | Stores backup data in any locally mounted Kubernetes Volumes such as NFS, etc.                                                                                      |
| Backup & Restore databases                                                              |     &#10007;      |      &#10003;      | PostgreSQL, MySQL, MongoDB, Elasticsearch, Redis, MariaDB, Percona XtraDB                                                                                           |
| Auto Backup                                                                             |     &#10007;      |      &#10003;      | Share backup configuration across workloads using templates. Enable backup for a target application via annotation.                                                 |
| Batch Backup & Batch Restore                                                            |     &#10007;      |      &#10003;      | Backup and restore co-related applications (eg, WordPress server and its database) together                                                                         |
| Point-In-Time Recovery (PITR)                                                           |     &#10007;      |      Planned       | Restore a set of files from a time in the past.                                                                                                                     |
