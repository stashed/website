---
title: Concepts | Stash
menu:
  docs_v2021.08.02:
    identifier: concepts-readme
    name: README
    parent: concepts
    weight: -1
product_name: stash
menu_name: docs_v2021.08.02
section_menu_id: concepts
url: /docs/v2021.08.02/concepts/
aliases:
- /docs/v2021.08.02/concepts/README/
info:
  cli: v0.15.0
  community: v0.15.0
  elasticsearch:
  - 5.6.4-v12
  - 6.2.4-v12
  - 6.3.0-v12
  - 6.4.0-v12
  - 6.5.3-v12
  - 6.8.0-v12
  - 7.2.0-v12
  - 7.3.2-v12
  enterprise: v0.15.0
  installer: v2021.08.02
  mariadb:
  - 10.5.8-v5
  mongodb:
  - 3.4.17-v11
  - 3.4.22-v11
  - 3.6.13-v11
  - 3.6.8-v11
  - 4.0.11-v11
  - 4.0.3-v11
  - 4.0.5-v11
  - 4.1.13-v11
  - 4.1.4-v11
  - 4.1.7-v11
  - 4.2.3-v11
  - 4.4.6-v2
  mysql:
  - 5.7.25-v12
  - 8.0.14-v12
  - 8.0.21-v6
  - 8.0.3-v12
  percona-xtradb:
  - 5.7-v7
  postgres:
  - 10.14-v10
  - 11.9-v10
  - 12.4-v10
  - 13.1-v7
  - 9.6.19-v10
  redis:
  - 5.0.13
  - 6.2.5
  version: v2021.08.02
---

# Concepts

Concepts help you to learn about the different parts of the Stash and the abstractions it uses.

This concept section is divided into the following modules:

- What is Stash?
  - [Overview](/docs/v2021.08.02/concepts/what-is-stash/overview) provides an introduction to Stash. It also give an overview of the features it provides.
  - [Architecture](/docs/v2021.08.02/concepts/what-is-stash/architecture) provides a visual representation of Stash architecture. It also provides a brief overview of the components it uses.

- Declarative API
  - [Repository](/docs/v2021.08.02/concepts/crds/repository) introduces the concept of `Repository` crd that holds backend information in a Kubernetes native way.
  - [BackupConfiguration](/docs/v2021.08.02/concepts/crds/backupconfiguration) introduces the concept of `BackupConfiguration` crd that is used to configure backup for a target resource in a Kubernetes native way.
  - [BackupBatch](/docs/v2021.08.02/concepts/crds/backupbatch) introduces the concept of `BackupBatch` crd that is used to setup backup of multiple co-related targets under single configuration.
  - [BackupSession](/docs/v2021.08.02/concepts/crds/backupsession) introduces the concept of `BackupSession` crd that represents a backup run of a target resource for the respective `BackupConfiguration` or `BackupBatch` object.
  - [RestoreSession](/docs/v2021.08.02/concepts/crds/restoresession) introduces the concept of `RestoreSession` crd that represents a restore run of a target resource.
  - [RestoreBatch](/docs/v2021.08.02/concepts/crds/restorebatch) introduces the concept of `RestoreBatch` crd that allows restore of multiple targets that were backed up using `BackupBatch` under single configuration.
  - [Function](/docs/v2021.08.02/concepts/crds/function) introduces the concept of `Function` crd that represents a step of a backup or restore process.
  - [Task](/docs/v2021.08.02/concepts/crds/task) introduces the concept of `Task` crd which specifies an ordered collection of multiple `Function`s and their parameters that make up a complete backup or restore process.
  - [BackupBlueprint](/docs/v2021.08.02/concepts/crds/backupblueprint) introduces the concept of `BackupBlueprint` crd that specifies a blueprint for `Repository` and `BackupConfiguration` object which provides an option to share backup configuration across similar targets.
  - [AppBinding](/docs/v2021.08.02/concepts/crds/appbinding) introduces the concept of `AppBinding` crd which holds the information that are necessary to connect with an application like database.
  - [Snapshot](/docs/v2021.08.02/concepts/crds/snapshot) introduces the concept of `Snapshot` object that represents backed up snapshots in a Kubernetes native way.

- v1alpha1 API
  - [Restic](/docs/v2021.08.02/concepts/crds/v1alpha1/restic) introduces the concept of `Restic` crd that is used for configuring [restic](https://restic.net) in a Kubernetes native way.
  - [Recovery](/docs/v2021.08.02/concepts/crds/v1alpha1/recovery) introduces the concept of `Recovery` crd that is used to restore a backup taken using Stash.
