---
title: Dedicated Backup Namespace | Stash
description: A guide on how to manage backup and restore from a dedicated namespace for targets of different namespaces using Stash.
menu:
  docs_v2025.6.30:
    identifier: managed-backup-dedicated-backup-namespace
    name: Dedicated Backup Namespace
    parent: managed-backup
    weight: 10
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

# Manage Backup and Restore from a Dedicated Namespace

This guide will show you how you can use a dedicated backup namespace to keep your backup resources isolated from your workloads.

## Before You Begin

- At first, you need to have a Kubernetes cluster, and the `kubectl` command-line tool must be configured to communicate with your cluster. If you do not already have a cluster, you can create one by using [kind](https://kind.sigs.k8s.io/docs/user/quick-start/).

- Install `Stash` in your cluster following the steps [here](/docs/v2025.6.30/setup/README).

- You should be familiar with the following `Stash` concepts:
  - [BackupConfiguration](/docs/v2025.6.30/concepts/crds/backupconfiguration/)
  - [BackupSession](/docs/v2025.6.30/concepts/crds/backupsession/)
  - [RestoreSession](/docs/v2025.6.30/concepts/crds/restoresession/)
  - [Repository](/docs/v2025.6.30/concepts/crds/repository/)

Here, we are going to take a backup from the `prod` namespace and restore it to the `staging` namespace. We are going to manage the backup and restore from a separate `backup` namespace.

Let's create the above-mentioned namespaces,

```bash
$ kubectl create ns prod
namespace/prod created

$ kubectl create ns backup
namespace/backup created

$ kubectl create ns staging
namespace/staging created
```

>**Note:** YAML files used in this tutorial can be found [here](https://github.com/stashed/docs/guides/managed-backup/dedicated-backup-namespace/examples).

## Backup 

In this section, we are going to backup a MySQL database from the `prod` namespace. We are going to use `backup` namespace for our backup resources.

### Deploy Sample MySQL Database

We are going to use KubeDB for deploying a sample MySQL Database. Let's deploy the following sample MySQL database in `prod` namespace,

```yaml
apiVersion: kubedb.com/v1alpha2
kind: MySQL
metadata:
  name: sample-mysql
  namespace: prod
spec:
  version: "8.0.27"
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

To create the above `MySQL` object,

```bash
$ kubectl apply -f https://github.com/stashed/docs/raw/{{< param "info.version" >}}/docs/guides/managed-backup/dedicated-backup-namespace/examples/mysql.yaml
mysql.kubedb.com/sample-mysql created
```

KubeDB will deploy a MySQL database according to the above specification. It will also create the necessary Secrets and Services to access the database.

Let's check if the database is ready to use,

```bash
$ kubectl get my -n prod sample-mysql
NAME           VERSION   STATUS   AGE
sample-mysql   8.0.27    Ready    3m32s
```
We can see that the database is `Ready`. 

**Verify AppBinding:**

Verify that the AppBinding has been created successfully using the following command,

```bash
$ kubectl get appbindings -n prod
NAME           TYPE               VERSION   AGE
sample-mysql   kubedb.com/mysql   8.0.27    11m
```
Stash uses the AppBinding CRD to connect with the target database. 

> If you are not using KubeDB to deploy the database, create the AppBinding manually.

**Insert Sample Data:**

Now, we are going to exec into the database pod and create some sample data. At first, find out the database Pod using the following command,

```bash
$ kubectl get pods -n prod --selector="app.kubernetes.io/instance=sample-mysql"
NAME             READY   STATUS    RESTARTS   AGE
sample-mysql-0   1/1     Running   0          33m
```

And copy the user name and password of the `root` user to access the `mysql` shell.

```bash
$ kubectl get secret -n prod  sample-mysql-auth -o jsonpath='{.data.username}'| base64 -d
root⏎

$ kubectl get secret -n prod  sample-mysql-auth -o jsonpath='{.data.password}'| base64 -d
vTSh3ZQxDBRm7dzl⏎
```

Now, let's exec into the Pod to enter into `mysql` shell and create a database and a table,

```bash
$ kubectl exec -it -n prod sample-mysql-0 -- mysql --user=root --password="vTSh3ZQxDBRm7dzl"
mysql: [Warning] Using a password on the command line interface can be insecure.
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 10
Server version: 8.0.14 MySQL Community Server - GPL

Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> CREATE DATABASE playground;
Query OK, 1 row affected (0.01 sec)

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
Query OK, 0 rows affected (0.01 sec)

mysql> SHOW TABLES IN playground;
+----------------------+
| Tables_in_playground |
+----------------------+
| equipment            |
+----------------------+
1 row in set (0.01 sec)

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

Now, we are ready to backup the database.

### Prepare Backend

We are going to store our backed-up data into a GCS bucket. We have to create a Secret with the necessary credentials and a Repository CRD to use this backend. 

If you want to use a different backend, please read the doc [here](/docs/v2025.6.30/guides/backends/overview/).

> For the GCS backend, if the bucket does not exist, Stash needs `Storage Object Admin` role permissions to create the bucket. For more details, please check the following [guide](/docs/v2025.6.30/guides/backends/gcs/).

**Create Secret:**

Let's create a Secret called `gcs-secret` in `backup` namespace with access credentials to our desired GCS bucket,

```bash
$ echo -n 'changeit' > RESTIC_PASSWORD
$ echo -n '<your-project-id>' > GOOGLE_PROJECT_ID
$ cat /path/to/downloaded-sa-key.json > GOOGLE_SERVICE_ACCOUNT_JSON_KEY
$ kubectl create secret generic -n backup gcs-secret \
    --from-file=./RESTIC_PASSWORD \
    --from-file=./GOOGLE_PROJECT_ID \
    --from-file=./GOOGLE_SERVICE_ACCOUNT_JSON_KEY
secret/gcs-secret created
```

**Create Repository:**

Now, create a Repository using this Secret. Below is the YAML of Repository object we are going to create, 

```yaml
apiVersion: stash.appscode.com/v1alpha1
kind: Repository
metadata:
  name: gcs-repo
  namespace: backup
spec:
  backend:
    gcs:
      bucket: stash-testing
      prefix: /cross-namespace-target/data/sample-mysql
    storageSecretName: gcs-secret
```

Let's create the Repository we have shown above,

```bash
$ kubectl apply -f https://github.com/stashed/docs/raw/{{< param "info.version" >}}/docs/guides/managed-backup/dedicated-backup-namespace/examples/repository.yaml
repository.stash.appscode.com/gcs-repo created
```
Now, we are ready to backup our sample data into this backend.

### Configure Backup

We are going to create a `BackupConfiguration` object in the `backup` namespace targetting the `sample-mysql` database of the `prod` namespace. Stash does not grant necessary RBAC permissions to the backup job for taking backup from a different namespace. In this case, we have to provide the RBAC permissions manually. This helps to prevent unauthorized namespaces from getting access to a database via Stash.

**Create ServiceAccount:**

At first, we are going to create a ServiceAccount in the `backup` namespace. We will grant necessary RBAC permissions to this ServiceAccount and use it in the backup job.

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: cross-namespace-target-reader
  namespace: backup
```

Let's create the ServiceAccount,

```bash
$ kubectl apply -f https://github.com/stashed/docs/raw/{{< param "info.version" >}}/docs/guides/managed-backup/dedicated-backup-namespace/examples/serviceaccount.yaml
serviceaccount/cross-namespace-target-reader created
```

**Create ClusterRole and ClusterRoleBinding**

We are going to create a ClusterRole and ClusterRoleBinding with the necessary permissions to perform the backup. Here are the YAMLs of the ClusterRole and ClusterRoleBinding,

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: cross-namespace-target-reader
rules:
- apiGroups: [""]
  resources: ["secrets","pods","endpoints"]
  verbs: ["get","list"]
- apiGroups: ["appcatalog.appscode.com"]
  resources: ["appbindings"]
  verbs: ["get","list"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: cross-namespace-target-reader
subjects:
- kind: ServiceAccount
  name: cross-namespace-target-reader
  namespace: backup
roleRef:
  kind: ClusterRole
  name: cross-namespace-target-reader
  apiGroup: rbac.authorization.k8s.io
```

Let's create the ClusterRole and ClusterRoleBinding we have shown above,

```bash
$ kubectl apply -f https://github.com/stashed/docs/raw/{{< param "info.version" >}}/docs/guides/managed-backup/dedicated-backup-namespace/examples/clusterrole_clusterrolebinding.yaml
clusterrole.rbac.authorization.k8s.io/cross-namespace-target-reader created
clusterrolebinding.rbac.authorization.k8s.io/cross-namespace-target-reader created
```

The above RBAC permissions will allow the ServiceAccounts to perform backup targets from any namespace.

Alternatively, you can create Role and RoleBinding with the same permissions in case you want to restrict the ServiceAccounts to backup targets from only a specific namespace.

**Create BackupConfiguration:**

Now, we are going to create the BackupConfiguration to backup our MySQL database of `prod` namespace. Below is the YAML of the `BackupConfiguration` object,

```yaml
apiVersion: stash.appscode.com/v1beta1
kind: BackupConfiguration
metadata:
  name: sample-mysql-backup
  namespace: backup
spec:
  schedule: "*/5 * * * *"
  repository:
    name: gcs-repo
  target:
    ref:
      apiVersion: appcatalog.appscode.com/v1alpha1
      kind: AppBinding
      name: sample-mysql
      namespace: prod
  runtimeSettings:
    pod:
      serviceAccountName: cross-namespace-target-reader
  retentionPolicy:
    name: keep-last-5
    keepLast: 5
    prune: true
```
Note that, we have mentioned the ServiceAccount name we have created earlier in the `spec.runtimeSettings.pod.serviceAccountName` field of the BackupConfiguration object.

Let's create the `BackupConfiguration` object we have shown above,

```bash
$ kubectl apply -f https://github.com/stashed/docs/raw/{{< param "info.version" >}}/docs/guides/managed-backup/dedicated-backup-namespace/examples/backupconfiguration.yaml
backupconfiguration.stash.appscode.com/sample-mysql-backup
```

**Verify BackupConfiguration Ready:**

If everything goes well, the phase of the BackupConfiguration should be `Ready`. Let's check the BackupConfiguration Phase,

```bash
❯ kubectl get backupconfiguration -n backup
NAME                   TASK   SCHEDULE      PAUSED   PHASE   AGE
sample-mysql-backup           */5 * * * *            Ready   13s
```

### Verify Backup

The `sample-mysql-backup` BackupConfiguration will create a CronJob in the `dev` namespace and that will trigger a backup on each scheduled slot by creating a `BackupSession` object.

Wait for the next schedule for the backup. Run the following command to watch the `BackupSession` object,

```bash
$ kubectl get backupsession -n backup -w

NAME                             INVOKER-TYPE          INVOKER-NAME          PHASE       DURATION   AGE
sample-mysql-backup-1650452100   BackupConfiguration   sample-mysql-backup   Running                0s
sample-mysql-backup-1650452100   BackupConfiguration   sample-mysql-backup   Running                16s
sample-mysql-backup-1650452100   BackupConfiguration   sample-mysql-backup   Running                32s
sample-mysql-backup-1650452100   BackupConfiguration   sample-mysql-backup   Succeeded   33s        32s
```

We can see from the above that the Phase of the  BackupSession is Succeeded. It indicates that Stash has successfully taken a backup of our target.

## Restore

In this section, we are going to restore the database into the `staging` namespace from the backup we have taken in the previous section.

**Stop Taking Backup of the Old Database:**

At first, let's stop taking any further backup of the old database so that no backup is taken during the restore process.

Let's pause the `sample-mysql-backup` BackupConfiguration,

```bash
$ kubectl patch backupconfiguration -n backup sample-mysql-backup --type="merge" --patch='{"spec": {"paused": true}}'
backupconfiguration.stash.appscode.com/sample-mysql-backup patched
```

Verify that the BackupConfiguration  has been paused,

```bash
$ kubectl get backupconfiguration -n backup sample-mysql-backup
NAME                 TASK                  SCHEDULE      PAUSED   PHASE   AGE
sample-mysql-backup  mysql-backup-8.0.21   */5 * * * *   true     Ready   26m
```

Notice the `PAUSED` column. Value `true` for this field means that the BackupConfiguration has been paused.

### Deploy Recovery MySQL Database

Now, we are going to deploy a new MySQL database in the `staging` namespace.

Below is the YAML for the MySQL database,

```yaml
apiVersion: kubedb.com/v1alpha2
kind: MySQL
metadata:
  name: mysql-recovery
  namespace: staging
spec:
  version: "8.0.27"
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

Let's create the above database,

```bash
$ kubectl apply -f https://github.com/stashed/docs/raw/{{< param "info.version" >}}/docs/guides/managed-backup/dedicated-backup-namespace/examples/mysql_recovery.yaml
mysql.kubedb.com/mysql-recovery created
```

Let's check the database status,

```bash
$ kubectl get my -n staging mysql-recovery
NAME             VERSION   STATUS   AGE
mysql-recovery   8.0.27    Ready    36s
```

**Verify AppBinding:**

Check that the AppBinding object has been created for the `mysql-recovery` database,

```bash
$ kubectl get appbindings -n staging
NAME             TYPE               VERSION   AGE
mysql-recovery   kubedb.com/mysql   8.0.27    2m
```

> If you are not using KubeDB to deploy database, create the AppBinding manually.

### Configure Restore

We are going to create a `RestoreSession` object in the `backup` namespace targeting the `mysql-recovery` database of `staging` namespace. Similar to BackupConfiguration, we need to grant the necessary RBAC permissions through a ServiceAccount to the RestoreSession as well.

**Create RestoreSession:**

Here is the YAML of the RestoreSession,

```yaml
apiVersion: stash.appscode.com/v1beta1
kind: RestoreSession
metadata:
  name: sample-mysql-restore
  namespace: backup
spec:
  task:
    name: mysql-restore-8.0.21
  repository:
    name: gcs-repo
  target:
    ref:
      apiVersion: appcatalog.appscode.com/v1alpha1
      kind: AppBinding
      name: mysql-recovery
      namespace: staging
  runtimeSettings:
    pod:
      serviceAccountName: cross-namespace-target-reader
  rules:
    - snapshots: [latest]
```

Note that, similarly to the BackupConfiguration we have mentioned the ServiceAccount here in the `spec.runtimeSettings.pod.serviceAccountName` field to grant necessary RBAC permissions to the RestoreSession.

Let's create the RestoreSession object,

```bash
$ kubectl apply -f https://github.com/stashed/docs/raw/{{< param "info.version" >}}/docs/guides/managed-backup/dedicated-backup-namespace/examples/restoresession.yaml
restoresession.stash.appscode.com/sample-mysql-restore created
```

Let's run the following command to watch the phase of the RestoreSession object,

```bash
$ kubectl get restoresession -n backup sample-mysql-restore -w

NAME                   REPOSITORY   PHASE     DURATION   AGE
sample-mysql-restore   gcs-repo     Running              2s
sample-mysql-restore   gcs-repo     Running              20s
sample-mysql-restore   gcs-repo     Succeeded   20s        20s
```

Here, we can see that the restore process has succeeded.

**Verify Restored Data:**

In this section, we are going to verify whether the desired data has been restored successfully.

Let's find out the database Pod by the following command,

```bash
$ kubectl get pods -n staging --selector="app.kubernetes.io/instance=mysql-recovery"
NAME               READY   STATUS    RESTARTS    AGE
mysql-recovery-0   1/1     Running   0           39m
```

Copy the username and password of the `root` user to access into `mysql` shell.

```bash
$ kubectl get secret -n staging  mysql-recovery-auth -o jsonpath='{.data.username}'| base64 -d
root⏎

$ kubectl get secret -n staging  mysql-recovery-auth -o jsonpath='{.data.password}'| base64 -d
XLy)x86brw)oVy0N⏎
```

Now, let's exec into the Pod to enter into `mysql` shell and create a database and a table,

```bash
$ kubectl exec -it -n staging mysql-recovery-0 -- mysql --user=root --password="XLy)x86brw)oVy0N"
mysql: [Warning] Using a password on the command line interface can be insecure.
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 9
Server version: 8.0.14 MySQL Community Server - GPL

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

