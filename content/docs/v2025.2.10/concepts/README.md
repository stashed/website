---
title: Concepts | Stash
menu:
  docs_v2025.2.10:
    identifier: concepts-readme
    name: README
    parent: concepts
    weight: -1
product_name: stash
menu_name: docs_v2025.2.10
section_menu_id: concepts
url: /docs/v2025.2.10/concepts/
aliases:
- /docs/v2025.2.10/concepts/README/
info:
  cli: v0.39.0
  community: v0.39.0
  elasticsearch:
  - 5.6.4-v35
  - 6.2.4-v35
  - 6.3.0-v35
  - 6.4.0-v35
  - 6.5.3-v35
  - 6.8.0-v35
  - 7.14.0-v21
  - 7.2.0-v35
  - 7.3.2-v35
  - 8.2.0-v18
  enterprise: v0.39.0
  etcd:
  - 3.5.0-v22
  installer: v2025.2.10
  kubedump:
  - 0.2.0-v3
  mariadb:
  - 10.5.8-v29
  mongodb:
  - 3.4.17-v36
  - 3.4.22-v36
  - 3.6.13-v36
  - 3.6.8-v36
  - 4.0.11-v36
  - 4.0.3-v36
  - 4.0.5-v36
  - 4.1.13-v36
  - 4.1.4-v36
  - 4.1.7-v36
  - 4.2.3-v36
  - 4.4.6-v27
  - 5.0.15-v9
  - 5.0.3-v24
  - 6.0.5-v12
  mysql:
  - 5.7.25-v36
  - 8.0.14-v35
  - 8.0.21-v29
  - 8.0.3-v35
  nats:
  - 2.6.1-v23
  - 2.8.2-v18
  percona-xtradb:
  - 5.7-v30
  postgres:
  - 10.14-v34
  - 11.9-v34
  - 12.4-v34
  - 13.1-v31
  - 14.0-v23
  - 15.1-v15
  - 16.1-v4
  - 17.2-v2
  - 9.6.19-v34
  redis:
  - 5.0.13-v23
  - 6.2.5-v23
  - 7.0.5-v16
  ui-server: v0.20.0
  vault:
  - 1.10.3-v15
  version: v2025.2.10
---

# Concepts

Concepts help you to learn about the different parts of the Stash and the abstractions it uses.

This concept section is divided into the following modules:

- What is Stash?
  - [Overview](/docs/v2025.2.10/concepts/what-is-stash/overview/) provides an introduction to Stash. It also give an overview of the features it provides.
  - [Architecture](/docs/v2025.2.10/concepts/what-is-stash/architecture/) provides a visual representation of Stash architecture. It also provides a brief overview of the components it uses.

- Declarative API
  - [Repository](/docs/v2025.2.10/concepts/crds/repository/) introduces the concept of `Repository` crd that holds backend information in a Kubernetes native way.
  - [BackupConfiguration](/docs/v2025.2.10/concepts/crds/backupconfiguration/) introduces the concept of `BackupConfiguration` crd that is used to configure backup for a target resource in a Kubernetes native way.
  - [BackupBatch](/docs/v2025.2.10/concepts/crds/backupbatch/) introduces the concept of `BackupBatch` crd that is used to setup backup of multiple co-related targets under single configuration.
  - [BackupSession](/docs/v2025.2.10/concepts/crds/backupsession/) introduces the concept of `BackupSession` crd that represents a backup run of a target resource for the respective `BackupConfiguration` or `BackupBatch` object.
  - [RestoreSession](/docs/v2025.2.10/concepts/crds/restoresession/) introduces the concept of `RestoreSession` crd that represents a restore run of a target resource.
  - [RestoreBatch](/docs/v2025.2.10/concepts/crds/restorebatch/) introduces the concept of `RestoreBatch` crd that allows restore of multiple targets that were backed up using `BackupBatch` under single configuration.
  - [Function](/docs/v2025.2.10/concepts/crds/function/) introduces the concept of `Function` crd that represents a step of a backup or restore process.
  - [Task](/docs/v2025.2.10/concepts/crds/task/) introduces the concept of `Task` crd which specifies an ordered collection of multiple `Function`s and their parameters that make up a complete backup or restore process.
  - [BackupBlueprint](/docs/v2025.2.10/concepts/crds/backupblueprint/) introduces the concept of `BackupBlueprint` crd that specifies a blueprint for `Repository` and `BackupConfiguration` object which provides an option to share backup configuration across similar targets.
  - [AppBinding](/docs/v2025.2.10/concepts/crds/appbinding/) introduces the concept of `AppBinding` crd which holds the information that are necessary to connect with an application like database.
  - [Snapshot](/docs/v2025.2.10/concepts/crds/snapshot/) introduces the concept of `Snapshot` object that represents backed up snapshots in a Kubernetes native way.
