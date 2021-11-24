---
title: PostgreSQL | Stash
description: Stash auto-backup for PostgreSQL database
menu:
  docs_v2021.11.24:
    identifier: stash-postgres-auto-backup
    name: Auto-Backup
    parent: stash-postgres
    weight: 30
product_name: stash
menu_name: docs_v2021.11.24
section_menu_id: stash-addons
info:
  cli: v0.17.0
  community: v0.17.0
  elasticsearch:
  - 5.6.4-v14
  - 6.2.4-v14
  - 6.3.0-v14
  - 6.4.0-v14
  - 6.5.3-v14
  - 6.8.0-v14
  - 7.14.0
  - 7.2.0-v14
  - 7.3.2-v14
  enterprise: v0.17.0
  etcd:
  - 3.5.0-v1
  installer: v2021.11.24
  mariadb:
  - 10.5.8-v7
  mongodb:
  - 3.4.17-v13
  - 3.4.22-v13
  - 3.6.13-v13
  - 3.6.8-v13
  - 4.0.11-v13
  - 4.0.3-v13
  - 4.0.5-v13
  - 4.1.13-v13
  - 4.1.4-v13
  - 4.1.7-v13
  - 4.2.3-v13
  - 4.4.6-v4
  - 5.0.3-v1
  mysql:
  - 5.7.25-v14
  - 8.0.14-v14
  - 8.0.21-v8
  - 8.0.3-v14
  nats:
  - 2.6.1-v1
  percona-xtradb:
  - 5.7-v9
  postgres:
  - 10.14-v12
  - 11.9-v12
  - 12.4-v12
  - 13.1-v9
  - 14.0-v1
  - 9.6.19-v12
  redis:
  - 5.0.13-v2
  - 6.2.5-v2
  version: v2021.11.24
---

{{< notice type="warning" message="This is an Enterprise-only feature. Please install [Stash Enterprise Edition](/docs/v2021.11.24/setup/install/enterprise) to try this feature." >}}

# Backup PostgreSQL using Stash Auto-Backup

Stash can be configured to automatically backup any PostgreSQL database in your cluster. Stash enables cluster administrators to deploy backup blueprints ahead of time so that the database owners can easily backup their database with just a few annotations.

In this tutorial, we are going to show how you can configure a backup blueprint for PostgreSQL databases in your cluster and backup them with few annotations.

## Before You Begin

