---
title: Recovery Overview
menu:
  docs_v2021.11.24:
    identifier: recovery-overview
    name: Recovery
    parent: v1alpha1-crds
    weight: 20
product_name: stash
menu_name: docs_v2021.11.24
section_menu_id: concepts
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

>**Warning:** Recovery crd has been deprecated in **v1beta1 api** and will be removed in future version. Please use [RestoreSession](/docs/v2021.11.24/concepts/crds/restoresession) instead.

# Recoveries

## What is Recovery

A `Recovery` is a Kubernetes `CustomResourceDefinition` (CRD). It provides configuration for restoring a backup taken using Stash. You only need to specify the `Repository`, `Snapshot` and `path` you want to recover and volume where the backup will be restored.

## Recovery Spec

As with all other Kubernetes objects, a Recovery needs `apiVersion`, `kind`, and `metadata` fields. It also needs a `.spec` section. Below is an example Recovery object.

```yaml
apiVersion: stash.appscode.com/v1alpha1
kind: Recovery
metadata:
  name: stash-demo
  namespace: default
spec:
  repository:
    name: deployment.stash-demo
    namespace: default
  snapshot: deployment.stash-demo-e0e9c272 # skip this field to recover latest snapshot
  paths:
  - /source/data
  recoveredVolumes:
  - mountPath: /source/data
    hostPath:
      path: /data/stash-test/restic-restored
```

The `.spec` section has following parts:

### spec.repository.name

Indicates the name of the `Repository` CRD that represents respective **restic** repository where the backed up snapshots are stored. To know more about `Repository` CRD, visit [here](/docs/v2021.11.24/concepts/crds/repository).

### spec.repository.namespace

Indicates the `Namespace` of `Repository` CRD. This field allow the users to recover backed up volume from a different namespace.

### spec.snapshot

Indicates the name of the `Snapshot` object that represents **restic** backup snapshot. This field allows users to recover specific snapshot. To recover the latest snapshot, skip this field. To know more about `Snapshot`s, visit [here](/docs/v2021.11.24/concepts/crds/snapshot).

### spec.paths

An array of strings specifying the file-group paths that were backed up using `Restic`.

### spec.recoveredVolumes

Indicates an array of volumes where recovered snapshot data will be stored. Here, `mountPath` specifies where the volume will be mounted in the restore `Job`. Note that, `Recovery` recovers data in the same paths from where the backup was taken (specified in `spec.paths`). So, volumes must be mounted on those paths or their parent paths. Following parameters are available for `recoveredVolumes`.

| Parameter                       | Description                                                                                       |
|---------------------------------|---------------------------------------------------------------------------------------------------|
| `recoveredVolumes.mountPath`    | `Required`. The path where this volume will be mounted in the sidecar container. Example: `/repo` |
| `recoveredVolumes.subPath`      | `Optional`. Sub-path inside the referenced volume instead of its root.                            |
| `recoveredVolumes.VolumeSource` | `Required`. Any Kubernetes volume. Can be specified inlined. Example: `hostPath`                  |

## Recovery Status

Stash operator updates `.status` of a Recovery CRD when the recovery operation is completed.

 - `status.phase` indicates the current phase of the overall recovery process. Possible values are `Pending`, `Running`, `Succeeded`, `Failed` and `Unknown`.
 - `status.stats` is an array status, each of which indicates the status for individual paths. Each element of the array has following fields:
   - `status.stats[].path` indicates a path that was backed up using `Restic` and is selected for recovery.
   - `status.stats[].phase` indicates the current phase of the recovery process for the particular path. Possible values are `Pending`, `Running`, `Succeeded`, `Failed` and `Unknown`.
   - `status.stats[].duration` indicates the elapsed time to successfully restore backup for the particular path.

## Next Steps

- Learn how to use Stash to backup a Kubernetes deployment [here](/docs/v2021.11.24/guides/v1alpha1/backup).
- Learn about the details of Restic CRD [here](/docs/v2021.11.24/concepts/crds/v1alpha1/restic).
- To restore a backup see [here](/docs/v2021.11.24/guides/v1alpha1/restore).
- To run backup in offline mode see [here](/docs/v2021.11.24/guides/v1alpha1/offline_backup)
- See the list of supported backends and how to configure them [here](/docs/v2021.11.24/guides/v1alpha1/backends/overview).
- See working examples for supported workload types [here](/docs/v2021.11.24/guides/v1alpha1/workloads).
- Thinking about monitoring your backup operations? Stash works [out-of-the-box with Prometheus](/docs/v2021.11.24/guides/v1alpha1/monitoring/overview).
- Learn about how to configure [RBAC roles](/docs/v2021.11.24/guides/v1alpha1/rbac).
