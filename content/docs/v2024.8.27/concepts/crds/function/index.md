---
title: Function Overview
menu:
  docs_v2024.8.27:
    identifier: function-overview
    name: Function
    parent: crds
    weight: 30
product_name: stash
menu_name: docs_v2024.8.27
section_menu_id: concepts
info:
  cli: v0.35.0
  community: v0.35.0
  elasticsearch:
  - 5.6.4-v32
  - 6.2.4-v32
  - 6.3.0-v32
  - 6.4.0-v32
  - 6.5.3-v32
  - 6.8.0-v32
  - 7.14.0-v18
  - 7.2.0-v32
  - 7.3.2-v32
  - 8.2.0-v15
  enterprise: v0.35.0
  etcd:
  - 3.5.0-v19
  installer: v2024.8.27
  kubedump:
  - 0.1.0-v15
  mariadb:
  - 10.5.8-v26
  mongodb:
  - 3.4.17-v32
  - 3.4.22-v32
  - 3.6.13-v32
  - 3.6.8-v32
  - 4.0.11-v32
  - 4.0.3-v32
  - 4.0.5-v32
  - 4.1.13-v32
  - 4.1.4-v32
  - 4.1.7-v32
  - 4.2.3-v32
  - 4.4.6-v23
  - 5.0.15-v5
  - 5.0.3-v20
  - 6.0.5-v8
  mysql:
  - 5.7.25-v32
  - 8.0.14-v32
  - 8.0.21-v26
  - 8.0.3-v32
  nats:
  - 2.6.1-v20
  - 2.8.2-v15
  percona-xtradb:
  - 5.7-v27
  postgres:
  - 10.14-v31
  - 11.9-v31
  - 12.4-v31
  - 13.1-v28
  - 14.0-v20
  - 15.1-v12
  - 16.1-v1
  - 9.6.19-v31
  redis:
  - 5.0.13-v20
  - 6.2.5-v20
  - 7.0.5-v13
  ui-server: v0.16.0
  vault:
  - 1.10.3-v12
  version: v2024.8.27
---

> New to Stash? Please start [here](/docs/v2024.8.27/concepts/README).

# Function

## What is Function

A complete backup or restore process may consist of several steps. For example, in order to backup a PostgreSQL database we first need to dump the database and upload the dumped file to a backend. Then we need to update the respective`Repository` and `BackupSession` status and send Prometheus metrics. In Stash, we call such individual steps a `Function`.

A `Function` is a Kubernetes `CustomResourceDefinition`(CRD) which basically specifies a template for a container that performs only a specific action. For example, `postgres-backup-*` function only dumps and uploads the dumped file into the backend where `update-status` function updates the status of respective `BackupSession` and `Repository` and sends Prometheus metrics to pushgateway based on the output of `postgres-backup-*` function.

When you install Stash, some `Function`s will be pre-installed for supported targets like databases, etc. However, you can create your own function to customize or extend the backup/restore process.

## Function CRD Specification

Like any official Kubernetes resource, a `Function` has `TypeMeta`, `ObjectMeta` and `Spec` sections. However, unlike other Kubernetes resources, it does not have a `Status` section.

A sample `Function` object to backup a PostgreSQL is shown below,

```yaml
apiVersion: stash.appscode.com/v1beta1
kind: Function
metadata:
  name: postgres-backup-11.2
spec:
  image: stashed/postgres-stash:11.2
  args:
  - backup-pg
  # setup information
  - --provider=${REPOSITORY_PROVIDER:=}
  - --bucket=${REPOSITORY_BUCKET:=}
  - --endpoint=${REPOSITORY_ENDPOINT:=}
  - --region=${REPOSITORY_REGION:=}
  - --path=${REPOSITORY_PREFIX:=}
  - --secret-dir=/etc/repository/secret
  - --scratch-dir=/tmp
  - --enable-cache=${ENABLE_CACHE:=true}
  - --max-connections=${MAX_CONNECTIONS:=0} # 0 indicates use default connection limit
  - --hostname=${HOSTNAME:=}
  - --backup-cmd=${backupCMD:=} # can specify dump command with either pg_dump or pg_dumpall
  - --pg-args=${args:=} # optional arguments pass to pgdump command
  - --wait-timeout=${waitTimeout:=}
  # target information
  - --namespace=${NAMESPACE:=default}
  - --appbinding=${TARGET_NAME:=}
  - --backupsession=${BACKUP_SESSION:=}
  # cleanup information
  - --retention-keep-last=${RETENTION_KEEP_LAST:=0}
  - --retention-prune=${RETENTION_PRUNE:=false}
  # output & metric information
  - --output-dir=${outputDir:=}
  volumeMounts:
  - name: ${secretVolume}
    mountPath: /etc/repository/secret
  runtimeSettings:
    resources:
      requests:
        memory: 256M
      limits:
        memory: 256M
    securityContext:
      runAsUser: 5000
      runAsGroup: 5000
```

A sample `Function` that updates `BackupSession` and `Repository`  status and sends metrics to Prometheus pushgateway is shown below,