- At first, you need to have a Kubernetes cluster, and the `kubectl` command-line tool must be configured to communicate with your cluster.
- Install Stash Enterprise in your cluster following the steps [here](/docs/v2021.11.24/setup/install/enterprise).
- Install [KubeDB](https://kubedb.com) in your cluster following the steps [here](https://kubedb.com/docs/latest/setup/). This step is optional. You can deploy your database using any method you want.
- If you are not familiar with how Stash backup and restore PostgreSQL databases, please check the following guide [here](/docs/v2021.11.24/addons/postgres/overview/).
- If you are not familiar with how auto-backup works in Stash, please check the following guide [here](/docs/v2021.11.24/guides/latest/auto-backup/overview).
- If you are not familiar with the available auto-backup options for databases in Stash, please check the following guide [here](/docs/v2021.11.24/guides/latest/auto-backup/database).

You should be familiar with the following `Stash` concepts:

- [BackupBlueprint](/docs/v2021.11.24/concepts/crds/backupblueprint)
- [BackupConfiguration](/docs/v2021.11.24/concepts/crds/backupconfiguration)
- [BackupSession](/docs/v2021.11.24/concepts/crds/backupsession)
- [Repository](/docs/v2021.11.24/concepts/crds/repository)
- [Function](/docs/v2021.11.24/concepts/crds/function)
- [Task](/docs/v2021.11.24/concepts/crds/task)

In this tutorial, we are going to show backup of three different PostgreSQL databases on three different namespaces named `demo`, `demo-2`, and `demo-3`. Create the namespaces as below if you haven't done it already.

```bash
❯ kubectl create ns demo
namespace/demo created

❯ kubectl create ns demo-2
namespace/demo-2 created

❯ kubectl create ns demo-3
namespace/demo-3 created
```

When you install Stash Enterprise edition, it will automatically installs the official database addons. Make sure the addons for PostgreSQL was installed properly using the following command.

```bash
❯ kubectl get tasks.stash.appscode.com | grep postgres
postgres-backup-10.14      8d
postgres-backup-11.9       8d
postgres-backup-12.4       8d
postgres-backup-13.1       8d
postgres-backup-9.6.19     8d
postgres-restore-10.14     8d
postgres-restore-11.9      8d
postgres-restore-12.4      8d
postgres-restore-13.1      8d
postgres-restore-9.6.19    8d
```

## Prepare Backup Blueprint

To backup a PostgreSQL database using Stash, you have to create a `Secret` containing the backend credentials, a `Repository` containing the backend information, and a `BackupConfiguration` containing the schedule and target information. A `BackupBlueprint` allows you to specify a template for the `Repository` and the `BackupConfiguration`.

The `BackupBlueprint` is a non-namespaced CRD. So, once you have created a `BackupBlueprint`, you can use it to backup any PostgreSQL database of any namespace just by creating the storage `Secret` in that namespace and adding few annotations to your Postgres CRO. Then, Stash will automatically create a `Repository` and a `BackupConfiguration` according to the template to backup the database.

Below is the `BackupBlueprint` object that we are going to use in this tutorial,

```yaml
apiVersion: stash.appscode.com/v1beta1
kind: BackupBlueprint
metadata:
  name: postgres-backup-template
spec:
  # ============== Blueprint for Repository ==========================
  backend:
    gcs:
      bucket: stash-testing
      prefix: stash-backup/${TARGET_NAMESPACE}/${TARGET_APP_RESOURCE}/${TARGET_NAME}
    storageSecretName: gcs-secret
  # ============== Blueprint for BackupConfiguration =================
  task:
    name: postgres-backup-11.9
  schedule: "*/5 * * * *"
  retentionPolicy:
    name: 'keep-last-5'
    keepLast: 5
    prune: true
```

Here, we are using a GCS bucket as our backend. We are providing `gcs-secret` at the `storageSecretName` field. Hence, we have to create a secret named `gcs-secret` with the access credentials of our bucket in every namespace where we want to enable backup through this blueprint.

Notice the `prefix` field of `backend` section. We have used some variables in form of `${VARIABLE_NAME}`. Stash will automatically resolve those variables from the database information to make the backend prefix unique for each database instance.

Let's create the `BackupBlueprint` we have shown above,

```bash
❯ kubectl apply -f https://github.com/stashed/docs/raw/{{< param "info.version" >}}/docs/addons/postgres/auto-backup/examples/backupblueprint.yaml
backupblueprint.stash.appscode.com/postgres-backup-template created
```

Now, we are ready to backup our PostgreSQL databases using few annotations. You can check available auto-backup annotations for a database from [here](/docs/v2021.11.24/guides/latest/auto-backup/database#available-auto-backup-annotations-for-database).

## Auto-backup with default configurations

In this section, we are going to backup a PostgreSQL database of `demo` namespace. We are going to use the default configurations specified in the `BackupBlueprint`.

### Create Storage Secret

At first, let's create the `gcs-secret` in `demo` namespace with the access credentials to our GCS bucket.

```bash
❯ echo -n 'changeit' > RESTIC_PASSWORD
❯ echo -n '<your-project-id>' > GOOGLE_PROJECT_ID
❯ cat downloaded-sa-json.key > GOOGLE_SERVICE_ACCOUNT_JSON_KEY
❯ kubectl create secret generic -n demo gcs-secret \
    --from-file=./RESTIC_PASSWORD \
    --from-file=./GOOGLE_PROJECT_ID \
    --from-file=./GOOGLE_SERVICE_ACCOUNT_JSON_KEY
secret/gcs-secret created
```

### Create Database

Now, we are going to create a Postgres CRO in `demo` namespace. Below is the YAML of the PostgreSQL object that we are going to create,

```yaml
apiVersion: kubedb.com/v1alpha2
kind: Postgres
metadata:
  name: sample-postgres-1
  namespace: demo
  annotations:
    stash.appscode.com/backup-blueprint: postgres-backup-template
spec:
  version: "11.11"
  storageType: Durable
  storage:
    storageClassName: "standard"
    accessModes:
    - ReadWriteOnce
    resources:
      requests:
        storage: 1Gi
  terminationPolicy: Delete
```

Notice the `annotations` section. We are pointing to the `BackupBlueprint` that we have created earlier though `stash.appscode.com/backup-blueprint` annotation. Stash will watch this annotation and create a `Repository` and a `BackupConfiguration` according to the `BackupBlueprint`.

Let's create the above Postgres CRO,

```bash
❯ kubectl apply -f https://github.com/stashed/docs/raw/{{< param "info.version" >}}/docs/addons/postgres/auto-backup/examples/sample-pg-1.yaml
postgres.kubedb.com/sample-postgres-1 created
```

### Verify Auto-backup configured

In this section, we are going to verify whether Stash has created the respective `Repository` and `BackupConfiguration` for our PostgreSQL database we have just deployed.

#### Verify Repository

At first, let's verify whether Stash has created a `Repository` for our PostgreSQL or not.

```bash
❯ kubectl get repository -n demo
NAME                    INTEGRITY   SIZE   SNAPSHOT-COUNT   LAST-SUCCESSFUL-BACKUP   AGE
app-sample-postgres-1                                                                25s
```

Now, let's check the YAML of the `Repository`.

```yaml
❯ kubectl get repository -n demo app-sample-postgres-1 -o yaml
apiVersion: stash.appscode.com/v1alpha1
kind: Repository
metadata:
  name: app-sample-postgres-1
  namespace: demo
  ...
spec:
  backend:
    gcs:
      bucket: stash-testing
      prefix: stash-backup/demo/postgres/sample-postgres-1
    storageSecretName: gcs-secret
```

Here, you can see that Stash has resolved the variables in `prefix` field and substituted them with the equivalent information from this database.

#### Verify BackupConfiguration

Now, let's verify whether Stash has created a `BackupConfiguration` for our PostgreSQL or not.

```bash
❯ kubectl get backupconfiguration -n demo
NAME                    TASK                        SCHEDULE      PAUSED   AGE
app-sample-postgres-1   postgres-backup-11.9   */5 * * * *            97s
```

Now, let's check the YAML of the `BackupConfiguration`.

```yaml
❯ kubectl get backupconfiguration -n demo app-sample-postgres-1 -o yaml
apiVersion: stash.appscode.com/v1beta1
kind: BackupConfiguration
metadata:
  name: app-sample-postgres-1
  namespace: demo
  ...
spec:
  driver: Restic
  repository:
    name: app-sample-postgres-1
  retentionPolicy:
    keepLast: 5
    name: keep-last-5
    prune: true
  runtimeSettings: {}
  schedule: '*/5 * * * *'
  target:
    ref:
      apiVersion: appcatalog.appscode.com/v1alpha1
      kind: AppBinding
      name: sample-postgres-1
  task:
    name: postgres-backup-11.9
  tempDir: {}
status:
  conditions:
  - lastTransitionTime: "2021-02-23T09:38:19Z"
    message: Repository demo/app-sample-postgres-1 exist.
    reason: RepositoryAvailable
    status: "True"
    type: RepositoryFound
  - lastTransitionTime: "2021-02-23T09:38:19Z"
    message: Backend Secret demo/gcs-secret exist.
    reason: BackendSecretAvailable
    status: "True"
    type: BackendSecretFound
  - lastTransitionTime: "2021-02-23T09:38:19Z"
    message: Backup target appcatalog.appscode.com/v1alpha1 appbinding/sample-postgres-1
      found.
    reason: TargetAvailable
    status: "True"
    type: BackupTargetFound
  - lastTransitionTime: "2021-02-23T09:38:19Z"
    message: Successfully created backup triggering CronJob.
    reason: CronJobCreationSucceeded
    status: "True"
    type: CronJobCreated
  observedGeneration: 1

```

Notice the `target` section. Stash has automatically added the respective AppBinding of our PostgreSQL database as the target of this `BackupConfiguration`.

#### Verify Backup

Now, let's wait for a backup run to complete. You can watch for `BackupSession` as below,

```bash
❯  kubectl get backupsession -n demo -w
NAME                               INVOKER-TYPE          INVOKER-NAME            PHASE       AGE
app-sample-postgres-1-1614073215   BackupConfiguration   app-sample-postgres-1               0s
app-sample-postgres-1-1614073215   BackupConfiguration   app-sample-postgres-1   Running     3s
app-sample-postgres-1-1614073215   BackupConfiguration   app-sample-postgres-1   Succeeded   47s
```

Once the backup has been completed successfully, you should see the backed-up data has been stored in the bucket at the directory pointed by the `prefix` field of the `Repository`.

<figure align="center">
  <img alt="Backup data in GCS Bucket" src="/docs/v2021.11.24/addons/postgres/auto-backup/images/sample-postgres-1.png">
  <figcaption align="center">Fig: Backup data in GCS Bucket</figcaption>
</figure>

## Auto-backup with a custom schedule

In this section, we are going to backup a PostgreSQL database of `demo-2` namespace. This time, we are going to overwrite the default schedule used in the `BackupBlueprint`.

### Create Storage Secret

At first, let's create the backend Secret `gcs-secret` in `demo-2` namespace with the access credentials to our GCS bucket.

```bash
❯ kubectl create secret generic -n demo-2 gcs-secret \
    --from-file=./RESTIC_PASSWORD \
    --from-file=./GOOGLE_PROJECT_ID \
    --from-file=./GOOGLE_SERVICE_ACCOUNT_JSON_KEY
secret/gcs-secret created
```

### Create Database

Now, we are going to create a Postgres CRO in `demo-2` namespace. Below is the YAML of the PostgreSQL object that we are going to create,

```yaml
apiVersion: kubedb.com/v1alpha2
kind: Postgres
metadata:
  name: sample-postgres-2
  namespace: demo-2
  annotations:
    stash.appscode.com/backup-blueprint: postgres-backup-template
    stash.appscode.com/schedule: "*/3 * * * *"
spec:
  version: "11.11"
  storageType: Durable
  storage:
    storageClassName: "standard"
    accessModes:
    - ReadWriteOnce
    resources:
      requests:
        storage: 1Gi
  terminationPolicy: Delete
```

Notice the `annotations` section. This time, we have passed a schedule via `stash.appscode.com/schedule` annotation along with the `stash.appscode.com/backup-blueprint` annotation.

Let's create the above Postgres CRO,

```bash
❯ kubectl apply -f https://github.com/stashed/docs/raw/{{< param "info.version" >}}/docs/addons/postgres/auto-backup/examples/sample-pg-2.yaml
postgres.kubedb.com/sample-postgres-2 created
```

### Verify Auto-backup configured

Now, let's verify whether the auto-backup has been configured properly or not.

#### Verify Repository

At first, let's verify whether Stash has created a `Repository` for our PostgreSQL or not.

```bash
❯ kubectl get repository -n demo-2
NAME                    INTEGRITY   SIZE   SNAPSHOT-COUNT   LAST-SUCCESSFUL-BACKUP   AGE
app-sample-postgres-2                                                                13s
```

Now, let's check the YAML of the `Repository`.

```yaml
❯ kubectl get repository -n demo-2 app-sample-postgres-2 -o yaml
apiVersion: stash.appscode.com/v1alpha1
kind: Repository
metadata:
  name: app-sample-postgres-2
  namespace: demo-2
  ...
spec:
  backend:
    gcs:
      bucket: stash-testing
      prefix: stash-backup/demo-2/postgres/sample-postgres-2
    storageSecretName: gcs-secret
```

Here, you can see that Stash has resolved the variables in `prefix` field and substituted them with the equivalent information from this new database.

#### Verify BackupConfiguration

Now, let's verify whether Stash has created a `BackupConfiguration` for our PostgreSQL or not.

```bash
❯ kubectl get backupconfiguration -n demo-2
NAME                    TASK                        SCHEDULE      PAUSED   AGE
app-sample-postgres-2   postgres-backup-11.9   */3 * * * *            61s
```

Now, let's check the YAML of the `BackupConfiguration`.

```yaml
❯ kubectl get backupconfiguration -n demo-2 app-sample-postgres-2 -o yaml
apiVersion: stash.appscode.com/v1beta1
kind: BackupConfiguration
metadata:
  name: app-sample-postgres-2
  namespace: demo-2
  ...
spec:
  driver: Restic
  repository:
    name: app-sample-postgres-2
  retentionPolicy:
    keepLast: 5
    name: keep-last-5
    prune: true
  runtimeSettings: {}
  schedule: '*/3 * * * *'
  target:
    ref:
      apiVersion: appcatalog.appscode.com/v1alpha1
      kind: AppBinding
      name: sample-postgres-2
  task:
    name: postgres-backup-11.9
  tempDir: {}
status:
  conditions:
  - lastTransitionTime: "2021-02-23T09:44:33Z"
    message: Repository demo-2/app-sample-postgres-2 exist.
    reason: RepositoryAvailable
    status: "True"
    type: RepositoryFound
  - lastTransitionTime: "2021-02-23T09:44:33Z"
    message: Backend Secret demo-2/gcs-secret exist.
    reason: BackendSecretAvailable
    status: "True"
    type: BackendSecretFound
  - lastTransitionTime: "2021-02-23T09:44:33Z"
    message: Backup target appcatalog.appscode.com/v1alpha1 appbinding/sample-postgres-2
      found.
    reason: TargetAvailable
    status: "True"
    type: BackupTargetFound
  - lastTransitionTime: "2021-02-23T09:44:33Z"
    message: Successfully created backup triggering CronJob.
    reason: CronJobCreationSucceeded
    status: "True"
    type: CronJobCreated
  observedGeneration: 1
```

Notice the `schedule` section. This time the `BackupConfiguration` has been created with the schedule we have provided via annotation.

Also, notice the `target` section. Stash has automatically added the new PostgreSQL as the target of this `BackupConfiguration`.

#### Verify Backup

Now, let's wait for a backup run to complete. You can watch for `BackupSession` as below,

```bash
❯  kubectl get backupsession -n demo-2 -w
NAME                               INVOKER-TYPE          INVOKER-NAME            PHASE       AGE
app-sample-postgres-2-1614073502   BackupConfiguration   app-sample-postgres-2               0s
app-sample-postgres-2-1614073502   BackupConfiguration   app-sample-postgres-2   Running     2s
app-sample-postgres-2-1614073502   BackupConfiguration   app-sample-postgres-2   Succeeded   48s
```

Once the backup has been completed successfully, you should see that Stash has created a new directory as pointed by the `prefix` field of the new `Repository` and stored the backed-up data there.

<figure align="center">
  <img alt="Backup data in GCS Bucket" src="/docs/v2021.11.24/addons/postgres/auto-backup/images/sample-postgres-2.png">
  <figcaption align="center">Fig: Backup data in GCS Bucket</figcaption>
</figure>

## Auto-backup with custom parameters

In this section, we are going to backup a PostgreSQL database of `demo-3` namespace. This time, we are going to pass some parameters for the Task through the annotations.

### Create Storage Secret

At first, let's create the `gcs-secret` in `demo-3` namespace with the access credentials to our GCS bucket.

```bash
❯ kubectl create secret generic -n demo-3 gcs-secret \
    --from-file=./RESTIC_PASSWORD \
    --from-file=./GOOGLE_PROJECT_ID \
    --from-file=./GOOGLE_SERVICE_ACCOUNT_JSON_KEY
secret/gcs-secret created
```

### Create Database

Now, we are going to create a Postgres CRO in `demo-3` namespace. Below is the YAML of the PostgreSQL object that we are going to create,

```yaml
apiVersion: kubedb.com/v1alpha2
kind: Postgres
metadata:
  name: sample-postgres-3
  namespace: demo-3
  annotations:
    stash.appscode.com/backup-blueprint: postgres-backup-template
    params.stash.appscode.com/args: --no-owner --clean
spec:
  version: "11.11"
  storageType: Durable
  storage:
    storageClassName: "standard"
    accessModes:
    - ReadWriteOnce
    resources:
      requests:
        storage: 1Gi
  terminationPolicy: Delete
```

Notice the `annotations` section. This time, we have passed an argument via `params.stash.appscode.com/args` annotation along with the `stash.appscode.com/backup-blueprint` annotation.

Let's create the above Postgres CRO,

```bash
❯ kubectl apply -f https://github.com/stashed/docs/raw/{{< param "info.version" >}}/docs/addons/postgres/auto-backup/examples/sample-pg-3.yaml
postgres.kubedb.com/sample-postgres-3 created
```

### Verify Auto-backup configured

Now, let's verify whether the auto-backup resources has been created or not.

#### Verify Repository

At first, let's verify whether Stash has created a `Repository` for our PostgreSQL or not.

```bash
❯ kubectl get repository -n demo-3
NAME                    INTEGRITY   SIZE   SNAPSHOT-COUNT   LAST-SUCCESSFUL-BACKUP   AGE
app-sample-postgres-3                                                                17s
```

Now, let's check the YAML of the `Repository`.

```yaml
❯ kubectl get repository -n demo-3 app-sample-postgres-3 -o yaml
apiVersion: stash.appscode.com/v1alpha1
kind: Repository
metadata:
  name: app-sample-postgres-3
  namespace: demo-3
  ...
spec:
  backend:
    gcs:
      bucket: stash-testing
      prefix: stash-backup/demo-3/postgres/sample-postgres-3
    storageSecretName: gcs-secret
```

Here, you can see that Stash has resolved the variables in `prefix` field and substituted them with the equivalent information from this new database.

#### Verify BackupConfiguration

Now, let's verify whether Stash has created a `BackupConfiguration` for our PostgreSQL or not.

```bash
❯ kubectl get backupconfiguration -n demo-3
NAME                    TASK                        SCHEDULE      PAUSED   AGE
app-sample-postgres-3   postgres-backup-11.9   */5 * * * *            51s
```

Now, let's check the YAML of the `BackupConfiguration`.

```yaml
❯ kubectl get backupconfiguration -n demo-3 app-sample-postgres-3 -o yaml
apiVersion: stash.appscode.com/v1beta1
kind: BackupConfiguration
metadata:
  name: app-sample-postgres-3
  namespace: demo-3
  ...
spec:
  driver: Restic
  repository:
    name: app-sample-postgres-3
  retentionPolicy:
    keepLast: 5
    name: keep-last-5
    prune: true
  runtimeSettings: {}
  schedule: '*/5 * * * *'
  target:
    ref:
      apiVersion: appcatalog.appscode.com/v1alpha1
      kind: AppBinding
      name: sample-postgres-3
  task:
    name: postgres-backup-11.9
    params:
    - name: args
      value: --no-owner --clean
  tempDir: {}
status:
  conditions:
  - lastTransitionTime: "2021-02-23T09:48:15Z"
    message: Repository demo-3/app-sample-postgres-3 exist.
    reason: RepositoryAvailable
    status: "True"
    type: RepositoryFound
  - lastTransitionTime: "2021-02-23T09:48:15Z"
    message: Backend Secret demo-3/gcs-secret exist.
    reason: BackendSecretAvailable
    status: "True"
    type: BackendSecretFound
  - lastTransitionTime: "2021-02-23T09:48:15Z"
    message: Backup target appcatalog.appscode.com/v1alpha1 appbinding/sample-postgres-3
      found.
    reason: TargetAvailable
    status: "True"
    type: BackupTargetFound
  - lastTransitionTime: "2021-02-23T09:48:15Z"
    message: Successfully created backup triggering CronJob.
    reason: CronJobCreationSucceeded
    status: "True"
    type: CronJobCreated
  observedGeneration: 1
```

Notice the `task` section. The `args` parameter that we had passed via annotations has been added to the `params` section.

Also, notice the `target` section. Stash has automatically added the new PostgreSQL as the target of this `BackupConfiguration`.

#### Verify Backup

Now, let's wait for a backup run to complete. You can watch for `BackupSession` as below,

```bash
❯  kubectl get backupsession -n demo-3 -w
NAME                               INVOKER-TYPE          INVOKER-NAME            PHASE       AGE
app-sample-postgres-3-1614073808   BackupConfiguration   app-sample-postgres-3               0s
app-sample-postgres-3-1614073808   BackupConfiguration   app-sample-postgres-3   Running     3s
app-sample-postgres-3-1614073808   BackupConfiguration   app-sample-postgres-3   Succeeded   47s
```

Once the backup has been completed successfully, you should see that Stash has created a new directory as pointed by the `prefix` field of the new `Repository` and stored the backed-up data there.

<figure align="center">
  <img alt="Backup data in GCS Bucket" src="/docs/v2021.11.24/addons/postgres/auto-backup/images/sample-postgres-3.png">
  <figcaption align="center">Fig: Backup data in GCS Bucket</figcaption>
</figure>

## Cleanup

To cleanup the resources crated by this tutorial, run the following commands,

```bash
❯ kubectl delete -f https://github.com/stashed/docs/raw/{{< param "info.version" >}}/docs/addons/postgres/auto-backup/examples/
backupblueprint.stash.appscode.com "postgres-backup-template" deleted
postgres.kubedb.com "sample-postgres-1" deleted
postgres.kubedb.com "sample-postgres-2" deleted
postgres.kubedb.com "sample-postgres-3" deleted

❯ kubectl delete repository -n demo --all
repository.stash.appscode.com "app-sample-postgres-1" deleted
❯ kubectl delete repository -n demo-2 --all
repository.stash.appscode.com "app-sample-postgres-2" deleted
❯ kubectl delete repository -n demo-3 --all
repository.stash.appscode.com "app-sample-postgres-3" deleted
```
