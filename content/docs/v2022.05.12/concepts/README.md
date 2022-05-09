---
title: Concepts | Stash
menu:
  docs_v2022.05.12:
    identifier: concepts-readme
    name: README
    parent: concepts
    weight: -1
product_name: stash
menu_name: docs_v2022.05.12
section_menu_id: concepts
url: /docs/v2022.05.12/concepts/
aliases:
- /docs/v2022.05.12/concepts/README/
info:
  cli: v0.20.0
  community: v0.20.0
  elasticsearch:
  - 5.6.4-v17
  - 6.2.4-v17
  - 6.3.0-v17
  - 6.4.0-v17
  - 6.5.3-v17
  - 6.8.0-v17
  - 7.14.0-v3
  - 7.2.0-v17
  - 7.3.2-v17
  enterprise: v0.20.0
  etcd:
  - 3.5.0-v4
  installer: v2022.05.12
  kubedump:
  - 0.1.0
  mariadb:
  - 10.5.8-v10
  mongodb:
  - 3.4.17-v16
  - 3.4.22-v16
  - 3.6.13-v16
  - 3.6.8-v16
  - 4.0.11-v16
  - 4.0.3-v16
  - 4.0.5-v16
  - 4.1.13-v16
  - 4.1.4-v16
  - 4.1.7-v16
  - 4.2.3-v16
  - 4.4.6-v7
  - 5.0.3-v4
  mysql:
  - 5.7.25-v17
  - 8.0.14-v17
  - 8.0.21-v11
  - 8.0.3-v17
  nats:
  - 2.6.1-v4
  percona-xtradb:
  - 5.7-v12
  postgres:
  - 10.14-v15
  - 11.9-v15
  - 12.4-v15
  - 13.1-v12
  - 14.0-v4
  - 9.6.19-v15
  redis:
  - 5.0.13-v5
  - 6.2.5-v5
  ui-server: v0.2.0
  version: v2022.05.12
---

# Concepts

Concepts help you to learn about the different parts of the Stash and the abstractions it uses.

This concept section is divided into the following modules:

- What is Stash?
  - [Overview](/docs/v2022.05.12/concepts/what-is-stash/overview) provides an introduction to Stash. It also give an overview of the features it provides.
  - [Architecture](/docs/v2022.05.12/concepts/what-is-stash/architecture) provides a visual representation of Stash architecture. It also provides a brief overview of the components it uses.

- Declarative API
  - [Repository](/docs/v2022.05.12/concepts/crds/repository) introduces the concept of `Repository` crd that holds backend information in a Kubernetes native way.
  - [BackupConfiguration](/docs/v2022.05.12/concepts/crds/backupconfiguration) introduces the concept of `BackupConfiguration` crd that is used to configure backup for a target resource in a Kubernetes native way.
  - [BackupBatch](/docs/v2022.05.12/concepts/crds/backupbatch) introduces the concept of `BackupBatch` crd that is used to setup backup of multiple co-related targets under single configuration.
  - [BackupSession](/docs/v2022.05.12/concepts/crds/backupsession) introduces the concept of `BackupSession` crd that represents a backup run of a target resource for the respective `BackupConfiguration` or `BackupBatch` object.
  - [RestoreSession](/docs/v2022.05.12/concepts/crds/restoresession) introduces the concept of `RestoreSession` crd that represents a restore run of a target resource.
  - [RestoreBatch](/docs/v2022.05.12/concepts/crds/restorebatch) introduces the concept of `RestoreBatch` crd that allows restore of multiple targets that were backed up using `BackupBatch` under single configuration.
  - [Function](/docs/v2022.05.12/concepts/crds/function) introduces the concept of `Function` crd that represents a step of a backup or restore process.
  - [Task](/docs/v2022.05.12/concepts/crds/task) introduces the concept of `Task` crd which specifies an ordered collection of multiple `Function`s and their parameters that make up a complete backup or restore process.
  - [BackupBlueprint](/docs/v2022.05.12/concepts/crds/backupblueprint) introduces the concept of `BackupBlueprint` crd that specifies a blueprint for `Repository` and `BackupConfiguration` object which provides an option to share backup configuration across similar targets.
  - [AppBinding](/docs/v2022.05.12/concepts/crds/appbinding) introduces the concept of `AppBinding` crd which holds the information that are necessary to connect with an application like database.
  - [Snapshot](/docs/v2022.05.12/concepts/crds/snapshot) introduces the concept of `Snapshot` object that represents backed up snapshots in a Kubernetes native way.
