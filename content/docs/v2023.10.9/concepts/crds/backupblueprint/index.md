---
title: BackupBlueprint Overview
menu:
  docs_v2023.10.9:
    identifier: backupblueprint-overview
    name: BackupBlueprint
    parent: crds
    weight: 40
product_name: stash
menu_name: docs_v2023.10.9
section_menu_id: concepts
info:
  cli: v0.32.0
  community: v0.32.0
  elasticsearch:
  - 5.6.4-v28
  - 6.2.4-v28
  - 6.3.0-v28
  - 6.4.0-v28
  - 6.5.3-v28
  - 6.8.0-v28
  - 7.14.0-v14
  - 7.2.0-v28
  - 7.3.2-v28
  - 8.2.0-v11
  enterprise: v0.32.0
  etcd:
  - 3.5.0-v15
  installer: v2023.10.9
  kubedump:
  - 0.1.0-v11
  mariadb:
  - 10.5.8-v21
  mongodb:
  - 3.4.17-v28
  - 3.4.22-v28
  - 3.6.13-v28
  - 3.6.8-v28
  - 4.0.11-v28
  - 4.0.3-v28
  - 4.0.5-v28
  - 4.1.13-v28
  - 4.1.4-v28
  - 4.1.7-v28
  - 4.2.3-v28
  - 4.4.6-v19
  - 5.0.15-v1
  - 5.0.3-v16
  - 6.0.5-v4
  mysql:
  - 5.7.25-v28
  - 8.0.14-v28
  - 8.0.21-v22
  - 8.0.3-v28
  nats:
  - 2.6.1-v16
  - 2.8.2-v11
  percona-xtradb:
  - 5.7-v23
  postgres:
  - 10.14-v27
  - 11.9-v27
  - 12.4-v27
  - 13.1-v24
  - 14.0-v16
  - 15.1-v8
  - 9.6.19-v27
  redis:
  - 5.0.13-v16
  - 6.2.5-v16
  - 7.0.5-v9
  ui-server: v0.13.0
  vault:
  - 1.10.3-v8
  version: v2023.10.9
---

> New to Stash? Please start [here](/docs/v2023.10.9/concepts/README).

# BackupBlueprint

## What is BackupBlueprint

Stash uses 1-1 mapping among `Repository`, `BackupConfiguration` and the target. So, whenever you want to backup a target(workload/PV/PVC/database), you have to create a `Repository` and `BackupConfiguration` object. This could become tiresome when you are trying to backup similar types of target and the `Repository` and `BackupConfiguration` has only slight difference. To mitigate this problem, Stash provides a way to specify a blueprint for these two objects via `BackupBlueprint` crd.

A `BackupBlueprint` is a Kubernetes `CustomResourceDefinition`(CRD) which specifies a blueprint for `Repository` and `BackupConfiguration` in a Kubernetes native way.

You have to create only one  `BackupBlueprint` for all similar types of workloads (i.e. Deployment, DaemonSet, StatefulSet etc.). Then, you just need to add some annotations in the target workload. Stash will automatically create respective `Repository`, `BackupConfiguration` object using the blueprint. In Stash parlance, we call this process as **auto backup**.

## BackupBlueprint CRD Specification

Like any official Kubernetes resource, a `BackupBlueprint` has `TypeMeta`, `ObjectMeta` and `Spec` sections. However, unlike other Kubernetes resources, it does not have a `Status` section.

A sample `BackupBlueprint` object to auto backup the volumes of a Deployment is shown below,

```yaml
apiVersion: stash.appscode.com/v1beta1
kind: BackupBlueprint
metadata:
  name: workload-backup-blueprint
spec:
  # ============== Blueprint for Repository ==========================
  backend:
    gcs:
      bucket: stash-backup
      prefix: stash/${TARGET_NAMESPACE}/${TARGET_KIND}/${TARGET_NAME}
    storageSecretName: gcs-secret
  wipeOut: false
  # backupNamespace: stash
  # ============== Blueprint for BackupConfiguration =================
  schedule: "* * * * *"
  backupHistoryLimit: 3
  timeOut: 30m
  retryConfig:
    maxRetry: 3
    delay: 10m
  # task: # no task section is required for workload data backup
  #   name: workload-backup
  runtimeSettings:
    container:
      securityContext:
        runAsUser: 2000
        runAsGroup: 2000
  tempDir:
    medium: "Memory"
    sizeLimit: "1Gi"
    disableCaching: false
  retentionPolicy:
    name: 'keep-last-5'
    keepLast: 5
    prune: true
```

The sample `BackupBlueprint` that has been shown above can be used to backup Deployments, DaemonSets, StatefulSets, ReplicaSets and ReplicationControllers. You only have to add some annotations to these workloads. For more details on how auto backup works in Stash, please visit [here](/docs/v2023.10.9/guides/auto-backup/overview/).

Here, we are going to describe the various sections of `BackupBlueprint` crd.

### BackupBlueprint `Spec`

We can divide BackupBlueprint's `.spec` section into two parts. One part specifies a blueprint for `Repository` object and other specifies a blueprint for `BackupConfiguration` object.

#### Repository Blueprint

You can configure `Repository` blueprint using `spec.backend` field and `spec.wipeOut` field.

