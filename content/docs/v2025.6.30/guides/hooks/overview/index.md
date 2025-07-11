---
title: Hooks Overview | Stash
menu:
  docs_v2025.6.30:
    identifier: hooks-overview
    name: Overview
    parent: hooks
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

# Stash Backup and Restore Hooks

Stash `v0.9.0+` supports executing custom commands before and after backup or restore process. This is called `hook` in Stash. This guide will give you an overview of what kind of hooks you can execute, how the hooks get executed, and how the hooks behave in different scenarios.

## Types of Hooks

We can categorize Stash backup and restore hooks based on the action they perform and based on their execution order.

### Based on Action

Based on the action of a hook, we can categorize them into four different categories. These are the followings:

- **HTTPGet:** Executes an HTTP GET request before/after the backup/restore process. The hook is considered successful if the return code is between `200` and `400`.

- **HTTPPost:** Executes an HTTP POST request before/after the backup/restore process. Like `HTTPGet`, the hook is considered successful if the return code is between `200` and `400`.

- **TCPSocket:** Performs a TCP check against the provided URL on a specific port before/after the backup/restore process. The hook is considered successful if the targeted port is open.

- **Exec:** Executes commands inside a targeted container before/after the backup/restore process. The hook is considered successful if the command executes with exit code 0.

### Execution Phases

Based on the execution order, we can categorize the hooks into two different phases. These are the followings:

- **Pre-Task Hook:** Pre task hooks are executed before the backup or restore process. `preBackup` and `preRestore` are the pre-task hooks.

- **Post-Task Hook:** Post task hooks are executed after the backup or restore process. `postBackup` and `postRestore` are the post-task hooks.

However, there is one more type of hooks for [BackupBatch](/docs/v2025.6.30/concepts/crds/backupbatch/) object. We call them **Global Hooks**. They get executed before any other individual target's hooks get executed (for the pre-task hooks) or after all the individual target's hooks has executed (for the post-task hooks).

## Who Executes the Hooks

You might be familiar that Stash uses two different models to take backup of the target based on their type. For Kubernetes workloads (i.e. Deployment, DaemonSet, StatefulSet etc.), Stash injects a sidecar into the workload that takes backup. However, for databases and standalone PVC backup, Stash creates a job for the task. The hooks are executed differently for these two different models.

Furthermore, we have introduced [BackupBatch](/docs/v2025.6.30/concepts/crds/backupbatch/) which allows to specify multiple target simultaneously. The individual targets may follow the sidecar model or the job model. The `BackupBatch` object allows specifying a global hook for all the targets as well as some local hooks for individual targets. This type of hooks also handled differently.

Here, we are going to discuss how Stash executes the hooks in different scenarios.

- **Sidecar Model:** In sidecar model, hooks are executed by the backup sidecar or restore init-container. The hook execution flow by sidecar/init-container is shown in the following diagram:

  <figure align="center">
    <img alt="Hook Execution flow in sidecar model" src="images/sidecar-model.svg">
  <figcaption align="center">Fig: Hook Execution flow in sidecar model</figcaption>
  </figure>

- **Job Model:** In the Job model, the hooks are run by the backup/restore job. For the `exec` hook, the command is executed inside the targeted application pod. In order to determine the targeted application pod, Stash uses the `Service` specified in the respective `AppBinding` CRD. It first determines the endpoints of the Service. Then, it executes  the hook into one of the pod pointed by those endpoints. Hence, if the `AppBinding` points to an external URL, it is not possible for Stash to execute the hook. The hook execution flow in job model is shown in the following diagram:

  <figure align="center">
    <img alt="Hook Execution flow in job model" src="images/job-model.svg">
  <figcaption align="center">Fig: Hook Execution flow in job model</figcaption>
  </figure>

> Note: If the postBackup hook doesn't run (e.g., because of a timeout, backup disruption, etc.), the Stash operator will handle it instead of the backup job. However, if the preBackup hook is configured but doesn’t execute or fails the postBackup hook will not be run.

- **Batch Backup:** In batch backup using `BackupBatch` object, the global hooks are executed by the Stash operator itself. When Stash operator completes executing the global pre-task hook, the individual targets start executing their local pre-task hook. Then, they complete their backup process and executes their local post-task hook. Finally, the Stash operator executes global post-task hooks. The hook execution flow in batch backup is shown in the following diagram:

<figure align="center">
  <img alt="Hook Execution flow in batch backup" src="images/batch-backup.svg">
<figcaption align="center">Fig: Hook Execution flow in batch backup</figcaption>
</figure>

## Hook's Behaviors

Now, we are going to discuss what will happen when a hook fails or backup/restore process fails.

### `preBackup` or `preRestore` hook