So, from the above output, we can see that the `playground` database and the `equipment` table we created earlier are restored in the `mysql-recovery` database successfully.

## Cleanup

To cleanup the Kubernetes resources created by this tutorial, run:

```bash
$ kubectl delete backupconfiguration -n backup sample-mysql-backup
backupconfiguration.stash.appscode.com "sample-mysql-backup" deleted
$ kubectl delete restoresession -n backup sample-mysql-restore
restoresession.stash.appscode.com "sample-mysql-restore" deleted
$ kubectl delete repository -n backup gcs-repo
repository.stash.appscode.com "gcs-repo" deleted
$ kubectl delete secret -n backup gcs-secret
secret "gcs-secret" deleted
$ kubectl delete sa -n backup cross-namespace-target-reader
serviceaccount "cross-namespace-target-reader" deleted
$ kubectl delete clusterrole cross-namespace-target-reader
role.rbac.authorization.k8s.io "cross-namespace-target-reader" deleted
$ kubectl delete clusterrolebinding cross-namespace-target-reader
rolebinding.rbac.authorization.k8s.io "cross-namespace-target-reader" deleted
$ kubectl delete my -n staging mysql-recovery
mysql.kubedb.com "mysql-recovery" deleted
$ kubectl delete my -n prod sample-mysql
mysql.kubedb.com "sample-mysql" deleted
```
