---
title: Backup & Restore Percona XtraDB Database | Stash
description: Backup & Restore a standalone Percona XtraDB Database database using Stash
menu:
  docs_v2025.6.30:
    identifier: stash-percona-xtradb-standalone
    name: Standalone Percona XtraDB
    parent: stash-percona-xtradb
    weight: 20
product_name: stash
menu_name: docs_v2025.6.30
section_menu_id: stash-addons
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

# Backup and Restore Percona XtraDB database using Stash

Stash 0.9.0+ supports backup and restoration of Percona XtraDB databases. This guide will show you how you can backup and restore your Percona XtraDB database with Stash.

## Before You Begin

- At first, you need to have a Kubernetes cluster, and the `kubectl` command-line tool must be configured to communicate with your cluster. If you do not already have a cluster, you can create one by using Minikube.
- Install Stash in your cluster following the steps [here](/docs/v2025.6.30/setup/README).
- Install [KubeDB](https://kubedb.com) in your cluster following the steps [here](https://kubedb.com/docs/latest/setup/). This step is optional. You can deploy your database using any method you want. We are using KubeDB because KubeDB simplifies many of the difficult or tedious management tasks to run a production-grade database on private and public clouds.
- If you are not familiar with how Stash takes backup and restores Percona XtraDB databases, please check the following guide [here](/docs/v2025.6.30/addons/percona-xtradb/overview/).

You have to be familiar with the following custom resources:

- [AppBinding](/docs/v2025.6.30/concepts/crds/appbinding/)
- [Function](/docs/v2025.6.30/concepts/crds/function/)
- [Task](/docs/v2025.6.30/concepts/crds/task/)
- [BackupConfiguration](/docs/v2025.6.30/concepts/crds/backupconfiguration/)
- [RestoreSession](/docs/v2025.6.30/concepts/crds/restoresession/)

To keep things isolated, we are going to use a separate namespace called `demo` throughout this tutorial. Create `demo` namespace if you haven't created yet.

```bash
$ kubectl create ns demo
namespace/demo created
```

> Note: YAML files used in this tutorial are stored [here](https://github.com/stashed/docs/tree/{{< param "info.version" >}}/docs/addons/percona-xtradb/standalone/examples).

## Backup Percona XtraDB

This section will demonstrate how to backup a Percona XtraDB database. Here, we are going to deploy a Percona XtraDB database using KubeDB. Then, we are going to back up this database into a GCS bucket. Finally, we are going to restore the backed up data into another Percona XtraDB database.

### Deploy Sample Percona XtraDB Database

Let's deploy a sample Percona XtraDB database and insert some data into it.

#### Create Percona XtraDB CRD

Below is the YAML of a sample `PerconaXtraDB` CRD that we are going to create for this tutorial:

```yaml
apiVersion: kubedb.com/v1alpha2
kind: PerconaXtraDB
metadata:
  name: sample-xtradb
  namespace: demo
spec:
  version: "5.7"
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

Create the above `PerconaXtraDB` CRD,

```bash
$ kubectl apply -f https://github.com/stashed/docs/raw/{{< param "info.version" >}}/docs/addons/percona-xtradb/standalone/examples/sample-xtradb.yaml
perconaxtradb.kubedb.com/sample-xtradb created
```

KubeDB will deploy a Percona XtraDB database according to the above specification. It will also create the necessary Secrets and Services to access the database.

Let's check if the database is ready to use,

```bash
$ kubectl get px -n demo sample-xtradb
NAME            VERSION   STATUS         AGE
sample-xtradb   5.7       Provisioning   54s
```

The database is `Running`. Verify that KubeDB has created a Secret and a Service for this database using the following commands,

```bash
$ kubectl get secret -n demo -l=app.kubernetes.io/instance=sample-xtradb
NAME                 TYPE     DATA   AGE
sample-xtradb-auth   Opaque   2      85s

$ kubectl get service -n demo -l=app.kubernetes.io/instance=sample-xtradb
NAME                TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
sample-xtradb       ClusterIP   10.108.43.167   <none>        3306/TCP   111s
sample-xtradb-gvr   ClusterIP   None            <none>        3306/TCP   111s
```

Here, we have to use service `sample-xtradb` and secret `sample-xtradb-auth` to connect with the database. KubeDB creates an [AppBinding](/docs/v2025.6.30/concepts/crds/appbinding/) CRD that holds the necessary information to connect with the database.

#### Verify AppBinding

Verify that the `AppBinding` has been created successfully using the following command,

```bash
$ kubectl get appbindings -n demo
NAME            TYPE                       VERSION   AGE
sample-xtradb   kubedb.com/perconaxtradb   5.7       89s
```

Let's check the YAML of the above AppBinding,

```bash
$ kubectl get appbindings -n demo sample-xtradb -o yaml
```

Output is as follows,

```yaml
apiVersion: appcatalog.appscode.com/v1alpha1
kind: AppBinding
metadata:
  creationTimestamp: "2020-01-26T06:57:35Z"
  generation: 1
  labels:
    app.kubernetes.io/component: database
    app.kubernetes.io/instance: sample-xtradb
    app.kubernetes.io/managed-by: kubedb.com
    app.kubernetes.io/name: perconaxtradbs.kubedb.com
  name: sample-xtradb
  namespace: demo
  ownerReferences:
  - apiVersion: kubedb.com/v1alpha2
    blockOwnerDeletion: true
    controller: true
    kind: PerconaXtraDB
    name: sample-xtradb
    uid: 279e90e5-7596-4cd2-b971-99e9a3dff839
  resourceVersion: "16218"
  selfLink: /apis/appcatalog.appscode.com/v1alpha1/namespaces/demo/appbindings/sample-xtradb
  uid: 5aff9236-e886-4a0d-b767-85d639cfe7c4
spec:
  clientConfig:
    service:
      name: sample-xtradb
      path: /
      port: 3306
      scheme: mysql
    url: tcp(sample-xtradb:3306)/
  secret:
    name: sample-xtradb-auth
  type: kubedb.com/perconaxtradb
  version: "5.7"
```

Stash uses the AppBinding CRD to connect with the target database. It requires the following two fields to be set in the AppBinding's `.spec` section.

- `.spec.clientConfig.service.name` specifies the name of the Service that connects to the database.
- `.spec.secret` specifies the name of the Secret that holds the necessary credentials to access the database.
- `.spec.type` specifies the type of the app that this AppBinding is pointing to. The format KubeDB generated AppBinding follows to set the value of `.spec.type` is `<app_group>/<app_resource_type>`.

#### Provisioning AppBinding Manually

If you deploy the Percona XtraDB database without KubeDB, you have to create the AppBinding CRD manually in the same namespace as the service and secret of the database.

The following YAML shows a minimal AppBinding specification that you have to create if you deploy the Percona XtraDB database without KubeDB.

```yaml
apiVersion: appcatalog.appscode.com/v1alpha1
kind: AppBinding
metadata:
  name: your-custom-appbinding-name
  namespace: your-database-namespace
spec:
  clientConfig:
    service:
      name: your-database-service-name
      port: 3306
      scheme: mysql
  secret:
    name: your-database_credentials_secret_name>
  # type field is optional. you can keep it empty.
  # if you keep it empty then the value of TARGET_APP_RESOURCE variable
  # will be set to "appbinding" during auto-backup.
  type: kubedb.com/perconaxtradb
```

You have to replace the `<...>` quoted part with proper values in the above YAML.

#### Insert Sample Data

Now, we are going to exec into the database pod and create some sample data. At first, find out the database pods using the following command,

```bash
$ kubectl get pods -n demo --selector="app.kubernetes.io/instance=sample-xtradb"
NAME              READY   STATUS    RESTARTS   AGE
sample-xtradb-0   1/1     Running   0          6m56s
```

And copy the username and password of the `root` user to access into `mysql` shell.

```bash
$ kubectl get secret -n demo  sample-xtradb-auth -o jsonpath='{.data.username}'| base64 -d
root⏎

$ kubectl get secret -n demo  sample-xtradb-auth -o jsonpath='{.data.password}'| base64 -d
5qtWP192NLD-nwgd⏎
```

Now, let's exec into the Pod to enter into `mysql` shell and create a database and a table,

```bash
$ kubectl exec -it -n demo sample-xtradb-0 -- mysql --user=root --password=5qtWP192NLD-nwgd
mysql: [Warning] Using a password on the command line interface can be insecure.
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 48
Server version: 5.7.26-29 Percona Server (GPL), Release 29, Revision 11ad961

Copyright (c) 2009-2019 Percona LLC and/or its affiliates
Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> CREATE DATABASE playground;
Query OK, 1 row affected (0.00 sec)

mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| playground         |
| sys                |
+--------------------+
5 rows in set (0.00 sec)

mysql> CREATE TABLE playground.equipment ( id INT NOT NULL AUTO_INCREMENT, type VARCHAR(50), quant INT, color VARCHAR(25), PRIMARY KEY(id));
Query OK, 0 rows affected (0.06 sec)

mysql> INSERT INTO playground.equipment (type, quant, color) VALUES ("slide", 2, "blue");
Query OK, 1 row affected (0.01 sec)

mysql> SELECT * FROM playground.equipment;
+----+-------+-------+-------+
| id | type  | quant | color |
+----+-------+-------+-------+
|  1 | slide |     2 | blue  |
+----+-------+-------+-------+
1 row in set (0.00 sec)

mysql> exit
Bye
```

Now, we are ready to back up the database.

### Prepare Backend

We are going to store our backed up data into a GCS bucket. At first, we need to create a secret with GCS credentials then we need to create a `Repository` CRD. If you want to use a different backend, please read the respective backend configuration doc from [here](/docs/v2025.6.30/guides/backends/overview/).

#### Create Storage Secret

Let's create a secret called `gcs-secret` with access credentials to our desired GCS bucket,

```bash
$ echo -n 'changeit' > RESTIC_PASSWORD
$ echo -n '<your-project-id>' > GOOGLE_PROJECT_ID
$ cat downloaded-sa-json.key > GOOGLE_SERVICE_ACCOUNT_JSON_KEY
$ kubectl create secret generic -n demo gcs-secret \
    --from-file=./RESTIC_PASSWORD \
    --from-file=./GOOGLE_PROJECT_ID \
    --from-file=./GOOGLE_SERVICE_ACCOUNT_JSON_KEY
secret/gcs-secret created
```

#### Create Repository

Now, crete a `Repository` using this secret. Below is the YAML of Repository CRD we are going to create,

```yaml
apiVersion: stash.appscode.com/v1alpha1
kind: Repository
metadata:
  name: gcs-repo-sample-xtradb
  namespace: demo
spec:
  backend:
    gcs:
      bucket: appscode-qa
      prefix: /demo/xtradb/sample-xtradb
    storageSecretName: gcs-secret
```

Let's create the `Repository` we have shown above,

```bash
$ kubectl create -f https://github.com/stashed/docs/raw/{{< param "info.version" >}}/docs/addons/percona-xtradb/standalone/examples/repository.yaml
repository.stash.appscode.com/gcs-repo-sample-xtradb created
```

Now, we are ready to back up our database to our desired backend.

### Backup

We have to create a `BackupConfiguration` targeting respective AppBinding CRD of our desired database. Then Stash will create a CronJob to periodically backup the database.

#### Create BackupConfiguration

Below is the YAML for `BackupConfiguration` CRD to backup the `sample-xtradb` database we have deployed earlier,

```yaml
apiVersion: stash.appscode.com/v1beta1
kind: BackupConfiguration
metadata:
  name: sample-xtradb-backup
  namespace: demo
spec:
  schedule: "*/5 * * * *"
  task:
    name: perconaxtradb-backup-5.7
  repository:
    name: gcs-repo-sample-xtradb
  target:
    ref:
      apiVersion: appcatalog.appscode.com/v1alpha1
      kind: AppBinding
      name: sample-xtradb
  retentionPolicy:
    name: keep-last-5
    keepLast: 5
    prune: true
```

Here,

- `.spec.schedule` specifies that we want to back up the database at 5 minutes interval.
- `.spec.task.name` specifies the name of the Task CRD that specifies the necessary Functions and their execution order to backup a Percona XtraDB database.
- `.spec.target.ref` refers to the AppBinding CRD that was created for the `sample-xtradb` database.

Let's create the `BackupConfiguration` CRD we have shown above,

```bash
$ kubectl create -f https://github.com/stashed/docs/raw/{{< param "info.version" >}}/docs/addons/percona-xtradb/standalone/examples/backupconfiguration.yaml
backupconfiguration.stash.appscode.com/sample-xtradb-backup created
```

#### Verify Backup Setup Successful

If everything goes well, the phase of the `BackupConfiguration` should be `Ready`. The `Ready` phase indicates that the backup setup is successful. Let's verify the `Phase` of the BackupConfiguration,

```bash
$ kubectl get backupconfiguration -n demo
NAME                   TASK                       SCHEDULE      PAUSED   PHASE      AGE
sample-xtradb-backup   perconaxtradb-backup-5.7   */5 * * * *            Ready      11s
```

```bash
$ kubectl describe backupconfiguration -n demo sample-xtradb-backup
```

#### Verify CronJob

Stash will create a CronJob with the schedule specified in `.spec.schedule` field of `BackupConfiguration` CRD.

Verify that the CronJob has been created using the following command,

```bash
$ kubectl get cronjob -n demo
NAME                                SCHEDULE      SUSPEND   ACTIVE   LAST SCHEDULE   AGE
stash-backup-sample-xtradb-backup   */5 * * * *   False     0        <none>          38s
```

#### Wait for BackupSession

The `sample-xtradb-backup` CronJob will trigger a backup on each scheduled slot by Provisioning a `BackupSession` CRD.

Wait for a schedule to appear. Run the following command to watch `BackupSession` CRD,

```bash
$ kubectl get backupsession -n demo -l=stash.appscode.com/invoker-name=sample-xtradb-backup --watch
NAME                              INVOKER-TYPE          INVOKER-NAME           PHASE       AGE
sample-xtradb-backup-1580023322   BackupConfiguration   sample-xtradb-backup               0s
sample-xtradb-backup-1580023322   BackupConfiguration   sample-xtradb-backup               0s
sample-xtradb-backup-1580023322   BackupConfiguration   sample-xtradb-backup   Running     0s
sample-xtradb-backup-1580023322   BackupConfiguration   sample-xtradb-backup   Running     36s
sample-xtradb-backup-1580023322   BackupConfiguration   sample-xtradb-backup   Succeeded   36s
```

Here, the phase **`Succeeded`** means that the backupsession has been succeeded.

>Note: Backup CronJob creates `BackupSession` CRD the label `stash.appscode.com/invoker-name=<BackupConfiguration_crd_name>`. We can use this label to watch only the `BackupSession` of our desired `BackupConfiguration`.

#### Verify Backup

Now, we are going to verify whether the backed up data is in the backend. Once a backup is completed, Stash will update the respective `Repository` CRD to reflect the backup completion. Check that the repository `gcs-repo-sample-xtradb` has been updated by the following command,

```bash
$ kubectl get repository -n demo gcs-repo-sample-xtradb
NAME                     INTEGRITY   SIZE   SNAPSHOT-COUNT   LAST-SUCCESSFUL-BACKUP   AGE
gcs-repo-sample-xtradb   true               3                82s                      17m
```

Now, if we navigate to the GCS bucket, we will see the backed up data has been stored in `demo/xtradb/sample-xtradb` directory as specified by `.spec.backend.gcs.prefix` field of Repository CRD.

<figure align="center">
  <img alt="Backed up data in GCS Bucket" src="/docs/v2025.6.30/addons/percona-xtradb/standalone/images/sample-xtradb-backup.png">
  <figcaption align="center">Fig: Backed up data in GCS Bucket</figcaption>
</figure>

> Note: Stash keeps all the backed up data encrypted. So, data in the backend will not make any sense until they are decrypted.

### Restore Percona XtraDB

In this section, we are going to restore the database from the backup we have taken in the previous section. We are going to deploy a new database and initialize it from the backup.

#### Stop Taking Backup of the Old Database

At first, let's stop taking any further backup of the old database so that no backup is taken during the restore process. We are going to pause the `BackupConfiguration` CRD that we had created to backup the `sample-xtradb` database. Then, Stash will stop taking any further backup for this database.

Let's pause the `sample-xtradb-backup` BackupConfiguration,

```console
$ kubectl patch backupconfiguration -n demo sample-xtradb-backup --type="merge" --patch='{"spec": {"paused": true}}'
backupconfiguration.stash.appscode.com/sample-xtradb-backup patched
```

Now, wait for a moment. Stash will pause the BackupConfiguration. Verify that the operator has paused the BackupConfiguration object,

```console
$ kkubectl get backupconfiguration -n demo sample-xtradb-backup
NAME                   TASK                        SCHEDULE      PAUSED   PHASE   AGE
sample-xtradb-backup   perconaxtradb-backup-5.7    */5 * * * *   true     Ready   13m
```

Notice the `PAUSED` column. Value `true` for this field means that the BackupConfiguration has been paused.

#### Deploy Restored Database

Now, we have to deploy the restored database similarly as we have deployed the original `sample-xtradb` database. However, this time there will be the following differences:

- We have to use the same secret that was used in the original database. We are going to specify it using `.spec.databaseSecret` field.
- We are going to specify `.spec.init.waitForInitialRestore: true` which tells KubeDB to wait for the initial restore to complete before marking this database as ready to use.

Below is the YAML for `PerconaXtraDB` CRD we are going deploy to initialize from backup,

```yaml
apiVersion: kubedb.com/v1alpha2
kind: PerconaXtraDB
metadata:
  name: restored-xtradb
  namespace: demo
spec:
  version: "5.7"
  replicas: 1
  authSecret:
    name: sample-xtradb-auth
  storageType: Durable
  storage:
    storageClassName: "standard"
    accessModes:
    - ReadWriteOnce
    resources:
      requests:
        storage: 1Gi
  init:
    waitForInitialRestore: true
  terminationPolicy: WipeOut
```

Let's create the above database,

```bash
$ kubectl apply -f https://github.com/stashed/docs/raw/{{< param "info.version" >}}/docs/addons/percona-xtradb/standalone/restored-xtradb.yaml
perconaxtradb.kubedb.com/restored-xtradb created
```

If you check the database status, you will see it is stuck in **`Provisioning`** state.

```bash
$ kubectl get px -n demo restored-xtradb --watch
NAME              VERSION   STATUS         AGE
restored-xtradb   5.7       Provisioning   42s
restored-xtradb   5.7       Provisioning   74s
```

#### Create RestoreSession

Now, we need to create a `RestoreSession` CRD pointing to the newly created restored database.

Using the following command, check that another AppBinding object has been created for the `restored-xtradb` object,

```bash
$ kubectl get appbindings -n demo restored-xtradb
NAME              TYPE                       VERSION   AGE
restored-xtradb   kubedb.com/perconaxtradb   5.7       4m6s
```

> If you are not using KubeDB to deploy database, create the AppBinding manually.

Below is the contents of YAML file of the RestoreSession CRD that we are going to create to restore the backed up data into the newly created database provisioned by PerconaXtrDB CRD named `restored-xtradb`.

```yaml
apiVersion: stash.appscode.com/v1beta1
kind: RestoreSession
metadata:
  name: restored-xtradb-restore
  namespace: demo
spec:
  task:
    name: perconaxtradb-restore-5.7
  repository:
    name: gcs-repo-sample-xtradb
  target:
    ref:
      apiVersion: appcatalog.appscode.com/v1alpha1
      kind: AppBinding
      name: restored-xtradb
  rules:
  - snapshots: ["latest"]
```

Here,

- `.spec.task.name` specifies the name of the Task CRD that specifies the necessary Functions and their execution order to restore a Percona XtraDB database.
- `.spec.repository.name` specifies the Repository CRD that holds the backend information where our backed up data has been stored.
- `.spec.target.ref` refers to the  AppBinding object for the `restored-xtradb` PerconaXtraDB object.
- `.spec.rules` specifies that we are restoring data from the `latest` backup snapshot of the database.

Let's create the RestoreSession CRD object we have shown above,

```bash
$ kubectl apply -f https://github.com/stashed/docs/raw/{{< param "info.version" >}}/docs/addons/percona-xtradb/standalone/restoresession.yaml
restoresession.stash.appscode.com/restored-xtradb-restore created
```

Once you have created the RestoreSession object, Stash will create a restore Job. We can watch the phase of the RestoreSession object to check whether the restore process has succeeded or not.

Run the following command to watch the phase of the RestoreSession object,

```bash
$ kubectl get restoresession -n demo restored-xtradb-restore --watch
NAME                      REPOSITORY               PHASE       AGE
restored-xtradb-restore   gcs-repo-sample-xtradb   Running     35s
restored-xtradb-restore   gcs-repo-sample-xtradb   Succeeded   44s
```

Here, we can see from the output of the above command that the restore process succeeded.

#### Verify Restored Data

In this section, we are going to verify whether the desired data has restored successfully. We are going to connect to the database server and check whether the database and the table we created earlier in the original database have restored.

At first, check if the database has gone into **`Ready`** state,

```bash
$ kubectl get px -n demo restored-xtradb --watch
NAME              VERSION   STATUS         AGE
restored-xtradb   5.7       Provisioning   10m
restored-xtradb   5.7       Ready        13m
```

Now, find out the database Pod,

```bash
$ kubectl get pods -n demo --selector="app.kubernetes.io/instance=restored-xtradb" --watch
NAME                READY   STATUS    RESTARTS   AGE
restored-xtradb-0   1/1     Running   0          15m
```

And then copy the user name and password of the `root` user to access into `mysql` shell.

> Notice: We used the same Secret for the `restored-xtradb` object. So, we will use the same commands as before.

```bash
$ kubectl get secret -n demo  sample-xtradb-auth -o jsonpath='{.data.username}'| base64 -d
root⏎

$ kubectl get secret -n demo  sample-xtradb-auth -o jsonpath='{.data.password}'| base64 -d
5qtWP192NLD-nwgd⏎
```

Now, let's exec into the Pod to enter into `mysql` shell and check the database and the table we created before,

```bash
$ kubectl exec -it -n demo restored-xtradb-0 -- mysql --user=root --password=5qtWP192NLD-nwgd
mysql: [Warning] Using a password on the command line interface can be insecure.
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 103
Server version: 5.7.26-29 Percona Server (GPL), Release 29, Revision 11ad961

Copyright (c) 2009-2019 Percona LLC and/or its affiliates
Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| playground         |
| sys                |
+--------------------+
5 rows in set (0.00 sec)

mysql> SHOW TABLES IN playground;
+----------------------+
| Tables_in_playground |
+----------------------+
| equipment            |
+----------------------+
1 row in set (0.00 sec)

mysql> SELECT * FROM playground.equipment;
+----+-------+-------+-------+
| id | type  | quant | color |
+----+-------+-------+-------+
|  1 | slide |     2 | blue  |
+----+-------+-------+-------+
1 row in set (0.00 sec)

mysql> exit
Bye
```

So, from the above output, we can see that the `playground` database and the `equipment` table we created before in the original database are restored successfully.

## Cleanup

To cleanup the Kubernetes resources created by this tutorial, run:

```bash
kubectl delete restoresession -n demo restored-xtradb-restore
kubectl delete px -n demo restored-xtradb
kubectl delete repository -n demo gcs-repo-sample-xtradb
kubectl delete backupconfiguration -n demo sample-xtradb-backup
kubectl delete px -n demo sample-xtradb
```
