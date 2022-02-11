---
title: Helm managed NATS
description: Backup Helm managed NATS using Stash
menu:
  docs_v2022.02.22:
    identifier: stash-nats-helm
    name: Helm managed NATS
    parent: stash-nats
    weight: 20
product_name: stash
menu_name: docs_v2022.02.22
section_menu_id: stash-addons
info:
  cli: v0.18.0
  community: v0.18.0
  elasticsearch:
  - 5.6.4-v15
  - 6.2.4-v15
  - 6.3.0-v15
  - 6.4.0-v15
  - 6.5.3-v15
  - 6.8.0-v15
  - 7.14.0-v1
  - 7.2.0-v15
  - 7.3.2-v15
  enterprise: v0.18.0
  etcd:
  - 3.5.0-v2
  installer: v2022.02.22
  mariadb:
  - 10.5.8-v8
  mongodb:
  - 3.4.17-v14
  - 3.4.22-v14
  - 3.6.13-v14
  - 3.6.8-v14
  - 4.0.11-v14
  - 4.0.3-v14
  - 4.0.5-v14
  - 4.1.13-v14
  - 4.1.4-v14
  - 4.1.7-v14
  - 4.2.3-v14
  - 4.4.6-v5
  - 5.0.3-v2
  mysql:
  - 5.7.25-v15
  - 8.0.14-v15
  - 8.0.21-v9
  - 8.0.3-v15
  nats:
  - 2.6.1-v2
  percona-xtradb:
  - 5.7-v10
  postgres:
  - 10.14-v13
  - 11.9-v13
  - 12.4-v13
  - 13.1-v10
  - 14.0-v2
  - 9.6.19-v13
  redis:
  - 5.0.13-v3
  - 6.2.5-v3
  ui-server: v0.1.0
  version: v2022.02.22
---

# Backup Helm managed NATS using Stash

Stash `{{< param "info.version" >}}` supports backup and restoration of NATS streams. This guide will show you how you can backup & restore a Helm managed NATS server using Stash.

## Before You Begin

- At first, you need to have a Kubernetes cluster, and the `kubectl` command-line tool must be configured to communicate with your cluster.
- Install Stash Enterprise in your cluster following the steps [here](/docs/v2022.02.22/setup/install/enterprise).
- If you are not familiar with how Stash backup and restore NATS streams, please check the following guide [here](/docs/v2022.02.22/addons/nats/overview/).

You have to be familiar with following custom resources:

- [AppBinding](/docs/v2022.02.22/concepts/crds/appbinding)
- [Function](/docs/v2022.02.22/concepts/crds/function)
- [Task](/docs/v2022.02.22/concepts/crds/task)
- [BackupConfiguration](/docs/v2022.02.22/concepts/crds/backupconfiguration)
- [BackupSession](/docs/v2022.02.22/concepts/crds/backupsession)
- [RestoreSession](/docs/v2022.02.22/concepts/crds/restoresession)

To keep things isolated, we are going to use a separate namespace called `demo` throughout this tutorial. Create `demo` namespace if you haven't created already.

```bash
$ kubectl create ns demo
namespace/demo created
```

