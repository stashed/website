---
title: Recovery Overview
menu:
  docs_0.7.0:
    identifier: recovery-overview
    name: Recovery
    parent: crds
    weight: 20
product_name: stash
menu_name: docs_0.7.0
section_menu_id: concepts
info:
  version: 0.7.0
---

> New to Stash? Please start [here](/docs/0.7.0/concepts/README).

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

Indicates the name of the `Repository` CRD that represents respective **restic** repository where the backed up snapshots are stored. To know more about `Repository` CRD, visit [here](/docs/0.7.0/concepts/crds/repository).

### spec.repository.namespace

Indicates the `Namespace` of `Repository` CRD. This field allow the users to recover backed up volume from a different namespace.

### spec.snapshot

Indicates the name of the `Snapshot` object that represents **restic** backup snapshot. This field allows users to recover specific snapshot. To recover the latest snapshot, skip this field. To know more about `Snapshot`s, visit [here](/docs/0.7.0/concepts/crds/snapshot).

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

- Learn how to use Stash to backup a Kubernetes deployment [here](/docs/0.7.0/guides/backup).
- Learn about the details of Restic CRD [here](/docs/0.7.0/concepts/crds/restic).
- To restore a backup see [here](/docs/0.7.0/guides/restore).
- To run backup in offline mode see [here](/docs/0.7.0/guides/offline_backup)
- See the list of supported backends and how to configure them [here](/docs/0.7.0/guides/backends).
- See working examples for supported workload types [here](/docs/0.7.0/guides/workloads).
- Thinking about monitoring your backup operations? Stash works [out-of-the-box with Prometheus](/docs/0.7.0/guides/monitoring).
- Learn about how to configure [RBAC roles](/docs/0.7.0/guides/rbac).
- Want to hack on Stash? Check our [contribution guidelines](/docs/0.7.0/CONTRIBUTING).
