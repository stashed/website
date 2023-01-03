---
title: Stash Overview
description: Stash Overview
menu:
  docs_v2023.01.05:
    identifier: overview-concepts
    name: Overview
    parent: what-is-stash
    weight: 10
product_name: stash
menu_name: docs_v2023.01.05
section_menu_id: concepts
info:
  cli: v0.25.0
  community: v0.25.0
  elasticsearch:
  - 5.6.4-v21
  - 6.2.4-v21
  - 6.3.0-v21
  - 6.4.0-v21
  - 6.5.3-v21
  - 6.8.0-v21
  - 7.14.0-v7
  - 7.2.0-v21
  - 7.3.2-v21
  - 8.2.0-v4
  enterprise: v0.25.0
  etcd:
  - 3.5.0-v8
  installer: v2023.01.05
  kubedump:
  - 0.1.0-v4
  mariadb:
  - 10.5.8-v14
  mongodb:
  - 3.4.17-v21
  - 3.4.22-v21
  - 3.6.13-v21
  - 3.6.8-v21
  - 4.0.11-v21
  - 4.0.3-v21
  - 4.0.5-v21
  - 4.1.13-v21
  - 4.1.4-v21
  - 4.1.7-v21
  - 4.2.3-v21
  - 4.4.6-v12
  - 5.0.3-v9
  mysql:
  - 5.7.25-v21
  - 8.0.14-v21
  - 8.0.21-v15
  - 8.0.3-v21
  nats:
  - 2.6.1-v9
  - 2.8.2-v4
  percona-xtradb:
  - 5.7-v16
  postgres:
  - 10.14-v20
  - 11.9-v20
  - 12.4-v20
  - 13.1-v17
  - 14.0-v9
  - 15.1-v1
  - 9.6.19-v20
  redis:
  - 5.0.13-v9
  - 6.2.5-v9
  - 7.0.5-v2
  ui-server: v0.7.0
  vault:
  - 1.10.3-v1
  version: v2023.01.05
---

# Stash

[Stash](https://stash.run) by AppsCode is a cloud native data backup and recovery solution for Kubernetes workloads. If you are running production workloads in Kubernetes, you might want to take backup of your disks, databases etc. Traditional tools are too complex to setup and maintain in a dynamic compute environment like Kubernetes. Stash is a Kubernetes operator that uses [restic](https://github.com/restic/restic) or Kubernetes CSI Driver VolumeSnapshotter functionality to address these issues. Using Stash, you can backup Kubernetes volumes mounted in workloads, stand-alone volumes and databases. User may even extend Stash via [addons](https://stash.run/docs/{{< param "info.version" >}}/guides/addons/overview/) for any custom workload.

## Features

| Features                                                                                | Community Edition | Enterprise Edition | Scope                                                                                                                                                               |
| --------------------------------------------------------------------------------------- | :---------------: | :----------------: | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|  | Open source Stash Free for everyone | Open Core Stash for production Enterprise workloads |  |
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
| Role Based Access Control (RBAC)                                                        |     &#10003;      |      &#10003;      | |
| Open Policy Agent (OPA)                                                                 |     &#10003;      |      &#10003;      | |
| Pod Security Policy (PSP)                                                               |     &#10003;      |      &#10003;      | |
| Network Policy                                                                          |     &#10003;      |      &#10003;      | |
| GDPR                                                                                    |     &#10007;      |      Planned       | |