> Note: YAML files used in this tutorial are stored [here](https://github.com/stashed/docs/tree/{{< param "info.version" >}}/docs/addons/nats/helm/examples).

## Prepare NATS

In this section, we are going to deploy a NATS cluster. Then, we are going to insert some sample data into it.

### Deploy NATS Cluster

At first, let's deploy a NATS cluster. Here, we are going to use [NATS]( https://github.com/nats-io/k8s/tree/main/helm/charts/nats)  chart from [nats.io](https://nats.io/).

Let's deploy a nats cluster named `sample-nats` using Helm as below,

```bash
# Add nats chart registry
$ helm repo add nats https://nats-io.github.io/k8s/helm/charts/ 
# Update helm registries
$ helm repo update
# Install nats/nats chart into demo namespace
$ helm install sample-nats nats/nats -n demo \
--set nats.jetstream.enabled=true \
--set nats.jetstream.fileStorage.enabled=true \
--set cluster.enabled=true \
--set cluster.replicas=3 
```

This chart will create the necessary StatefulSet, Service, PVCs etc. for the NATS cluster. You can easily view all the resources created by chart using [ketall](https://github.com/corneliusweig/ketall) `kubectl` plugin as below,

```bash
❯ kubectl get-all -n demo -l app.kubernetes.io/instance=sample-nats
NAME                                                    NAMESPACE  AGE
configmap/sample-nats-config                            demo       11m  
endpoints/sample-nats                                   demo       11m  
persistentvolumeclaim/sample-nats-js-pvc-sample-nats-0  demo       11m  
persistentvolumeclaim/sample-nats-js-pvc-sample-nats-1  demo       10m  
persistentvolumeclaim/sample-nats-js-pvc-sample-nats-2  demo       10m  
pod/sample-nats-0                                       demo       11m  
pod/sample-nats-1                                       demo       10m  
pod/sample-nats-2                                       demo       10m  
service/sample-nats                                     demo       11m  
controllerrevision.apps/sample-nats-775468b94f          demo       11m  
statefulset.apps/sample-nats                            demo       11m  
endpointslice.discovery.k8s.io/sample-nats-7n7v6        demo       11m  
```

Now, wait for the NATS server pods `sample-nats-0`, `sample-nats-1`, `sample-nats-2` to go into `Running` state,

```bash
❯ kubectl get pod -n demo -l app.kubernetes.io/instance=sample-nats
NAME            READY   STATUS    RESTARTS   AGE
sample-nats-0   3/3     Running   0          9m58s
sample-nats-1   3/3     Running   0          9m35s
sample-nats-2   3/3     Running   0          9m12s
```

Once the nats server pods are in `Running` state, verify that the NATS server is ready to accept the connections.

```bash
❯ kubectl logs -n demo sample-nats-0 -c nats
[7] 2021/09/06 08:33:53.111508 [INF] Starting nats-server
[7] 2021/09/06 08:33:53.111560 [INF] Version:  2.6.1
...
[7] 2021/09/06 08:33:53.116004 [INF] Server is ready
```

From the above log, we can see the NATS server is ready to accept connections.

### Insert Sample Data

The above Helm chart also deploy a pod with nats-box image which can be used to interact with the NATS server. Let's verify the nats-box pod has been created.

```bash
❯ kubectl get pod -n demo -l app=sample-nats-box
NAME                               READY   STATUS    RESTARTS   AGE
sample-nats-box-785f8458d7-wtnfx   1/1     Running   0          7m20s
```

Now, let's exec into the nats-box pod and insert some sample data,

```
❯ kubectl exec -n demo -it sample-nats-box-785f8458d7-wtnfx -- sh -l
...
# Let's create a stream named "ORDERS"
sample-nats-box-785f8458d7-wtnfx:~# nats stream add ORDERS --subjects "ORDERS.*" --ack --max-msgs=-1 --max-bytes=-1 --max-age=1y --storage file --retention limits --max-msg-size=-1 --max-msgs-per-subject=-1 --discard old --dupe-window="0s" --replicas 1
Stream ORDERS was created

Information for Stream ORDERS created 2021-09-03T07:12:07Z

Configuration:

             Subjects: ORDERS.*
     Acknowledgements: true
            Retention: File - Limits
             Replicas: 1
       Discard Policy: Old
     Duplicate Window: 2m0s
     Maximum Messages: unlimited
        Maximum Bytes: unlimited
          Maximum Age: 1y0d0h0m0s
 Maximum Message Size: unlimited
    Maximum Consumers: unlimited


Cluster Information:

                 Name: nats
               Leader: nats-0

State:

             Messages: 0
                Bytes: 0 B
             FirstSeq: 0
              LastSeq: 0
     Active Consumers: 0


# Verify that the stream has been created successfully
sample-nats-box-785f8458d7-wtnfx:~#  nats stream ls
Streams:

	ORDERS
	
# Lets add some messages to the stream "ORDERS"
sample-nats-box-785f8458d7-wtnfx:~# nats pub ORDERS.scratch hello
08:55:39 Published 5 bytes to "ORDERS.scratch"

# Add another message
sample-nats-box-785f8458d7-wtnfx:~# nats pub ORDERS.scratch world
08:56:11 Published 5 bytes to "ORDERS.scratch"

# Verify that the messages have been published to the stream successfully
sample-nats-box-785f8458d7-wtnfx:~# nats stream info ORDERS
Information for Stream ORDERS created 2021-09-03T07:12:07Z

Configuration:

             Subjects: ORDERS.*
     Acknowledgements: true
            Retention: File - Limits
             Replicas: 1
       Discard Policy: Old
     Duplicate Window: 2m0s
     Maximum Messages: unlimited
        Maximum Bytes: unlimited
          Maximum Age: 1y0d0h0m0s
 Maximum Message Size: unlimited
    Maximum Consumers: unlimited


Cluster Information:

                 Name: nats
               Leader: nats-0

State:

             Messages: 2
                Bytes: 98 B
             FirstSeq: 1 @ 2021-09-03T08:55:39 UTC
              LastSeq: 2 @ 2021-09-03T08:56:11 UTC
     Active Consumers: 0

sample-nats-box-785f8458d7-wtnfx:~# exit
```

We have successfully deployed a NATS cluster, created a stream and publish some messages into the stream. In the subsequent sections, we are going to backup this sample data using Stash.

## Prepare for Backup

In this section, we are going to prepare the necessary resources (i.e. connection information, backend information, etc.) before backup.

### Ensure NATS Addon

When you install Stash Enterprise version, it will automatically install all the official addons. Make sure that NATS addon has been installed properly using the following command.

```bash
❯ kubectl get tasks.stash.appscode.com | grep nats
nats-backup-2.6.1            24m
nats-restore-2.6.1           24m
```

This addon should be able to take backup of the NATS streams with matching major versions as discussed in [Addon Version Compatibility](/docs/v2022.02.22/addons/nats/README#addon-version-compatibility).

### Create AppBinding

Stash needs to know how to connect with the NATS server. An `AppBinding` exactly provides this information. It holds the Service and Secret information of the NATS server. You have to point to the respective `AppBinding` as a target of backup instead of the NATS server itself.

Here, is the YAML of the `AppBinding` that we are going to create for the NATS server we have deployed earlier.

```yaml
apiVersion: appcatalog.appscode.com/v1alpha1
kind: AppBinding
metadata:
  labels:
    app.kubernetes.io/instance: sample-nats
  name: sample-nats
  namespace: demo
spec:
  clientConfig:
    service:
      name: sample-nats
      port: 4222
      scheme: nats
  type: nats.io/nats
  version: 2.6.1
```

Here,

- **.spec.clientConfig.service** specifies the Service information to use to connects with the NATS server.
- `spec.type` specifies the type of the target. This is particularly helpful in auto-backup where you want to use different path prefixes for different types of target.

Let's create the `AppBinding` we have shown above,

```bash
$ kubectl apply -f https://github.com/stashed/docs/tree/{{< param "info.version" >}}/docs/addons/nats/helm/examples/appbinding.yaml
appbinding.appcatalog.appscode.com/sample-nats created
```

### Prepare Backend

We are going to store our backed up data into a GCS bucket. So, we need to create a Secret with GCS credentials and a `Repository` object with the bucket information. If you want to use a different backend, please read the respective backend configuration doc from [here](/docs/v2022.02.22/guides/backends/overview).

**Create Storage Secret:**

At first, let's create a secret called `gcs-secret` with access credentials to our desired GCS bucket,

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

**Create Repository:**

Now, create a `Repository` object with the information of your desired bucket. Below is the YAML of `Repository` object we are going to create,

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
      prefix: /demo/nats/sample-nats
    storageSecretName: gcs-secret
```

Let's create the `Repository` we have shown above,

```bash
$ kubectl create -f https://github.com/stashed/docs/raw/{{< param "info.version" >}}/docs/addons/nats/helm/examples/repository.yaml
repository.stash.appscode.com/gcs-repo created
```

Now, we are ready to backup our streams into our desired backend.

### Backup

To schedule a backup, we have to create a `BackupConfiguration` object targeting the respective `AppBinding` of our NATS server. Then, Stash will create a CronJob to periodically backup the streams.

#### Create BackupConfiguration

Below is the YAML for `BackupConfiguration` object that we are going to use to backup the streams of the NATS server we have created earlier,

```yaml
apiVersion: stash.appscode.com/v1beta1
kind: BackupConfiguration
metadata:
  name: sample-nats-backup
  namespace: demo
spec:
  task:
    name: nats-backup-2.6.1
  schedule: "*/5 * * * *"
  repository:
    name: gcs-repo
  target:
    ref:
      apiVersion: appcatalog.appscode.com/v1alpha1
      kind: AppBinding
      name: sample-nats
  interimVolumeTemplate:
    metadata:
      name: sample-nats-backup-tmp-storage
    spec:
      accessModes: [ "ReadWriteOnce" ]
      storageClassName: "standard"
      resources:
        requests:
          storage: 1Gi  
  retentionPolicy:
    name: keep-last-5
    keepLast: 5
    prune: true
```

Here,

- `.spec.schedule` specifies that we want to backup the streams at 5 minutes intervals.
- `.spec.task.name` specifies the name of the Task object that specifies the necessary Functions and their execution order to backup NATS streams.
- `.spec.repository.name` specifies the Repository CR name we have created earlier with backend information.
- `.spec.target.ref` refers to the AppBinding object that holds the connection information of our targeted NATS server.
- `spec.interimVolumeTemplate` specifies a PVC template that will be used by Stash to hold the dumped data temporarily before uploading it into the cloud bucket.
- `.spec.retentionPolicy` specifies a policy indicating how we want to cleanup the old backups.

Let's create the `BackupConfiguration` object we have shown above,

```bash
$ kubectl create -f https://github.com/stashed/docs/raw/{{< param "info.version" >}}/docs/addons/nats/helm/examples/backupconfiguration.yaml
backupconfiguration.stash.appscode.com/sample-nats-backup created
```

#### Verify CronJob

If everything goes well, Stash will create a CronJob with the schedule specified in `spec.schedule` field of `BackupConfiguration` object.

Verify that the CronJob has been created using the following command,

```bash
❯ kubectl get cronjob -n demo
NAME                               SCHEDULE      SUSPEND   ACTIVE   LAST SCHEDULE   AGE
stash-backup-sample-nats-backup    */5 * * * *   False     0        <none>          14s
```

#### Wait for BackupSession

The `sample-nats-backup` CronJob will trigger a backup on each scheduled slot by creating a `BackupSession` object.

Now, wait for a schedule to appear. Run the following command to watch for `BackupSession` object,

```bash
❯ kubectl get backupsession -n demo -w
NAME                       INVOKER-TYPE          INVOKER-NAME         PHASE       DURATION   AGE
sample-nats-backup-8x8fp   BackupConfiguration   sample-nats-backup   Succeeded   42s        8m28s
```

Here, the phase `Succeeded` means that the backup process has been completed successfully.

#### Verify Backup

Now, we are going to verify whether the backed up data is present in the backend or not. Once a backup is completed, Stash will update the respective `Repository` object to reflect the backup completion. Check that the repository `gcs-repo` has been updated by the following command,

```bash
❯ kubectl get repository -n demo
NAME       INTEGRITY   SIZE        SNAPSHOT-COUNT   LAST-SUCCESSFUL-BACKUP   AGE
gcs-repo   true        1.382 KiB   1                9m4s                     24m
```

Now, if we navigate to the GCS bucket, we will see the backed up data has been stored in `demo/nats/sample-nats` directory as specified by `.spec.backend.gcs.prefix` field of the `Repository` object.

<figure align="center">
  <img alt="Backup data in GCS Bucket" src="/docs/v2022.02.22/addons/nats/helm/images/sample-nats-backup.png">
  <figcaption align="center">Fig: Backup data in GCS Bucket</figcaption>
</figure>


> Note: Stash keeps all the backed up data encrypted. So, data in the backend will not make any sense until they are decrypted.

## Restore

If you have followed the previous sections properly, you should have a successful backup of your NATS streams. Now, we are going to show how you can restore the streams from the backed up data.

### Restore Into the Same NATS Cluster

You can restore your data into the same NATS cluster you have backed up from or into a different NATS cluster in the same cluster or a different cluster. In this section, we are going to show you how to restore in the same NATS cluster which may be necessary when you have accidentally lost any data.

#### Temporarily Pause Backup

At first, let's stop taking any further backup of the NATS streams so that no backup runs after we delete the sample data. We are going to pause the `BackupConfiguration` object. Stash will stop taking any further backup when the `BackupConfiguration` is paused.

Let's pause the `sample-nats-backup` BackupConfiguration,

```bash
$ kubectl patch backupconfiguration -n demo sample-nats-backup --type="merge" --patch='{"spec": {"paused": true}}'
backupconfiguration.stash.appscode.com/sample-nats-backup patched
```

Verify that the `BackupConfiguration` has been paused,

```bash
❯ kubectl get backupconfiguration -n demo sample-nats-backup
NAME                 TASK                SCHEDULE      PAUSED   AGE
sample-nats-backup   nats-backup-2.6.1   */5 * * * *   true     2d18h
```

Notice the `PAUSED` column. Value `true` for this field means that the `BackupConfiguration` has been paused.

Stash will also suspend the respective CronJob.

```bash
❯ kubectl get cronjob -n demo
NAME                              SCHEDULE      SUSPEND   ACTIVE   LAST SCHEDULE   AGE
stash-backup-sample-nats-backup   */5 * * * *   True      0        56s             2d18h
```

#### Simulate Disaster

Now, let's simulate a disaster scenario. Here, we are going to exec into the nats-box pod and delete the sample data we have inserted earlier.

```bash
❯ kubectl exec -n demo -it sample-nats-box-785f8458d7-wtnfx -- sh -l
...
# delete the stream "ORDERS"
sample-nats-box-785f8458d7-wtnfx:~# nats stream rm ORDERS -f

# verify that the stream has been deleted
sample-nats-box-785f8458d7-wtnfx:~# nats stream ls
No Streams defined

sample-nats-box-785f8458d7-wtnfx:~# exit
```

#### Create RestoreSession

To restore the streams, you have to create a `RestoreSession` object pointing to the `AppBinding` of the targeted NATS server.

Here, is the YAML of the `RestoreSession` object that we are going to use for restoring the streams of the NATS server.

```yaml
apiVersion: stash.appscode.com/v1beta1
kind: RestoreSession
metadata:
  name: sample-nats-restore
  namespace: demo
spec:
  task:
    name: nats-restore-2.6.1
  repository:
    name: gcs-repo
  target:
    ref:
      apiVersion: appcatalog.appscode.com/v1alpha1
      kind: AppBinding
      name: sample-nats
  interimVolumeTemplate:
    metadata:
      name: nats-restore-tmp-storage
    spec:
      accessModes: [ "ReadWriteOnce" ]
      storageClassName: "standard"
      resources:
        requests:
          storage: 1Gi
  rules:
  - snapshots: [latest]
```

Here,

- `.spec.task.name` specifies the name of the Task object that specifies the necessary Functions and their execution order to restore NATS streams.
- `.spec.repository.name` specifies the Repository object that holds the backend information where our backed up data has been stored.
- `.spec.target.ref` refers to the respective AppBinding of the `sample-nats` cluster.
- `.spec.interimVolumeTemplate` specifies a PVC template that will be used by Stash to hold the restored data temporarily before injecting into the NATS server.
- `.spec.rules` specifies that we are restoring data from the latest backup snapshot of the streams.

Let's create the `RestoreSession` object object we have shown above,

```bash
$ kubectl apply -f https://github.com/stashed/docs/raw/{{< param "info.version" >}}/docs/addons/nats/helm/examples/restoresession.yaml
restoresession.stash.appscode.com/sample-nats-restore created
```

Once, you have created the `RestoreSession` object, Stash will create a restore Job. Run the following command to watch the phase of the `RestoreSession` object,

```bash
❯ kubectl get restoresession -n demo -w
NAME                  REPOSITORY   PHASE       DURATION   AGE
sample-nats-restore   gcs-repo     Succeeded   15s        55s
```

The `Succeeded` phase means that the restore process has been completed successfully.

#### Verify Restored Data

Now, let's exec into the nats-box pod and verify whether data actual data has been restored or not,

```bash
❯ kubectl exec -n demo -it sample-nats-box-785f8458d7-wtnfx -- sh -l
...
# Verify that the stream has been restored successfully
sample-nats-box-785f8458d7-wtnfx:~#  nats stream ls
Streams:

	ORDERS

# Verify that the messages have been restored successfully
sample-nats-box-785f8458d7-wtnfx:~# nats stream info ORDERS
Information for Stream ORDERS created 2021-09-03T07:12:07Z

Configuration:

             Subjects: ORDERS.*
     Acknowledgements: true
            Retention: File - Limits
             Replicas: 1
       Discard Policy: Old
     Duplicate Window: 2m0s
     Maximum Messages: unlimited
        Maximum Bytes: unlimited
          Maximum Age: 1y0d0h0m0s
 Maximum Message Size: unlimited
    Maximum Consumers: unlimited


Cluster Information:

                 Name: nats
               Leader: nats-0

State:

             Messages: 2
                Bytes: 98 B
             FirstSeq: 1 @ 2021-09-03T08:55:39 UTC
              LastSeq: 2 @ 2021-09-03T08:56:11 UTC
     Active Consumers: 0

sample-nats-box-785f8458d7-wtnfx:~# exit
```

Hence, we can see from the above output that the deleted data has been restored successfully from the backup.

#### Resume Backup

Since our data has been restored successfully we can now resume our usual backup process. Resume the `BackupConfiguration` using following command,

```bash
❯ kubectl patch backupconfiguration -n demo sample-nats-backup --type="merge" --patch='{"spec": {"paused": false}}'
backupconfiguration.stash.appscode.com/sample-nats-backup patched
```

Verify that the `BackupConfiguration` has been resumed,
```bash
❯ kubectl get backupconfiguration -n demo sample-nats-backup
NAME                 TASK                SCHEDULE      PAUSED   AGE
sample-nats-backup   nats-backup-2.6.1   */5 * * * *   false    2d19h
```

Here,  `false` in the `PAUSED` column means the backup has been resumed successfully. The CronJob also should be resumed now.

```bash
❯ kubectl get cronjob -n demo
NAME                               SCHEDULE      SUSPEND   ACTIVE   LAST SCHEDULE   AGE
stash-backup-sample-nats-backup    */5 * * * *   False     0        3m24s           4h54m
```

Here, `False` in the `SUSPEND` column means the CronJob is no longer suspended and will trigger in the next schedule.

### Restore Into Different NATS Cluster of the Same Namespace

If you want to restore the backed up data into a different NATS cluster of the same namespace, you have to create another `AppBinding` pointing to the desired NATS cluster. Then, you have to create the `RestoreSession` pointing to the new `AppBinding`.

### Restore Into Different Namespace

If you want to restore into a different namespace of the same cluster, you have to create the Repository, backend Secret, AppBinding, in the desired namespace. You can use [Stash kubectl plugin](https://stash.run/docs/{{< param "info.version" >}}/guides/cli/cli/) to easily copy the resources into a new namespace. Then, you have to create the `RestoreSession` object in the desired namespace pointing to the Repository, AppBinding of that namespace.

### Restore Into Different Cluster

If you want to restore into a different cluster, you have to install Stash in the desired cluster. Then, you have to create the Repository, backend Secret, AppBinding, in the desired cluster. Finally, you have to create the `RestoreSession` object in the desired cluster pointing to the Repository, AppBinding of that cluster.

## Cleanup

To cleanup the Kubernetes resources created by this tutorial, run:

```bash
kubectl delete -n demo backupconfiguration sample-nats-backup
kubectl delete -n demo restoresession sample-nats-restore
kubectl delete -n demo repository gcs-repo
# delete the nats chart
helm delete sample-nats -n demo
```