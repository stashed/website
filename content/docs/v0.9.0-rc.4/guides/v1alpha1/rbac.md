---
title: RBAC | Stash
description: RBAC
menu:
  docs_v0.9.0-rc.4:
    identifier: rbac-stash
    name: RBAC
    parent: v1alpha1-guides
    weight: 45
product_name: stash
menu_name: docs_v0.9.0-rc.4
section_menu_id: guides
info:
  catalog: v0.2.0
  cli: v0.3.0
  version: v0.9.0-rc.4
---

> New to Stash? Please start [here](/docs/v0.9.0-rc.4/concepts/README).

# Configuring RBAC

To use Stash in a RBAC enabled cluster, [install Stash](/docs/v0.9.0-rc.4/setup/install) with RBAC options. This creates a ClusterRole named `stash-sidecar`.

Sidecar container added to workloads makes various calls to Kubernetes api. ServiceAccounts used with Deployment, ReplicaSet, DaemonSet and ReplicationController workloads are automatically bound to `stash-sidecar` ClusterRole by Stash operator. Users should manually add the following RoleBinding to service accounts used with StatefulSet workloads to authorize these api calls.

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: <statefulset-name>-stash-sidecar
  namespace: <statefulset-namespace>
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: stash-sidecar
subjects:
- kind: ServiceAccount
  name: <statefulset-sa>
  namespace: <statefulset-namespace>
```

You can find full working examples [here](/docs/v0.9.0-rc.4/guides/v1alpha1/workloads).

## Next Steps

- Learn how to use Stash to backup a Kubernetes deployment [here](/docs/v0.9.0-rc.4/guides/v1alpha1/backup).
- Learn about the details of Restic CRD [here](/docs/v0.9.0-rc.4/concepts/crds/v1alpha1/restic).
- To restore a backup see [here](/docs/v0.9.0-rc.4/guides/v1alpha1/restore).
- Learn about the details of Recovery CRD [here](/docs/v0.9.0-rc.4/concepts/crds/v1alpha1/recovery).
- To run backup in offline mode see [here](/docs/v0.9.0-rc.4/guides/v1alpha1/offline_backup)
- See the list of supported backends and how to configure them [here](/docs/v0.9.0-rc.4/guides/v1alpha1/backends/overview).
- See working examples for supported workload types [here](/docs/v0.9.0-rc.4/guides/v1alpha1/workloads).
- Thinking about monitoring your backup operations? Stash works [out-of-the-box with Prometheus](/docs/v0.9.0-rc.4/guides/v1alpha1/monitoring/overview).
- Want to hack on Stash? Check our [contribution guidelines](/docs/v0.9.0-rc.4/CONTRIBUTING).
