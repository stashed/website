---
title: Repository Locked | Stash
description: Troubleshooting "repository is already locked" issue
menu:
  docs_v2022.05.12:
    identifier: troubleshooting-repo-locked
    name: Repository Locked
    parent: troubleshooting
    weight: 10
product_name: stash
menu_name: docs_v2022.05.12
section_menu_id: guides
info:
  cli: v0.20.0
  community: v0.20.0
  elasticsearch:
  - 5.6.4-v17
  - 6.2.4-v17
  - 6.3.0-v17
  - 6.4.0-v17
  - 6.5.3-v17
  - 6.8.0-v17
  - 7.14.0-v3
  - 7.2.0-v17
  - 7.3.2-v17
  enterprise: v0.20.0
  etcd:
  - 3.5.0-v4
  installer: v2022.05.12
  kubedump:
  - 0.1.0
  mariadb:
  - 10.5.8-v10
  mongodb:
  - 3.4.17-v16
  - 3.4.22-v16
  - 3.6.13-v16
  - 3.6.8-v16
  - 4.0.11-v16
  - 4.0.3-v16
  - 4.0.5-v16
  - 4.1.13-v16
  - 4.1.4-v16
  - 4.1.7-v16
  - 4.2.3-v16
  - 4.4.6-v7
  - 5.0.3-v4
  mysql:
  - 5.7.25-v17
  - 8.0.14-v17
  - 8.0.21-v11
  - 8.0.3-v17
  nats:
  - 2.6.1-v4
  percona-xtradb:
  - 5.7-v12
  postgres:
  - 10.14-v15
  - 11.9-v15
  - 12.4-v15
  - 13.1-v12
  - 14.0-v4
  - 9.6.19-v15
  redis:
  - 5.0.13-v5
  - 6.2.5-v5
  ui-server: v0.2.0
  version: v2022.05.12
---

# Troubleshooting `"repository is already locked "` issue

Sometimes, the backend repository gets locked and the subsequent backup fails. In this guide, we are going to explain why this can happen and what you can do to solve the issue.

## Identifying the issue

If the repository gets locked, the new backup will fail. If you describe the `BackupSession`, you should see an error message indicating that the repository is already locked by another process.

```bash
kubectl describe -n <namespace> backupsession <backupsession name>
```

You will also see the error message in the backup sidecar/job log.

```bash
# For backup that uses sidecar (i.e. Deployment, StatefulSet etc.)
kubectl logs -n <namespace> <workload pod name> -c stash

# For backup that uses job (i.e. Database, PVC, etc.)
kubectl logs -n <namespace> <backup job's pod name> --all-containers
```

## Possible reasons

A restic process that modifies the repository, creates a lock at the beginning of its operation. When it completes the operation, it removes the lock so that other restic processes can use the repository. Now, if the process is killed unexpectedly, it can not remove the lock. As a result, the repository remains locked and becomes unusable for other processes.

### Possible scenarios when a repository can get locked

The repository can get locked in the following scenarios.

#### 1. The backup job/pod containing the sidecar has been terminated.

If the workload pod that has the `stash` sidecar or backup job's pod gets terminated while a backup is running, the repository can get locked. In this case, you have to find out why the pod was terminated.

#### 2. The temp-dir is set too low

Stash uses an `emptyDir` as a temporary volume where it stores cache for improving backup performance. By default, the `emptyDir` does not have any size limit. However, if you set the limit manually using `spec.tempDir` section of `BackupConfiguration` make sure you have set it to a reasonable size based on your targeted data size. If the `tempDir` limit is too low, cache size may cross the limit resulting in the backup pod getting evicted by Kubernetes. This is a tricky case because you may not notice that the backup pod has been evicted. You can describe the respective workload/job to check if it was the case.

In such a scenario, make sure that you have set the `tempDir` size to a reasonable amount. You can also disable caching by setting `spec.tempDir.disableCaching: true`. However, this might impact the backup performance significantly.

## Solutions

If your repository gets locked, you have to unlock it manually. You can use one of the following methods.

### Use Stash kubectl plugin

At first, install the Stash `kubectl` plugin by following the instruction [here](https://stash.run/docs/{{< param "info.version" >}}/setup/install/kubectl_plugin/).

Then, run the following command to unlock the repository:

```bash
kubectl stash unlock <repository name> --namespace=<namespace>
```

### Delete the locks folder from the backend

If you are using a cloud bucket that provides a UI to browse the storage, you can go to the repository directory and delete the `locks` folder.

<figure align="center">
  <img alt="Locks in the backend repository" src="images/repo_lock.png">
<figcaption align="center">Fig: Locks in the backend repository</figcaption>
</figure>

## Further Action

Once you have found the issue of why the repository got locked in the first place, take the necessary measure to prevent it from occurring in the future.