```yaml
apiVersion: stash.appscode.com/v1beta1
kind: Function
metadata:
  name: update-status
spec:
  image: appscode/stash:0.10.0
  args:
  - update-status
  - --provider=${REPOSITORY_PROVIDER:=}
  - --bucket=${REPOSITORY_BUCKET:=}
  - --endpoint=${REPOSITORY_ENDPOINT:=}
  - --path=${REPOSITORY_PREFIX:=}
  - --secret-dir=/etc/repository/secret
  - --scratch-dir=/tmp
  - --enable-cache=${ENABLE_CACHE:=true}
  - --max-connections=${MAX_CONNECTIONS:=0}
  - --namespace=${NAMESPACE:=default}
  - --backupsession=${BACKUP_SESSION:=}
  - --repository=${REPOSITORY_NAME:=}
  - --invoker-kind=${INVOKER_KIND:=}
  - --invoker-name=${INVOKER_NAME:=}
  - --target-kind=${TARGET_KIND:=}
  - --target-name=${TARGET_NAME:=}
  - --output-dir=${outputDir:=}
  - --metrics-enabled=true
  - --metrics-pushgateway-url=http://stash.kube-system.svc:56789
  - --prom-job-name=${PROMETHEUS_JOB_NAME:=}
  volumeMounts:
  - mountPath: /etc/repository/secret
    name: ${secretVolume}
```

Here, we are going to describe the various sections of a `Function` crd.

### Function `Spec`

A `Function` object has the following fields in the `spec` section:

#### spec.image

`spec.image` specifies the docker image to use to create a container using the template specified in this `Function`.

#### spec.command

`spec.command` specifies the commands to be executed by the container. Docker image's `ENTRYPOINT` will be executed if no commands are specified.

#### spec.args

`spec.args` specifies a list of arguments that will be passed to the entrypoint. You can templatize this section using `envsubst` style variables. Stash will resolve all the variables before creating the respective container. A variable should follow the following patterns:

- ${VARIABLE_NAME:=default-value}
- ${VARIABLE_NAME:=}

In the first case, if Stash can't resolve the variable, the default value will be used in place of this variable. In the second case, if Stash can't resolve the variable, an empty string will be used to replace the variable.

##### Stash Provided Variables

Stash operator provides the following built-in variables based on `BackupConfiguration`, `BackupSession`, `RestoreSession`, `Repository`, `Task`, `Function`, `BackupBlueprint` etc.

| Environment Variable         | Usage                                                                                                                                                                            |
| ---------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `NAMESPACE`                  | Namespace of backup or restore job/workload                                                                                                                                      |
| `BACKUP_SESSION`             | Name of the respective BackupSession object                                                                                                                                      |
| `RESTORE_SESSION`            | Name of the respective RestoreSession object                                                                                                                                     |
| `REPOSITORY_NAME`            | Name of the Repository object that holds respective backend information                                                                                                          |
| `REPOSITORY_PROVIDER`        | Type of storage provider. i.e. gcs, s3, aws, local etc.                                                                                                                          |
| `REPOSITORY_SECRET_NAME`     | Name of the secret that holds the credentials to access the backend                                                                                                              |
| `REPOSITORY_BUCKET`          | Name of the bucket where backed up data will be stored                                                                                                                           |
| `REPOSITORY_PREFIX`          | A prefix of the directory inside bucket where backed up data will be stored                                                                                                      |
| `REPOSITORY_ENDPOINT`        | URL of S3 compatible Minio/Rook server                                                                                                                                           |
| `REPOSITORY_URL`             | URL of the REST server for REST backend                                                                                                                                          |
| `HOSTNAME`                   | An identifier for the backed up data. If multiple pods backup in same Repository (i.e. StatefulSet or DaemonSet) this host name is to used identify data of the individual host. |
| `SOURCE_HOSTNAME`            | An identifier of the host whose backed up data will be restored                                                                                                                  |
| `TARGET_NAME`                | Name of the target of backup or restore                                                                                                                                          |
| `TARGET_API_VERSION`         | API version of the target of backup or restore                                                                                                                                   |
| `TARGET_KIND`                | Kind of the target of backup or restore                                                                                                                                          |
| `TARGET_NAMESPACE`           | Namespace of the target object for backup or restore                                                                                                                             |
| `TARGET_MOUNT_PATH`          | Directory where target PVC will be mounted in stand-alone PVC backup or restore                                                                                                  |
| `TARGET_PATHS`               | Array of file paths that are subject to backup                                                                                                                                   |
| `RESTORE_PATHS`              | Array of file paths that are subject to restore                                                                                                                                  |
| `RESTORE_SNAPSHOTS`          | Name of the snapshot that will be restored                                                                                                                                       |
| `TARGET_APP_VERSION`         | Version of the application pointed by an AppBinding                                                                                                                              |
| `TARGET_APP_GROUP`           | The application group where the app pointed by an AppBinding belongs                                                                                                             |
| `TARGET_APP_RESOURCE`        | The resource kind under an application group that the app pointed by an AppBinding works with                                                                                    |
| `TARGET_APP_TYPE`            | The total types of the application. It's simply `TARGET_APP_GROUP/TARGET_APP_RESOURCE`                                                                                           |
| `TARGET_APP_REPLICAS`        | Number of replicas of an application targeted for backup or restore                                                                                                              |
| `RETENTION_KEEP_LAST`        | Number of latest snapshots to keep                                                                                                                                               |
| `RETENTION_KEEP_HOURLY`      | Number of hourly snapshots to keep                                                                                                                                               |
| `RETENTION_KEEP_DAILY`       | Number of daily snapshots to keep                                                                                                                                                |
| `RETENTION_KEEP_WEEKLY`      | Number of weekly snapshots to keep                                                                                                                                               |
| `RETENTION_KEEP_MONTHLY`     | Number of monthly snapshots to keep                                                                                                                                              |
| `RETENTION_KEEP_YEARLY`      | Number of yearly snapshots to keep                                                                                                                                               |
| `RETENTION_KEEP_TAGS`        | Keep only those snapshots that have these tags                                                                                                                                   |
| `RETENTION_PRUNE`            | Specify whether to remove data of old snapshot completely from the backend                                                                                                       |
| `RETENTION_DRY_RUN`          | Specify whether to run cleanup in test mode                                                                                                                                      |
| `ENABLE_CACHE`               | Specify whether to use cache while backup or restore                                                                                                                             |
| `MAX_CONNECTIONS`            | Specifies number of parallel connections to upload/download data to/from backend                                                                                                 |
| `NICE_ADJUSTMENT`            | Adjustment value to configure `nice` to throttle the load on cpu.                                                                                                                |
| `IONICE_CLASS`               | Name of the `ionice` class                                                                                                                                                       |
| `IONICE_CLASS_DATA`          | Value of the `ionice` class data                                                                                                                                                 |
| `ENABLE_STATUS_SUBRESOURCE`  | Specifies whether crd has subresource enabled                                                                                                                                    |
| `PROMETHEUS_PUSHGATEWAY_URL` | URL of the Prometheus pushgateway that collects the backup/restore metrics                                                                                                       |
| `INTERIM_DATA_DIR`           | Directory to store backed up or restored data temporarily before uploading to the backend or injecting into the target                                                           |

