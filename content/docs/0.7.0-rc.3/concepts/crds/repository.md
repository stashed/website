---
title: Repository Overview
menu:
  docs_0.7.0-rc.3:
    identifier: repository-overview
    name: Repository
    parent: crds
    weight: 15
product_name: stash
menu_name: docs_0.7.0-rc.3
section_menu_id: concepts
info:
  version: 0.7.0-rc.3
---

> New to Stash? Please start [here](/docs/0.7.0-rc.3/concepts/README).

# Repository

## What is Repository
A `Repository` is a Kubernetes `CustomResourceDefinition (CRD)`. It provides information of a [restic](https://restic.net/) repository in Kubernetes native way. When [stash](/docs/0.7.0-rc.3/concepts/what-is-stash/overview) sidecar create a restic repository for backup in desired backend, it also create a `Repository` CRD object with relevant information of the repository. This enable user to view backup status of the workloads very easily.

## Repository CRD structure
A sample `Repository` CRD object for backup a `Deployment` in local backend is shown below,

```yaml
apiVersion: stash.appscode.com/v1alpha1
kind: Repository
metadata:
  clusterName: ""
  creationTimestamp: 2018-03-29T05:39:04Z
  generation: 0
  labels:
    restic: stash-demo
    workload-kind: Deployment
    workload-name: stash-demo
  name: deployment.stash-demo
  namespace: default
  resourceVersion: "10389"
  selfLink: /apis/stash.appscode.com/v1alpha1/namespaces/default/repositories/deployment.stash-demo
  uid: 7db206b0-3313-11e8-ad40-0800277f165c
spec:
  backend:
    local:
      hostPath:
        path: /data/stash-test/restic-repo
      mountPath: /safe/data
    storageSecretName: local-secret
  backupPath: deployment/stash-demo
status:
  backupCount: 3
  firstBackupTime: 2018-03-29T05:40:05Z
  lastBackupDuration: 2.724088654s
  lastBackupTime: 2018-03-29T05:42:04Z
```

Here, we are going describe some important sections of `Repository` CRD.

## Repository Labels

`Repository` maintain some important information in label. These labels enable user to filter `Repository` according to `restic`, `workload-kind`, `workload-name`, `node-name` etc. Details of these labels are given below.

| Label name      | Description                                                                                                                                                   |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `restic`        | Name of the `Restic` which is responsible for this `Repository`.                                                                                              |
| `workload-kind` | `Kind` of the workload for which the `Repository` has been created.                                                                                           |
| `workload-name` | `Name` of the workload for which the `Repository` has been created.                                                                                           |
| `pod-name`      | This `label` present when the respective workload is  `StatefulSet`. It represent the pod name of the `StatefulSet` who is responsible for this `Repository`. |
| `node-name`     | This `label` present when the respective workload is  `DaemonSet`. It represent the node name where the  `DaemonSet` is running.                              |

## Repository Spec

`Repository` CRD needs the following information in `.spec` section.

### spec.backend

`spec.backend` holds the backend information where the backup snapshots are being stored. To learn how to configure various backends for Restic, please visit [here](/docs/0.7.0-rc.3/guides/backends).

### backend prefix/subPath

`prefix` of any backend denotes the directory inside the backend where the snapshots are being stored. If the backend is a **Local** backend then `subPath` is used for this purpose.

## Repository Status

Stash operator updates `.status` of a Repository CRD every time a backup operation is completed.

- `status.backupCount` indicated the total number of backup operation completed for this Repository.
- `status.firstBackupTime` indicates the timestamp of first backup operation.
- `status.lastBackupTime` indicates the timestamp of last backup operation.
- `status.lastSuccessfulBackupTime` indicates the timestamp of last successful backup operation. If `status.lastBackupTime` and `status.lastSuccessfulBackupTime` are same, it means that last backup operation was successful.
- `status.lastBackupDuration` indicates the duration of last backup operation.

## Creation of Repository CRD

Whenever a `restic` repository is created according to [these](/docs/0.7.0-rc.3/concepts/crds/restic#backup-repository-structure) rules, it also create respective `Repository` CRD object. Name of this `Repository` CRD object is generated based on rules below:

- For workload kind `Deployment`, `Replicaset` and `ReplicationController` `Repository` is created with name `<WORKLOAD_KIND>.<WORKLOAD_NAME>`. For multiple replicas, only one `Repository` is created as backup is taken by sidecar of replica determined by leader-election.
- For workload kind `Statefulset` `Repository` is created with name`<WORKLOAD_KIND>.<POD_NAME>`. A separate `Repository` is created for each replica of a StatefulSet..
- For workload kind `Daemonset` Repository is created with name `<WORKLOAD_KIND>.<WORKLOAD_NAME>.<NODE_NAME>`. One repository is created for each node where pods of a DaemonSet are running.

## Working with Repository CRD

Here are some helpful commands to play with `Repository` CRD,

```
# List all Repositories of all namespaces in the cluster
$ kubectl get repository --all-namespaces

# List all Repositories created for a particular Restic
$ kubectl get repository -l restic=stash-demo --all-namespaces

# List all Repositories of Deployment workloads of all namespaces
$ kubectl get repository -l workload-kind=Deployment --all-namespaces

# List all Respositories of a particular Workload
$ kubectl get repository -l workload-kind=StatefulSet,workload-name=stash-demo

# List all Repositories of a particular node (DaemonSet only)
$ kubectl get repository -l node-name=minikube
```

## Next Steps

- Learn how to use Stash to backup a Kubernetes deployment [here](/docs/0.7.0-rc.3/guides/backup).
- To restore a backup see [here](/docs/0.7.0-rc.3/guides/restore).
- Learn about the details of Recovery CRD [here](/docs/0.7.0-rc.3/concepts/crds/recovery).
- To run backup in offline mode see [here](/docs/0.7.0-rc.3/guides/offline_backup)
- See the list of supported backends and how to configure them [here](/docs/0.7.0-rc.3/guides/backends).
- See working examples for supported workload types [here](/docs/0.7.0-rc.3/guides/workloads).
- Thinking about monitoring your backup operations? Stash works [out-of-the-box with Prometheus](/docs/0.7.0-rc.3/guides/monitoring).
- Learn about how to configure [RBAC roles](/docs/0.7.0-rc.3/guides/rbac).
- Want to hack on Stash? Check our [contribution guidelines](/docs/0.7.0-rc.3/CONTRIBUTING).
