---
title: Using Workload Idenity with Stash on GKE
description: A guide on how to use GKE workload identity with Stash
menu:
  docs_v2025.6.30:
    identifier: platforms-gke
    name: GKE Workload Identity
    parent: platforms
    weight: 18
product_name: stash
menu_name: docs_v2025.6.30
section_menu_id: guides
info:
  cli: v0.40.0
  community: v0.40.0
  elasticsearch:
  - 5.6.4-v36
  - 6.2.4-v36
  - 6.3.0-v36
  - 6.4.0-v36
  - 6.5.3-v36
  - 6.8.0-v36
  - 7.14.0-v22
  - 7.2.0-v36
  - 7.3.2-v36
  - 8.2.0-v19
  enterprise: v0.40.0
  etcd:
  - 3.5.0-v23
  installer: v2025.6.30
  kubedump:
  - 0.2.0-v4
  mariadb:
  - 10.5.8-v30
  mongodb:
  - 3.4.17-v37
  - 3.4.22-v37
  - 3.6.13-v37
  - 3.6.8-v37
  - 4.0.11-v37
  - 4.0.3-v37
  - 4.0.5-v37
  - 4.1.13-v37
  - 4.1.4-v37
  - 4.1.7-v37
  - 4.2.3-v37
  - 4.4.6-v28
  - 5.0.15-v10
  - 5.0.3-v25
  - 6.0.5-v13
  mysql:
  - 5.7.25-v37
  - 8.0.14-v36
  - 8.0.21-v30
  - 8.0.3-v36
  nats:
  - 2.6.1-v24
  - 2.8.2-v19
  percona-xtradb:
  - 5.7-v31
  postgres:
  - 10.14-v35
  - 11.9-v35
  - 12.4-v35
  - 13.1-v32
  - 14.0-v24
  - 15.1-v16
  - 16.1-v5
  - 17.2-v3
  - 9.6.19-v35
  redis:
  - 5.0.13-v24
  - 6.2.5-v24
  - 7.0.5-v17
  ui-server: v0.21.0
  vault:
  - 1.10.3-v16
  version: v2025.6.30
---

# Using Workload Identity with Stash on Google Kubernetes Engine (GKE)

