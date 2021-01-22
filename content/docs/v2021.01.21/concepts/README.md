---
title: Concepts | Stash
menu:
  docs_v2021.01.21:
    identifier: concepts-readme
    name: README
    parent: concepts
    weight: -1
product_name: stash
menu_name: docs_v2021.01.21
section_menu_id: concepts
url: /docs/v2021.01.21/concepts/
aliases:
- /docs/v2021.01.21/concepts/README/
info:
  catalog: v2021.01.21
  cli: v0.11.9
  community: v0.11.9
  elasticsearch:
  - 5.6.4-v6
  - 6.2.4-v6
  - 6.3.0-v6
  - 6.4.0-v6
  - 6.5.3-v6
  - 6.8.0-v6
  - 7.2.0-v6
  - 7.3.2-v6
  enterprise: v0.11.9
  installer: v0.11.9
  mariadb:
  - 10.5.8
  mongodb:
  - 3.4.17-v5
  - 3.4.22-v5
  - 3.6.13-v5
  - 3.6.8-v5
  - 4.0.11-v5
  - 4.0.3-v5
  - 4.0.5-v5
  - 4.1.13-v5
  - 4.1.4-v5
  - 4.1.7-v5
  - 4.2.3-v5
  mysql:
  - 5.7.25-v6
  - 8.0.14-v6
  - 8.0.21
  - 8.0.3-v6
  percona-xtradb:
  - 5.7.0-v1
  postgres:
  - 10.14.0-v4
  - 11.9.0-v4
  - 12.4.0-v4
  - 13.1.0-v1
  - 9.6.19-v4
  version: v2021.01.21
---

# Concepts

Concepts help you to learn about the different parts of the Stash and the abstractions it uses.

This concept section is divided into the following modules:

- What is Stash?
  - [Overview](/docs/v2021.01.21/concepts/what-is-stash/overview) provides an introduction to Stash. It also give an overview of the features it provides.
  - [Architecture](/docs/v2021.01.21/concepts/what-is-stash/architecture) provides a visual representation of Stash architecture. It also provides a brief overview of the components it uses.

- Declarative API
  - [Repository](/docs/v2021.01.21/concepts/crds/repository) introduces the concept of `Repository` crd that holds backend information in a Kubernetes native way.
  - [BackupConfiguration](/docs/v2021.01.21/concepts/crds/backupconfiguration) introduces the concept of `BackupConfiguration` crd that is used to configure backup for a target resource in a Kubernetes native way.
  - [BackupBatch](/docs/v2021.01.21/concepts/crds/backupbatch) introduces the concept of `BackupBatch` crd that is used to setup backup of multiple co-related targets under single configuration.
  - [BackupSession](/docs/v2021.01.21/concepts/crds/backupsession) introduces the concept of `BackupSession` crd that represents a backup run of a target resource for the respective `BackupConfiguration` or `BackupBatch` object.
  - [RestoreSession](/docs/v2021.01.21/concepts/crds/restoresession) introduces the concept of `RestoreSession` crd that represents a restore run of a target resource.
  - [RestoreBatch](/docs/v2021.01.21/concepts/crds/restorebatch) introduces the concept of `RestoreBatch` crd that allows restore of multiple targets that were backed up using `BackupBatch` under single configuration.
  - [Function](/docs/v2021.01.21/concepts/crds/function) introduces the concept of `Function` crd that represents a step of a backup or restore process.
  - [Task](/docs/v2021.01.21/concepts/crds/task) introduces the concept of `Task` crd which specifies an ordered collection of multiple `Function`s and their parameters that make up a complete backup or restore process.
  - [BackupBlueprint](/docs/v2021.01.21/concepts/crds/backupblueprint) introduces the concept of `BackupBlueprint` crd that specifies a blueprint for `Repository` and `BackupConfiguration` object which provides an option to share backup configuration across similar targets.
  - [AppBinding](/docs/v2021.01.21/concepts/crds/appbinding) introduces the concept of `AppBinding` crd which holds the information that are necessary to connect with an application like database.
  - [Snapshot](/docs/v2021.01.21/concepts/crds/snapshot) introduces the concept of `Snapshot` object that represents backed up snapshots in a Kubernetes native way.

- v1alpha1 API
  - [Restic](/docs/v2021.01.21/concepts/crds/v1alpha1/restic) introduces the concept of `Restic` crd that is used for configuring [restic](https://restic.net) in a Kubernetes native way.
  - [Recovery](/docs/v2021.01.21/concepts/crds/v1alpha1/recovery) introduces the concept of `Recovery` crd that is used to restore a backup taken using Stash.