If you want to use a variable that is not present this table, you have to provide its value in `spec.task.params` section of `BackupConfiguration` crd.

#### spec.workDir

`spec.workDir` specifies the container's working directory. If this field is not specified, the container's runtime default will be used.

#### spec.ports

`spec.ports` specifies a list of the ports to expose from the respective container that will be created for this function.

#### spec.volumeMounts

`spec.volumeMounts` specifies a list of volume names and their `mountPath` that will be mounted into the container that will be created for this function.

#### spec.volumeDevices

`spec.volumeDevices` specifies a list of the block devices to be used by the container that will be created for this function.

#### spec.runtimeSettings

`spec.runtimeSettings.container` allows to configure runtime environment of a backup job at container level. You can configure the following container level parameters:

| Field             | Usage                                                                                                                                                                                                                      |
| ----------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `resources`       | Compute resources required by sidecar container or backup job. To know how to manage resources for containers, please visit [here](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/). |
| `livenessProbe`   | Periodic probe of backup sidecar/job container's liveness. Container will be restarted if the probe fails.                                                                                                                 |
| `readinessProbe`  | Periodic probe of backup sidecar/job container's readiness. Container will be removed from service endpoints if the probe fails.                                                                                           |
| `lifecycle`       | Actions that the management system should take in response to container lifecycle events.                                                                                                                                  |
| `securityContext` | Security options that backup sidecar/job's container should run with. For more details, please visit [here](https://kubernetes.io/docs/concepts/policy/security-context/).                                                 |
| `nice`            | Set CPU scheduling priority for the backup process. For more details about `nice`, please visit [here](https://www.askapache.com/optimize/optimize-nice-ionice/#nice).                                                     |
| `ionice`          | Set I/O scheduling class and priority for the backup process. For more details about `ionice`, please visit [here](https://www.askapache.com/optimize/optimize-nice-ionice/#ionice).                                       |
| `env`             | A list of the environment variables to set in the container that will be created for this function.                                                                                                                        |
| `envFrom`         | This allows to set environment variables to the container that will be created for this function from a Secret or ConfigMap.                                                                                               |

#### spec.podSecurityPolicyName

If you are using a [PSP enabled cluster](https://kubernetes.io/docs/concepts/policy/pod-security-policy/) and the function needs any specific permission then you can specify the PSP name using `spec.podSecurityPolicyName` field. Stash will add this PSP in the respective RBAC roles that will be created for this function.

>Note that Stash operator can't give permission to use a PSP to a backup job if the operator itself does not have permission to use it. So, if you want to specify PSP name in this section, make sure to add that in `stash-operator` ClusterRole too.

## Next Steps

- Learn how to use `Function` to create a `Task` from [here](/docs/v2024.8.27/concepts/crds/task/).
