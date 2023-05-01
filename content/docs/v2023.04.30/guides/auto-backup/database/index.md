---
title: Auto Backup Databases | Stash
description: Stash auto-backup for databases.
menu:
  docs_v2023.04.30:
    identifier: auto-backup-database
    name: Auto Backup for Databases
    parent: auto-backup
    weight: 40
product_name: stash
menu_name: docs_v2023.04.30
section_menu_id: guides
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

You can also specify multiple BackupBlueprint name separated by comma (`,`). For example:

```yaml
stash.appscode.com/backup-blueprint: daily-gcs-backup,weekly-s3-backup
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

- [Backup PostgreSQL using Stash Auto-Backup](/docs/v2023.04.30/addons/postgres/auto-backup/)
- [Backup Elasticsearch using Stash Auto-Backup](/docs/v2023.04.30/addons/elasticsearch/auto-backup/)