This guide will show you how to use workload identity of [Google Kubernetes Engine (GKE)](https://cloud.google.com/kubernetes-engine/) with Stash. Here, we are going to backup a MariaDB database and store the backed up data into a [GCS Bucket](https://cloud.google.com/storage/). Then, we are going to show how to restore this backed up data.

## Before You Begin

- At first, you need to have a Kubernetes cluster in the Google Cloud Platform with [Workload Identity](https://cloud.google.com/kubernetes-engine/docs/how-to/workload-identity) enabled. If you don’t already have a cluster, create one from [here](https://cloud.google.com/kubernetes-engine/docs/how-to/workload-identity#console).

- Install `Stash` in your cluster following the steps [here](/docs/v2025.6.30/setup/README).
- Install `KubeDB` operator in your cluster following the steps [here](https://kubedb.com/docs/latest/setup/).
- You should be familiar with the following `Stash` concepts:
  - [BackupConfiguration](/docs/v2025.6.30/concepts/crds/backupconfiguration/)
  - [BackupSession](/docs/v2025.6.30/concepts/crds/backupsession/)
  - [RestoreSession](/docs/v2025.6.30/concepts/crds/restoresession/)
  - [Repository](/docs/v2025.6.30/concepts/crds/repository/)
- Install Google Cloud CLI following the steps [here](https://cloud.google.com/sdk/downloads).
- You will need a [GCS Bucket](https://console.cloud.google.com/storage/).

To keep everything isolated, we are going to use a separate namespace called `demo` throughout this tutorial.

```bash
$ kubectl create ns demo
namespace/demo created
```

## Prepare IAM Service Account

At first, let's create a IAM service account which will contain the roles for accessing GCS Bucket,

```bash
$ gcloud iam service-accounts create bucket-accessor \
    --project=sample-project
```

Let's add the required roles to this service account for accessing the GCS bucket.

```bash
$ gcloud projects add-iam-policy-binding sample-project \
    --member "serviceAccount:bucket-accessor@sample-project.iam.gserviceaccount.com" \
    --role "roles/storage.objectAdmin"
```

```bash
$ gcloud projects add-iam-policy-binding sample-project \
    --member "serviceAccount:bucket-accessor@sample-project.iam.gserviceaccount.com" \
    --role "roles/storage.admin"
```

> For GCS backend, if the bucket does not exist, Stash needs `Storage Object Admin` role permissions to create the bucket. For more details, please check the following [guide](/docs/v2025.6.30/guides/backends/gcs/).

## Prepare MariaDB 

In this section, we are going to deploy a MariaDB database using KubeDB. Then, we are going to insert some sample data into it.

### Deploy MariaDB using KubeDB

At first, let’s deploy a MariaDB standalone database named `sample-mariadb` using KubeDB,

```yaml
apiVersion: kubedb.com/v1alpha2
kind: MariaDB
metadata:
  name: sample-mariadb
  namespace: demo
spec:
  version: "10.5.8"
  replicas: 1
  storageType: Durable
  storage:
    storageClassName: "standard"
    accessModes:
    - ReadWriteOnce
    resources:
      requests:
        storage: 1Gi
  terminationPolicy: WipeOut
```

```bash
$ kubectl apply -f https://github.com/stashed/docs/raw/{{< param "info.version" >}}/docs/guides/platforms/gke/examples/mariadb.yaml
mariadb.kubedb.com/sample-mariadb created
```

Now, wait for the database pod `sample-mariadb-0` to go into Running state,

```bash
$ kubectl get pod -n demo sample-mariadb-0
NAME               READY   STATUS    RESTARTS   AGE
sample-mariadb-0   1/1     Running   0          29m
```

Once the database pod is in Running state, verify that the database is ready to accept the connections.

```bash
$ kubectl logs -n demo sample-mariadb-0
2021-02-22  9:41:37 0 [Note] Reading of all Master_info entries succeeded
2021-02-22  9:41:37 0 [Note] Added new Master_info '' to hash table
2021-02-22  9:41:37 0 [Note] mysqld: ready for connections.
Version: '10.5.8-MariaDB-1:10.5.8+maria~focal'  socket: '/run/mysqld/mysqld.sock'  port: 3306  mariadb.org binary distribution
```

From the above log, we can see the database is ready to accept connections.

### Insert Sample Data

Now, we are going to exec into the database pod and create some sample data. The sample-mariadb object creates a secret containing the credentials of MariaDB and set them as pod’s Environment varibles `MYSQL_ROOT_USERNAME` and `MYSQL_ROOT_PASSWORD`.

Here, we are going to use the root user (`MYSQL_ROOT_USERNAME`) credential `MYSQL_ROOT_PASSWORD` to insert the sample data. Let’s exec into the database pod and insert some sample data,

```bash
$ kubectl exec -it -n demo sample-mariadb-0 -- bash
root@sample-mariadb-0:/ mysql -u${MYSQL_ROOT_USERNAME} -p${MYSQL_ROOT_PASSWORD}
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 341
Server version: 10.5.8-MariaDB-1:10.5.8+maria~focal mariadb.org binary distribution

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

# Let's create a database named "company"
MariaDB [(none)]>  create database company;
Query OK, 1 row affected (0.000 sec)

# Verify that the database has been created successfully
MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| company            |
| information_schema |
| mysql              |
| performance_schema |
+--------------------+
4 rows in set (0.001 sec)

# Now, let's create a table called "employee" in the "company" table
MariaDB [(none)]> create table company.employees ( name varchar(50), salary int);
Query OK, 0 rows affected (0.018 sec)

# Verify that the table has been created successfully
MariaDB [(none)]> show tables in company;
+-------------------+
| Tables_in_company |
+-------------------+
| employees         |
+-------------------+
1 row in set (0.007 sec)

# Now, let's insert a sample row in the table
MariaDB [(none)]> insert into company.employees values ('John Doe', 5000);
Query OK, 1 row affected (0.003 sec)

# Insert another sample row
MariaDB [(none)]> insert into company.employees values ('James William', 7000);
Query OK, 1 row affected (0.002 sec)

# Verify that the rows have been inserted into the table successfully
MariaDB [(none)]>  select * from company.employees;
+---------------+--------+
| name          | salary |
+---------------+--------+
| John Doe      |   5000 |
| James William |   7000 |
+---------------+--------+
2 rows in set (0.001 sec)

MariaDB [(none)]> exit
Bye
```

We have successfully deployed a MariaDB database and inserted some sample data into it.

## Prepare Backup

In this section, we are going to prepare the necessary resources (i.e. database connection information, backend information, etc.) before backup.

### Verify Stash MariaDB Addon Installed

When you install the Stash, it automatically installs all the official database addons. Verify that it has installed the MariaDB addons using the following command.

```bash
$ kubectl get tasks.stash.appscode.com | grep mariadb
mariadb-backup-10.5.8    35s
mariadb-restore-10.5.8   35s
```

### Ensure AppBinding

Stash needs to know how to connect with the database. An `AppBinding` exactly provides this information. It holds the Service and Secret information of the database. You have to point to the respective `AppBinding` as a target of backup instead of the database itself.

Stash expect your database Secret to have `username` and `password` keys. If your database secret does not have them, the `AppBinding` can also help here. You can specify a `secretTransforms` section with the mapping between the current keys and the desired keys.

You don’t need to worry about appbindings if you are using KubeDB. It creates an appbinding containing the necessary informations when you deploy the database. Let’s ensure the appbinding create by `KubeDB` operator.

```bash
$ kubectl get appbinding -n demo 
NAME             TYPE                 VERSION   AGE
sample-mariadb   kubedb.com/mariadb   10.5.8      62m
```

We have a appbinding named same as database name sample-mariadb. We will use this later for connecting into this database.

### Prepare Backend

We are going to store our backed up data into a [GCS bucket](https://cloud.google.com/storage/). As we are using workload identity enabled cluster, we don't need the `GOOGLE_PROJECT_ID` and `GOOGLE_SERVICE_ACCOUNT_JSON_KEY` to access the GCS bucket.

At first, we need to create a secret with a Restic password. Then, we have to create a `Repository` crd that will hold the information about our backend storage.

**Create Secret:**

Let's create a secret called `encryption-secret` with the Restic password,

```bash
$ echo -n 'changeit' > RESTIC_PASSWORD
$ kubectl create secret generic -n demo encryption-secret \
    --from-file=./RESTIC_PASSWORD \
secret "encryption-secret" created
```

**Create Repository:**

Now, let's create a `Repository` with the information of our desired GCS bucket. Below is the YAML of `Repository` crd we are going to create,

```yaml
apiVersion: stash.appscode.com/v1alpha1
kind: Repository
metadata:
  name: gcs-repo
  namespace: demo
spec:
  backend:
    gcs:
      bucket: stash-testing
      prefix: /demo/mariadb/sample-mariadb
    storageSecretName: encryption-secret
```

Let's create the `Repository` we have shown above,

```bash
$ kubectl apply -f https://github.com/stashed/docs/raw/{{< param "info.version" >}}/docs/guides/platforms/gke/examples/repository.yaml
repository.stash.appscode.com/gcs-repo created
```

Now, we are ready to backup our sample data into this backend.

### Prepare ServiceAccount

Now we are going create a Kubernetes service account and bind it with the IAM service account `bucket-accessor` that we have created earlier. This binding allows the Kubernetes service account to act as the IAM service account.

Lets create a `ServiceAccount`,

```bash
$ kubectl create serviceaccount -n demo bucket-user
```

Let's add the IAM annotations to the `ServiceAccount`,

```bash
$ kubectl annotate sa -n demo bucket-user iam.gke.io/gcp-service-account="bucket-accessor@appscode-testing.iam.gserviceaccount.com"
```

Now Let's bind it with the IAM service account,

```bash
$ gcloud iam service-accounts add-iam-policy-binding bucket-accessor@sample-project.iam.gserviceaccount.com \
    --role roles/iam.workloadIdentityUser \
    --member "serviceAccount:sample-project.svc.id.goog[demo/bucket-user]"
```

## Backup

To schedule a backup, we have to create a `BackupConfiguration` object targeting the respective AppBinding of our desired database. Then Stash will create a CronJob to periodically backup the database.

**Create BackupConfiguration:**

Below is the `YAML` for BackupConfiguration object we are going to use to backup the sample-mariadb database we have deployed earlier,

```yaml
apiVersion: stash.appscode.com/v1beta1
kind: BackupConfiguration
metadata:
  name: sample-mariadb-backup
  namespace: demo
spec:
  runtimeSettings:
    pod:
      serviceAccountName: bucket-user
  schedule: "*/5 * * * *"
  repository:
    name: gcs-repo
  target:
    ref:
      apiVersion: appcatalog.appscode.com/v1alpha1
      kind: AppBinding
      name: sample-mariadb
  retentionPolicy:
    name: keep-last-5
    keepLast: 5
    prune: true
```

Here,

- `spec.runtimeSettins.pod.serviceAccountName` refers to the name of the `ServiceAccount` to use in the backup pod.
- `spec.repository` refers to the `Repository` object `gcs-repo` that holds backend [GCS bucket](https://cloud.google.com/storage/) information.
- `spec.target.ref`refers to the AppBinding object that holds the connection information of our targeted database.

Let's create the `BackupConfiguration` crd we have shown above,

```bash
$ kubectl apply -f https://github.com/stashed/docs/raw/{{< param "info.version" >}}/docs/guides/platforms/gke/examples/backupconfiguration.yaml
backupconfiguration.stash.appscode.com/sample-mariadb-backup created
```

**Verify Backup Setup Successful:**

If everything goes well, the phase of the `BackupConfiguration` should be `Ready`. The `Ready` phase indicates that the backup setup is successful. Let’s verify the Phase of the `BackupConfiguration`,

```bash
$ kubectl get backupconfiguration -n demo
NAME                    TASK                    SCHEDULE      PAUSED   PHASE      AGE
sample-mariadb-backup   mariadb-backup-10.5.8   */5 * * * *            Ready      11s
```

**Verify CronJob:**

Stash will create a CronJob with the schedule specified in `spec.schedule` field of `BackupConfiguration` object.

Verify that the CronJob has been created using the following command,

```bash
$ kubectl get cronjob -n demo
NAME                                 SCHEDULE      SUSPEND   ACTIVE   LAST SCHEDULE   AGE
stash-backup-sample-mariadb-backup   */5 * * * *   False     0        15s             17s
```

**Wait for BackupSession:**

The `sample-mariadb-backup` CronJob will trigger a backup on each scheduled slot by creating a `BackupSession` object.

Now, wait for a schedule to appear. Run the following command to watch for a `BackupSession` object,

```bash
$ kubectl get backupsession -n demo -w
NAME                               INVOKER-TYPE          INVOKER-NAME            PHASE     AGE
sample-mariadb-backup-1606994706   BackupConfiguration   sample-mariadb-backup   Running   24s
sample-mariadb-backup-1606994706   BackupConfiguration   sample-mariadb-backup   Running   75s
sample-mariadb-backup-1606994706   BackupConfiguration   sample-mariadb-backup   Succeeded   103s

```

Here, the phase `Succeeded` means that the backup process has been completed successfully.

**Verify Backup:**

Now, we are going to verify whether the backed up data is present in the backend or not. Once a backup is completed, Stash will update the respective `Repository` object to reflect the backup completion. Check that the repository `gcs-repo` has been updated by the following command,

```bash
$ kubectl get repository -n demo gcs-repo
NAME       INTEGRITY   SIZE        SNAPSHOT-COUNT   LAST-SUCCESSFUL-BACKUP   AGE
gcs-repo   true        1.327 MiB   1                60s                      8m
```

Now, if we navigate to the GCS bucket, we will see the backed up data has been stored in `demo/mariadb/sample-mariadb` directory as specified by `.spec.backend.gcs.prefix` field of the Repository object.

<figure align="center">
  <img alt="Backup data in GCS Bucket" src="/docs/v2025.6.30/guides/platforms/gke/images/gke.png">
  <figcaption align="center">Fig: Backup data in GCS Bucket</figcaption>
</figure>

> **Note:** Stash keeps all the backed up data encrypted. So, data in the backend will not make any sense until they are decrypted.

## Restore

In this section, we are going to show you how to restore in the same database which may be necessary when you have accidentally deleted any data from the running database.

**Temporarily Pause Backup:**

At first, let's stop taking any further backup of the database so that no backup runs after we delete the sample data. We are going to pause the `BackupConfiguration` object. Stash will stop taking any further backup when the `BackupConfiguration` is paused.

Let's pause the `sample-mariadb-backup` BackupConfiguration,

```bash
$ kubectl patch backupconfiguration -n demo sample-mariadb-backup--type="merge" --patch='{"spec": {"paused": true}}'
backupconfiguration.stash.appscode.com/sample-mgo-rs-backup patched
```

Or you can use the Stash `kubectl` plugin to pause the `BackupConfiguration`,

```bash
$ kubectl stash pause backup -n demo --backupconfig=sample-mariadb-backup
BackupConfiguration demo/sample-mariadb-backup has been paused successfully.
```

Verify that the `BackupConfiguration` has been paused,

```bash
$ kubectl get backupconfiguration -n demo sample-mariadb-backup
NAME                   TASK                    SCHEDULE      PAUSED   PHASE   AGE
sample-mariadb-backup  mariadb-backup-10.5.8   */5 * * * *   true     Ready   26m
```

Notice the `PAUSED` column. Value `true` for this field means that the `BackupConfiguration` has been paused.

Stash will also suspend the respective CronJob.

```bash
$ kubectl get cronjob -n demo
NAME                                 SCHEDULE      SUSPEND   ACTIVE   LAST SCHEDULE   AGE
stash-backup-sample-mariadb-backup   */5 * * * *   True      0        2m59s           20m
```

**Simulate Disaster:**

Now, let's simulate an accidental deletion scenario. Here, we are going to exec into the database pod and delete the `company` database we had created earlier.

```bash
$ kubectl exec -it -n demo sample-mariadb-0 -c mariadb -- bash
root@sample-mariadb-0:/ mysql -u${MYSQL_ROOT_USERNAME} -p${MYSQL_ROOT_PASSWORD}
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 341
Server version: 10.5.8-MariaDB-1:10.5.8+maria~focal mariadb.org binary distribution

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

# View current databases
MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| company            |
| information_schema |
| mysql              |
| performance_schema |
+--------------------+
4 rows in set (0.001 sec)

# Let's delete the "company" database
MariaDB [(none)]> drop database company;
Query OK, 1 row affected (0.268 sec)

# Verify that the "company" database has been deleted
MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
+--------------------+
3 rows in set (0.000 sec)

MariaDB [(none)]> exit
Bye
```

**Create RestoreSession:**

To restore the database, you have to create a `RestoreSession` object pointing to the `AppBinding` of the targeted database.

Here, is the YAML of the `RestoreSession` object that we are going to use for restoring our `sample-mariadb` database.

```yaml
apiVersion: stash.appscode.com/v1beta1
kind: RestoreSession
metadata:
  name: sample-mariadb-restore
  namespace: demo
spec:
  runtimeSettings:
    pod:
      serviceAccountName: bucket-user
  repository:
    name: gcs-repo
  target:
    ref:
      apiVersion: appcatalog.appscode.com/v1alpha1
      kind: AppBinding
      name: sample-mariadb
  rules:
  - snapshots: [latest]
```

Here,

- `spec.runtimeSettins.pod.serviceAccountName` refers to the name of the `ServiceAccount` to use in the restore pod.
- `spec.repository.name` specifies the Repository object that holds the backend information where our backed up data has been stored.
- `spec.target.ref` refers to the respective AppBinding of the `sample-mariadb` database.
- `spec.rules` specifies that we are restoring data from the latest backup snapshot of the database.

Let's create the `RestoreSession` object object we have shown above,

```bash
$ kubectl apply -f https://github.com/stashed/docs/raw/{{< param "info.version" >}}/docs/guides/platforms/gke/examples/restoresession.yaml
restoresession.stash.appscode.com/sample-mariadb-restore created
```

Once, you have created the `RestoreSession` object, Stash will create a restore Job. Run the following command to watch the phase of the `RestoreSession` object,

```bash
$ kubectl get restoresession -n demo -w
NAME                     REPOSITORY   PHASE     AGE
sample-mariadb-restore   gcs-repo     Running   15s
sample-mariadb-restore   gcs-repo     Succeeded   18s
```

The `Succeeded` phase means that the restore process has been completed successfully.

**Verify Restored Data:**

Now, let's exec into the database pod and verify whether data actual data was restored or not,

```bash
$ kubectl exec -it -n demo sample-mariadb-0 -c mariadb -- bash
root@sample-mariadb-0:/ mysql -u${MYSQL_ROOT_USERNAME} -p${MYSQL_ROOT_PASSWORD}
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 341
Server version: 10.5.8-MariaDB-1:10.5.8+maria~focal mariadb.org binary distribution

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

# Verify that the "company" database has been restored
MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| company            |
| information_schema |
| mysql              |
| performance_schema |
+--------------------+
4 rows in set (0.001 sec)

# Verify that the tables of the "company" database have been restored
MariaDB [(none)]> show tables from company;
+-------------------+
| Tables_in_company |
+-------------------+
| employees         |
+-------------------+
1 row in set (0.000 sec)

# Verify that the sample data of the "employees" table has been restored
MariaDB [(none)]> select * from company.employees;
+---------------+--------+
| name          | salary |
+---------------+--------+
| John Doe      |   5000 |
| James William |   7000 |
+---------------+--------+
2 rows in set (0.000 sec)

MariaDB [(none)]> exit
Bye
```

Hence, we can see from the above output that the deleted data has been restored successfully from the backup.

**Resume Backup**

Since our data has been restored successfully we can now resume our usual backup process. Resume the `BackupConfiguration` using following command,

```bash
$ kubectl patch backupconfiguration -n demo sample-mariadb-backup --type="merge" --patch='{"spec": {"paused": false}}'
backupconfiguration.stash.appscode.com/sample-mariadb-backup patched
```

Or you can use the Stash `kubectl` plugin to resume the `BackupConfiguration`,

```bash
$ kubectl stash resume -n demo --backupconfig=sample-mariadb-backup
BackupConfiguration demo/sample-mariadb-backup has been resumed successfully.
```

Verify that the `BackupConfiguration` has been resumed,

```bash
$ kubectl get backupconfiguration -n demo sample-mariadb-backup
NAME                    TASK                    SCHEDULE      PAUSED   PHASE   AGE
sample-mariadb-backup   mariadb-backup-10.5.8   */5 * * * *   false    Ready   29m
```

Here,  `false` in the `PAUSED` column means the backup has been resume successfully. The CronJob also should be resumed now.

```bash
$ kubectl get cronjob -n demo
NAME                                 SCHEDULE      SUSPEND   ACTIVE   LAST SCHEDULE   AGE
stash-backup-sample-mariadb-backup   */5 * * * *   False     0        2m59s           29m
```

Here, `False` in the `SUSPEND` column means the CronJob is no longer suspended and will trigger in the next schedule.

## Allow Operator to List Snapshots

When you list Snapshots using `kubectl get snapshot` command, Stash operator itself read the Snapshots directly from the backend. So, the operator needs permission to access the bucket.
Stash operator has it own's `ServiceAccount`. Therefore, this `ServiceAccount` should be binded with the IAM service account as well.  Run the following command to get the service account used by the Stash operator,

```bash
$ kubectl get serviceaccount -n stash stash-stash-enterprise
NAME                                   SECRETS   AGE
stash-stash-enterprise                 1         9m52s
```

Let's add the IAM annotations to the `ServiceAccount`,

```bash
$ kubectl annotate sa -n stash stash-stash-enterprise iam.gke.io/gcp-service-account="bucket-accessor@appscode-testing.iam.gserviceaccount.com"
```

Now Let's bind it with the IAM service account,

```bash
$ gcloud iam service-accounts add-iam-policy-binding bucket-accessor@sample-project.iam.gserviceaccount.com \
    --role roles/iam.workloadIdentityUser \
    --member "serviceAccount:sample-project.svc.id.goog[stash/stash-stash-enterprise]"
```

### Cleanup

To cleanup the Kubernetes resources created by this tutorial, run:

```bash
kubectl delete -n demo backupconfiguration sample-mariadb-backup
kubectl delete -n demo restoresession sample-mariadb-restore
kubectl delete -n demo sa bucket-user
kubectl delete -n demo secret encryption-secret
kubectl delete -n demo repository gcs-repo
kubectl delete -n demo mariadb sample-mariadb
kubectl delete ns demo
```
