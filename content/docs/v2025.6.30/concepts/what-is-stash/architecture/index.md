---
title: Stash Architecture
description: Stash Architecture
menu:
  docs_v2025.6.30:
    identifier: architecture-concepts
    name: Architecture
    parent: what-is-stash
    weight: 20
product_name: stash
menu_name: docs_v2025.6.30
section_menu_id: concepts
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

# Stash Architecture

Stash is a Kubernetes operator for [restic](https://restic.net/). At the heart of Stash, it is a Kubernetes [controller](https://book.kubebuilder.io/cronjob-tutorial/controller-overview.html). It uses [Custom Resource Definition(CRD)](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/) to specify targets and behaviors of backup and restore process in a Kubernetes native way. A simplified architecture of Stash is shown below:

<figure align="center">
  <img alt="Stash Architecture" src="images/stash_architecture.svg">
  <figcaption align="center">Fig: Stash Architecture</figcaption>
</figure>

## Components

Stash consists of various components that implement backup and restore logic. This section will give you a brief overview of such components.

### Stash Operator

When a user installs Stash, it creates a Kubernetes [Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) typically named `stash-operator`. This deployment controls the entire backup and restore process. `stash-operator` deployment runs two containers. One of them is called `operator` which performs the core functionality of Stash and the other one is `pushgateway` which is a Prometheus [pushgateway](https://github.com/prometheus/pushgateway).

#### Operator

`operator` container runs all the controllers as well as an [Aggregated API Server](https://kubernetes.io/docs/tasks/access-kubernetes-api/setup-extension-api-server/).

##### Controllers

Controllers watch various Kubernetes resources as well as the custom resources introduced by Stash. It applies the backup or restore logic for a target resource when requested by users.

##### Aggregated API Server

Aggregated API Server self-hosts validating and mutating [webhooks](https://kubernetes.io/docs/reference/access-authn-authz/extensible-admission-controllers/) and runs an Extension API Server for Snapshot resource.

- **Mutating Webhook:** Stash uses Mutating Webhook to inject backup `sidecar` or restore `init-container` into a workload if any backup or restore process is configured for it. It is also used for defaulting custom resources.

- **Validating Webhook:** Validating Webhook is used to validate the custom resource objects.

- **Snapshot Server:** Stash uses Kubernetes Extended API Server to provide `view` and `list` capability of backed up snapshots. When a user requests for Snapshot objects, Snapshot server reads respective information directly from backend repository and returns object representation in a Kubernetes native way.

#### Pushgateway

`pushgateway` container runs Prometheus [pushgateway](https://github.com/prometheus/pushgateway). All the backup sidecars/jobs and restore init-containers/jobs send Prometheus metrics to this pushgateway after completing their backup or restore process. Prometheus server can scrape those metrics from this pushgateway.

### Backend

Backend is the storage where Stash stores backed up files. It can be a cloud storage like GCS bucket, AWS S3, Azure Blob Storage etc. or a Kubernetes persistent volume like [NFS](https://kubernetes.io/docs/concepts/storage/volumes/#nfs), [PersistentVolumeClaim](https://kubernetes.io/docs/concepts/storage/volumes/#persistentvolumeclaim), etc. To learn more about backend, please visit [here](/docs/v2025.6.30/guides/backends/overview/).

### CronJob

When a user creates a [BackupConfiguration](#backupconfiguration) object, Stash creates a CronJob with the schedule specified in it. At each scheduled slot, this CronJob triggers a backup for the targeted workload.

### Backup Sidecar / Backup Job

When a user creates a [BackupConfiguration](#backupconfiguration) object, Stash injects a `sidecar` to the target if it is a workload (i.e. `Deployment`, `DaemonSet`, `StatefulSet` etc.). This `sidecar` takes backup when the respective CronJob triggers a backup. If the target is a database or stand-alone volume, Stash creates a job to take backup at each trigger.

### Restore Init-Container / Restore Job

When a user creates a [RestoreSession](#restoresession) object, Stash injects an `init-container` to the target if it is a workload (i.e. `Deployment`, `DaemonSet`, `StatefulSet` etc.). This `init-container` performs restore process on restart. If the target is a database or stand-alone volume, Stash creates a job to restore the target.

### Custom Resources

Stash uses [Custom Resource Definition(CRD)](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/) to specify targets and behaviors of backup and restore process in a Kubernetes native way. This section will give you a brief overview of the custom resources used by Stash.

- **Repository:** A `Repository` specifies the backend storage system where the backed up data will be stored. A user has to create `Repository` object for each backup target. Only one target can be backed up into one `Repository`. For details about `Repository`, please visit [here](/docs/v2025.6.30/concepts/crds/repository/).

- **BackupConfiguration:** A `BackupConfiguration` specifies the backup target, behaviors (schedule, retention policy etc.), `Repository` object that holds backend information etc. A user has to create one `BackupConfiguration` object for each backup target. When a user creates a `BackupConfiguration`, Stash creates a CronJob for it and injects backup sidecar to the target if it is a workload (i.e. Deployment, DaemonSet, StatefulSet etc.). For more details about `BackupConfiguration`, please visit [here](/docs/v2025.6.30/concepts/crds/backupconfiguration/).

- **BackupSession:** A `BackupSession` object represents a backup run of a target. It is created by respective CronJob at each scheduled time slot. It refers to a `BackupConfiguration` object for necessary configuration. Controller that runs inside backup sidecar (in case of backup via job, it is stash operator itself) will watch this `BackupSession` object and start taking the backup instantly. A user can also create a `BackupSession` object manually to trigger instant backups. For more details about `BackupSession`s, please visit [here](/docs/v2025.6.30/concepts/crds/backupsession/).

- **RestoreSession:** A `RestoreSession` specifies what to restore and the source of data. A user has to create a `RestoreSession` object when s/he wants to restore a target. When s/he creates a `RestoreSession`, Stash injects an `init-container` into the target workload (launches a job if the target is not a workload) to restore. For more details about `RestoreSession`, please visit [here](/docs/v2025.6.30/concepts/crds/restoresession/).

- **Function:** A `Function` is a template for a container that performs only a specific action.  For example, `pg-backup` function only dumps and uploads the dumped file into the backend, whereas `update-status` function updates the status of the respective `BackupSession` and `Repository` and sends Prometheus metrics to `pushgateway` based on the output of another function. For more details about `Function`, please visit [here](/docs/v2025.6.30/concepts/crds/function/).

- **Task:** A complete backup or restore process may consist of several steps. For example, in order to backup a PostgreSQL database we first need to dump the database, upload the dumped file to backend and then we need to update `Repository` and `BackupSession` status and send Prometheus metrics. We represent such individual steps via `Function` objects. An entire backup or restore process needs an ordered execution of one or more functions. A `Task` specifies an ordered collection of functions along with their parameters. `Function` and `Task` enables users to extend or customize the backup/restore process. For more details about `Task`, please visit [here](/docs/v2025.6.30/concepts/crds/task/).

- **BackupBlueprint:** A `BackupBlueprint` enables users to provide a blueprint for `Repository` and `BackupConfiguration` object. Then, s/he just needs to add some annotations to the workload s/he wants to backup. Stash will automatically create respective `Repository` and `BackupConfiguration` according to the blueprint. In this way, users can create a single blueprint for all similar types of workloads and backup them only by applying some annotations on them. In Stash parlance, we call this process **Auto Backup**. For more details about `BackupBlueprint`, please visit [here](/docs/v2025.6.30/concepts/crds/backupblueprint/).

- **BackupBatch:** Sometimes, a single stateful component may not meet the requirements of your application. For example, in order to deploy a WordPress, you will need a Deployment for the WordPress and another Deployment for database to store it's contents. Now, you may want to backup both of the deployment and database together as they are parts of a single application. A `BackupBatch` is a Kubernetes `CustomResourceDefinition`(CRD) which lets you configure backup for multiple co-related stateful components(workload, database etc.) together. For more details, please visit [here](/docs/v2025.6.30/concepts/crds/backupbatch/).

- **RestoreBatch:** A `RestoreBatch` allows restoring of multiple co-related targets together that were backed up using a `BackupBatch`. For more details, please visit [here](/docs/v2025.6.30/concepts/crds/restorebatch/).

- **AppBinding:** An `AppBinding` holds necessary information to connect with an application. For more details about `AppBinding`, please visit [here](/docs/v2025.6.30/concepts/crds/appbinding/).

- **Snapshot:** A `Snapshot` is a representation of a backup snapshot in a Kubernetes native way. Stash uses Kuberentes Extended API Server for handling `Snapshot`s. For more details about `Snapshot`s, please visit [here](/docs/v2025.6.30/concepts/crds/snapshot/).
