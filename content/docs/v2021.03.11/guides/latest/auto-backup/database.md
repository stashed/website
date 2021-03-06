---
title: Auto Backup Databases | Stash
description: Stash auto-backup for databases.
menu:
  docs_v2021.03.11:
    identifier: auto-backup-database
    name: Auto Backup for Databases
    parent: auto-backup
    weight: 40
product_name: stash
menu_name: docs_v2021.03.11
section_menu_id: guides
info:
  catalog: v2021.03.11
  cli: v0.11.11
  community: v0.11.11
  elasticsearch:
  - 5.6.4-v7
  - 6.2.4-v7
  - 6.3.0-v7
  - 6.4.0-v7
  - 6.5.3-v7
  - 6.8.0-v7
  - 7.2.0-v7
  - 7.3.2-v7
  enterprise: v0.11.11
  installer: v0.11.11
  mariadb:
  - 10.5.8-v1
  mongodb:
  - 3.4.17-v6
  - 3.4.22-v6
  - 3.6.13-v6
  - 3.6.8-v6
  - 4.0.11-v6
  - 4.0.3-v6
  - 4.0.5-v6
  - 4.1.13-v6
  - 4.1.4-v6
  - 4.1.7-v6
  - 4.2.3-v6
  mysql:
  - 5.7.25-v7
  - 8.0.14-v7
  - 8.0.21-v1
  - 8.0.3-v7
  percona-xtradb:
  - 5.7.0-v2
  postgres:
  - 10.14.0-v5
  - 11.9.0-v5
  - 12.4.0-v5
  - 13.1.0-v2
  - 9.6.19-v5
  version: v2021.03.11
---

{{< notice type="warning" message="This is an Enterprise-only feature. Please install [Stash Enterprise Edition](/docs/v2021.03.11/setup/install/enterprise) to try this feature." >}}

# Auto Backup for Database

This tutorial will give you an overview of how you can configure Stash auto-backup of the databases and the available configurable options for database auto-backup.

## Configuring auto-backup for databases

To configure auto-backup for a database, you have to follow the following steps:

- **Create BackupBlueprint:** At first, you have to create a `BackupBlueprint` with the template for `Repository` and `BackupConfiguration`. Use the appropriate `Task` in the `task` section.
- **Create Storage Secret:** Then, you have to create a storage Secret in the same namespace as your database with the access credential to your backend. You can re-use this secret to backup the other databases in that namespace.
- **Add auto-backup annotations:** Finally, add the auto-backup annotations to your database object or the respective `AppBinding` object.

## Where to put auto-backup annotations

If you are using KubeDB to manage your databases, you can add the annotations to your database object. KubeDB will automatically pass those annotations to the respective `AppBinding`.

If you are not managing your database using KubeDB, you have to add the annotation in the respective `AppBinding` that you have created for your database.

## Available Auto-Backup Annotations for Database

The following auto-backup annotations are available for the databases:

- **BackupBlueprint Name:** You have to specify the `BackupBlueprint` name that holds the template for `Repository` and `BackupConfiguration` in the following annotation:

```yaml
stash.appscode.com/backup-blueprint: <BackupBlueprint name>
```

- **Schedule:** You can specify a custom schedule for a target to overwrite the schedule of the BackupBlueprint through this annotation.

```yaml
 stash.appscode.com/schedule: <Cron Expression>
```

- **Task Parameters:** You can also pass some parameters to the respective backup `Task` through annotations. Use following format to pass parameters via annotations:

```yaml
params.stash.appscode.com/key1: value1
params.stash.appscode.com/key2: value2,value3
params.stash.appscode.com/key3: ab=123,bc=234
```

The above parameters will be added in the `spec.task.params` section as bellow,

```yaml
task:
  name: postgres-backup-13.1-v1
  params:
  - name: key1
    value: value1
  - name: key2
    value: value3,value3
  - name: key3
    value: ab=123,bc=234
```

## Database Auto-backup Examples

You can find auto-backup examples for the databases here:

- [Backup PostgreSQL using Stash Auto-Backup](/docs/v2021.03.11/addons/postgres/guides/auto-backup/)
- [Backup Elasticsearch using Stash Auto-Backup](/docs/v2021.03.11/addons/elasticsearch/guides/auto-backup/)