If a `preBackup` or `preRestore` hook fails to execute, the rest of the backup/restore process will be skipped and the respective `BackupSession`/`RestoreSession` will be marked as `Failed`. You may see the following things happen in addition to skipping the backup process:

- **Backup Sidecar:** If the `preBackup` hook fails in the backup sidecar, the sidecar will just log the failure and continue watching for `BackupSession` for the next backup.
- **Restore Init-Container:** If the `preRestore` hook fails in restore init-container, the container will crash. Hence, your workload will be stuck in the initialization phase.
- **Backup or Restore Job:** If the `preBackup` or `preRestore` hook fails in backup/restore job, the container will fail. Hence, the job will never go into completed stage. You may see the job creating multiple pods to retry.

### `postBackup` or `postRestore` hook

If the backup or restore process fails then the respective `postBackup` or `postRestore` hook will be executed according to the policy specified in the `executionPolicy` field of the respective hook. The current acceptable values and behaviors are:

- `Always`: The hook will be executed after the backup/restore process no matter the backup/restore has failed or succeeded. This is the default behavior.
- `OnSuccess`: The hook will be executed after the backup/restore process only if the backup/restore has succeeded.
- `OnFailure`: The hook will be executed after the backup/restore process only if the backup/restore has failed.
- `OnFinalRetryFailure`: The hook will be executed after the backup process only if the backup has failed with no more retry attempts left.

In case of backup job, if the postBackup hook container doesn't run for any reason, the postBackup hook is executed by the Stash operator instead of the backup job (if targeted application is not `AppBinding`). For `AppBinding`, hook is executed in the targeted application pod.

If the `postBackup` or `postRestore` hook fails, the respective BackupSession or RestoreSession will be marked as `Failed`.

## Templating Support in Hook

Stash support [Go template](https://pkg.go.dev/text/template) in hook. This is particularly helpful when you want to send custom message to a Slack webhook with information about the backup/restore session.

Stash exposes a summary for a backup/restore process. The Go template variables are then resolved from the summary. Here, is an example of a summary exposed by Stash:

```json
{
    "name": "deployment-backup-1646741400",
    "namespace": "demo",
    "invoker":{
        "apiGroup": "stash.appscode.com",
        "kind": "BackupConfiguration",
        "name": "deployment-backup"
    },
    "target":{
        "apiVersion": "apps/v1",
        "kind": "Deployment",
        "name": "stash-demo"
    },
    "status":{
        "phase": "Failed",
        "duration": "10s",
        "error": "failed to backup host-0. Reason: path not found"
    }
}
```

The summary contains the following fields:

- **name:** Name of the respective BackupSession or RestoreSession. You can access it in your template as `.Name` variable.
- **namespace:** Namespace of the respective BackupSession or RestoreSession. You can access it in your template as `.Namespace` variable.
- **invoker:** Contains the respective BackupConfiguration or RestoreSession information which is responsible for triggering this backup or restore process. It has the following fields:
  - **apiVersion:** API version of the invoker. You can access it in your template as `.Invoker.ApiVersion` variable.
  - **kind:** Kind of the invoker. You can access it in your template as `.Invoker.Kind` variable.
  - **name:** Name of the invoker. You can access it in your template as `.Invoker.Name` variable.

- **target:** Contains respective backup/restore target information. It has the following fields:
  - **apiVersion:** API version of the target. You can access it in your template as `.Target.ApiVersion` variable.
  - **kind:** Kind of the target. You can access it in your template as `.Target.Kind` variable.
  - **name:** Name of the target. You can access it in your template as `.Target.Name` variable.

- **status:** Specifies the backup/restore status. It has the following fields:
  - **phase:** Phase of the backup/restore process. You can access it in your template as `.Status.Phase` variable.
  - **duration:** Specifies how long it took to complete the backup/restore process. You can access it in your template as `Status.Duration` variable.
  - **error:** If the backup/restore process fail, this field contains the reason why it failed. You can access it in your template as `.Status.Error` variable.

Below is an example of using Go template in hook. Here, we are sending a message to a slack incoming webhook if the backup process fails.

```yaml
hooks:
  postBackup:
    executionPolicy: OnFailure
    httpPost:
      host: hooks.slack.com
      path: /services/XX/XXX/XXXX
      port: 443
      scheme: HTTPS
      httpHeaders:
        - name: Content-Type
          value: application/json
      body: |
          {{- $msg := dict  "type" "mrkdwn" "text" (printf ":x: Backup failed for %s/%s Reason: %s." .Namespace .Target.Name .Status.Error) -}}
          {
            "blocks": [
                {
                  "type": "section",
                  "text": {{ toJson $msg }}
                }
              ]
          }
```
