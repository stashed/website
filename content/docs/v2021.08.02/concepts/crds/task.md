---
title: Task Overview
menu:
  docs_v2021.08.02:
    identifier: task-overview
    name: Task
    parent: crds
    weight: 35
product_name: stash
menu_name: docs_v2021.08.02
section_menu_id: concepts
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

> New to Stash? Please start [here](/docs/v2021.08.02/concepts/README).

# Task

## What is Task

An entire backup or restore process needs an ordered execution of one or more steps. A [Function](/docs/v2021.08.02/concepts/crds/function) represents a step of a backup or restore process. A `Task` is a Kubernetes `CustomResourceDefinition`(CRD) which specifies a sequence of functions along with their parameters in a Kubernetes native way.

When you install Stash, some `Task`s will be pre-installed for supported targets like databases, etc. However, you can create your own `Task` to customize or extend the backup/restore process. Stash will execute these steps in the order you have specified.

## Task CRD Specification

Like any official Kubernetes resource, a `Task` has `TypeMeta`, `ObjectMeta` and `Spec` sections. However, unlike other Kubernetes resources, it does not have a `Status` section.

A sample `Task` object to backup a PostgreSQL database is shown below:

```yaml
apiVersion: stash.appscode.com/v1beta1
kind: Task
metadata:
  name: postgres-backup-11.2
spec:
  steps:
  - name: postgres-backup-11.2
    params:
    - name: outputDir # specifies where to write output file
      value: /tmp/output
    - name: secretVolume # specifies where backend secret has been mounted
      value: secret-volume
  - name: update-status
    params:
    - name: outputDir
      value: /tmp/output
    - name: secretVolume
      value: secret-volume
  volumes:
  - name: secret-volume
    secret:
      secretName: ${REPOSITORY_SECRET_NAME}
```

This `Task` uses two functions to backup a PostgreSQL database. The first step uses `postgres-backup-11.2` function that dumps PostgreSQL database and uploads the dumped file. The second step uses `update-status` function which updates the status of the `BackupSession` and `Repository` crd for respective backup.

Here, we are going to describe the various sections of a `Task` crd.

### Task `Spec`

A `Task` object has the following fields in the `spec` section:

#### spec.steps

`spec.steps` section specifies a list of functions and their parameters in the order they should be executed. You can also templatize this section using the [variables](/docs/v2021.08.02/concepts/crds/function#stash-provided-variables) that Stash can resolve itself. Stash will resolve all the variables and create a pod definition with a container specification for each `Function` specified in `steps` section.

Each `step` consists of the following fields:

- **name :** `name` specifies the name of the `Function` that will be executed at this step.
- **params :** `params` specifies an optional list of variables names and their values that Stash should use to resolve the respective `Function`. If you use a variable in a `Function` specification whose value Stash cannot provide, you can pass the value of that variable using this `params` section. You have to specify the following fields for a variable:
  - **name :** `name` of the variable.
  - **value :** value of the variable.

In the above example `Task`, we have used `outputDir` variable in `postgres-backup-11.2` function that Stash can't resolve automatically. So, we have passed the value using the `params` section in the `Task` object.

>Stash executes the `Functions` in the order they appear in `spec.steps` section. All the functions except the last one will be used to create `init-container` specification and the last function will be used to create `container` specification for respective backup job. This guarantees an ordered execution of the steps.

#### spec.volumes

`spec.volumes` specifies a list of volumes that should be mounted in the respective job created for this `Task`. In the sample we have shown above, we need to mount storage secret for the backup job. So, we have added the secret volume in `spec.volumes` section. Note that, we have used `REPOSITORY_SECRET_NAME` variable as secret name. This variable will be resolved by Stash from `Repository` specification.

## Why Function and Task?

You might be wondering why we have introduced `Function` and `Task` crd. We have designed `Function-Task` model for the following reasons:

- **Customizability:** `Function` and `Task` enables you to customize backup/recovery process. For example, currently we use [mysqldump](https://dev.mysql.com/doc/refman/8.0/en/mysqldump.html) in `mysql-backup` Function to backup MySQL database. You can build a custom `Function` using Percona's [xtrabackup](https://www.percona.com/software/mysql-database/percona-xtrabackup) tool instead of `mysqldump`. Then you can write a `Task` with this custom `Function` and use it to backup your target MySQL database.

  You can also customize backup/restore process by executing hooks before or after the backup/restore process. For example, if you want to execute some logic to prepare your apps for backup or you want to send an email notification after each backup, you just need to add `Function` with your custom logic and respective `Task` to execute them.

- **Extensibility:** Currently, Stash supports backup of MySQL, MongoDB and PostgreSQL databases. You can easily backup the databases that are not officially supported by Stash. You just need to create a `Function` and a `Task` for your desired database.

- **Re-usability:** `Function`'s are self-sufficient and independent of Stash. So, you can reuse them in any application that uses `Function-Task` model.

## Next Steps

- Learn how Stash backup databases using `Function-Task` model from [here](/docs/v2021.08.02/guides/latest/addons/overview).
- Learn how Stash backup stand-alone PVC using `Function-Task` model from [here](/docs/v2021.08.02/guides/latest/volumes/overview).
