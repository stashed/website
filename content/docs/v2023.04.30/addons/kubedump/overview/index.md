---
title: KubeDump Backup Overview | Stash
description: How KubeDump Backup Works in Stash
menu:
  docs_v2023.04.30:
    identifier: stash-kubedump-overview
    name: How does it work?
    parent: stash-kubedump
    weight: 10
product_name: stash
menu_name: docs_v2023.04.30
section_menu_id: stash-addons
info:
  cli: v0.29.0
  community: v0.29.0
  elasticsearch:
  - 5.6.4-v25
  - 6.2.4-v25
  - 6.3.0-v25
  - 6.4.0-v25
  - 6.5.3-v25
  - 6.8.0-v25
  - 7.14.0-v11
  - 7.2.0-v25
  - 7.3.2-v25
  - 8.2.0-v8
  enterprise: v0.29.0
  etcd:
  - 3.5.0-v12
  installer: v2023.04.30
  kubedump:
  - 0.1.0-v8
  mariadb:
  - 10.5.8-v18
  mongodb:
  - 3.4.17-v25
  - 3.4.22-v25
  - 3.6.13-v25
  - 3.6.8-v25
  - 4.0.11-v25
  - 4.0.3-v25
  - 4.0.5-v25
  - 4.1.13-v25
  - 4.1.4-v25
  - 4.1.7-v25
  - 4.2.3-v25
  - 4.4.6-v16
  - 5.0.3-v13
  - 6.0.5-v1
  mysql:
  - 5.7.25-v25
  - 8.0.14-v25
  - 8.0.21-v19
  - 8.0.3-v25
  nats:
  - 2.6.1-v13
  - 2.8.2-v8
  percona-xtradb:
  - 5.7-v20
  postgres:
  - 10.14-v24
  - 11.9-v24
  - 12.4-v24
  - 13.1-v21
  - 14.0-v13
  - 15.1-v5
  - 9.6.19-v24
  redis:
  - 5.0.13-v13
  - 6.2.5-v13
  - 7.0.5-v6
  ui-server: v0.11.0
  vault:
  - 1.10.3-v5
  version: v2023.04.30
---

{{< notice type="warning" message="This is an Enterprise-only feature. Please install [Stash Enterprise Edition](/docs/v2023.04.30/setup/install/enterprise/) to try this feature." >}}

# How Stash Backups Kubernetes Resources

Stash `{{< param "info.version" >}}` supports taking backup of Kubernetes resource YAMLs. You can backup the YAML definition of the resources of entire cluster, a particular namespaces, or only an application etc. In this guide, we are going to show you how Kubernetes resource backup works in Stash.

## How Backup Works

The following diagram shows how Stash takes backup of the Kubernetes resources. Open the image in a new tab to see the enlarged version.

<figure align="center">
  <img alt="KubeDump Backup Overview" src="/docs/v2023.04.30/addons/kubedump/overview/images/kubedump-backup.svg">
  <figcaption align="center">Fig: KubeDump Backup Overview</figcaption>
</figure>

The backup process consists of the following steps:

1. At first, a user creates a secret with access credentials of the backend where the backed up data will be stored.

2. Then, she creates a `Repository` crd that specifies the backend information along with the secret that holds the credentials to access the backend.

3. Then, she creates a `BackupConfiguration` crd which specifies the `Task` to use to backup the the resources.

4. Stash operator watches for `BackupConfiguration` crd.

5. Once Stash operator finds a `BackupConfiguration` crd, it creates a CronJob with the schedule specified in `BackupConfiguration` object to trigger backup periodically.

6. On the next scheduled slot, the CronJob triggers a backup by creating a `BackupSession` crd.

7. Stash operator also watches for `BackupSession` crd.

8. When it finds a `BackupSession` object, it resolves the respective `Task` and `Function` and prepares a Job definition to backup.

9. Then, it creates the Job to backup the desired resource YAMLs.

10. Then, the Job dumps the Kubernetes Resource YAML, and store them temporarily in a directory.Then, it upload the content of the directory to the cloud backend.

11. Finally, when the backup is complete, the Job sends Prometheus metrics to the Pushgateway running inside Stash operator pod. It also updates the `BackupSession` and `Repository` status to reflect the backup procedure.

## Next Steps

- Backup the YAMLs of your entire Kubernetes cluster using Stash following the guide from [here](/docs/v2023.04.30/addons/kubedump/cluster/).
- Backup the YAMLs of an entire Namespace using Stash following the guide from [here](/docs/v2023.04.30/addons/kubedump/namespace/).
- Backup the YAMLs of a particular application using Stash following the guide from [here](/docs/v2023.04.30/addons/kubedump/application/).