- **spec.backend :** `spec.backend` field is backend specification similar to [spec.backend](/docs/v2023.10.9/concepts/crds/repository/#specbackend) field of a `Repository` crd. There is only one difference. You can now templatize `prefix` section (`subPath` for local volume) of the backend to store backed up data of different workloads at different directory. You can use the following variables to templatize `spec.backend` field:

|       Variable        |                                                              Usage                                                              |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| `TARGET_API_VERSION`  | API version of the target                                                                                                       |
| `TARGET_KIND`         | Resource kind of the target                                                                                                     |
| `TARGET_NAMESPACE`    | Namespace of the target                                                                                                         |
| `TARGET_NAME`         | Name of the target                                                                                                              |
| `TARGET_RESOURCE`     | Plural form of the target kind. i.e. `deployments`, `statefulsets` etc.                                                         |

The following variables are available only for database backup.

|       Variable        |                                                              Usage                                                              |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| `TARGET_APP_GROUP`    | Represents the application group where the respective app belongs (i.e: `kubedb.com`).                                          |
| `TARGET_APP_RESOURCE` | Represents the resource kind under that application group that the respective app works with (i.e: `postgres`).                 |
| `TARGET_APP_TYPE`     | Represents the total types of the application. It's simply `TARGET_APP_GROUP/TARGET_APP_RESOURCE` (i.e: `kubedb.com/postgres`). |

  If you use the sample `BackupBlueprint` that has been shown above to backup a Deployment named `my-deploy` of `test` namespace, the backed up data will be stored in `stash/test/deployment/my-deploy` directory of the `stash-backup` bucket. Similarly, if you want to backup a StatefulSet with name `my-sts` of same namespace, the backed up data will be stored in `/stash/test/statefulset/my-sts` directory of the backend.

- **spec.backend.\<backend type>.storageSecretName:** specifies the name of the secret that holds the access credentials to the backend.

>Note: `BackupBlueprint` is a non-namespaced crd. So, you can use a `BackupBlueprint` to backup targets in multiple namespaces. However, Storage Secret is a namespaced object. So, you have to manually create the secret in each namespace where you have a target for backup.

- **spec.wipeOut :** `spec.wipeOut` indicates whether Stash should delete the respective backed up data from the backend if a user deletes a `Repository` crd. For more details, please visit [here](/docs/v2023.10.9/concepts/crds/repository/#specwipeout).

#### BackupConfiguration Blueprint

You can provide a blueprint for the `BackupConfiguration` object that will be created for respective target using the following fields:

- **spec.schedule :** `spec.schedule` is the schedule that will be used to create `BackupConfiguration` for respective target. For more details, please visit [here](/docs/v2023.10.9/concepts/crds/backupconfiguration/#specschedule).

- **spec.backupHistoryLimit :** `spec.backupHistoryLimit` specifies a limit for backup history to keep for debugging purposes. For more details, please visit [here](/docs/v2023.10.9/concepts/crds/backupconfiguration/#specbackuphistroylimit).

- **spec.backupNamespace :**  `spec.backupNamespace` specifies the namespace where the backup resources (i.e. BackupConfiguration, BackupSession, Job, Repository etc.) will be created.

- **spec.retryConfig :** `spec.retryConfig` specifies a retry logic for failed backup. For more details, please visit [here](/docs/v2023.10.9/concepts/crds/backupconfiguration/#specretryconfig).

- **spec.task :** `spec.task` specifies the name and the parameters of [Task](/docs/v2023.10.9/concepts/crds/task/) to use to backup the target. You can template the name field with `TARGET_APP_VERSION` variable for database backup. Stash will replace this variable with respective database version. This will allow you to backup multiple database versions with the same `BackupBlueprint`. For more details, please check the following [guide](/docs/v2023.10.9/guides/auto-backup/database/).

- **spec.runtimeSettings :** `spec.runtimeSettings` allows to configure runtime environment for backup sidecar or job. For more details, please visit [here](/docs/v2023.10.9/concepts/crds/backupconfiguration/#specruntimesettings).

- **spec.tempDir :** `spec.tempDir` specifies the temporary volume setting that will be used to create respective `BackupConfiguration` object. For more details, please visit [here](/docs/v2023.10.9/concepts/crds/backupconfiguration/#spectempdir).

- **spec.interimVolumeTemplate :** `spec.interimVolumeTemplate` specifies a PVC template for holding data temporarily before uploading to the backend. For more details, please visit [here](/docs/v2023.10.9/concepts/crds/backupconfiguration/#specinterimvolumetemplate).

- **spec.retentionPolicy :** `spec.retentionPolicy` specifies the retention policies that will be used to create respective `BackupConfiguration` object. For more details, please visit [here](/docs/v2023.10.9/concepts/crds/backupconfiguration/#specretentionpolicy).

## Next Steps

- Learn how to use `BackupBlueprint` for auto backup of workloads data from [here](/docs/v2023.10.9/guides/auto-backup/workload/).
- Learn how to use `BackupBlueprint` for auto backup of database from [here](/docs/v2023.10.9/guides/auto-backup/database/).
- Learn how to use `BackupBlueprint` for auto backup of stand-alone PVC from [here](/docs/v2023.10.9/guides/auto-backup/pvc/).
