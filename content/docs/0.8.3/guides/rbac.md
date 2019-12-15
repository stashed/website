---
title: RBAC | Stash
description: RBAC
menu:
  docs_0.8.3:
    identifier: rbac-stash
    name: RBAC
    parent: guides
    weight: 45
product_name: stash
menu_name: docs_0.8.3
section_menu_id: guides
info:
  version: 0.8.3
---

> New to Stash? Please start [here](/docs/0.8.3/concepts/README).

# Configuring RBAC

To use Stash in a RBAC enabled cluster, [install Stash](/docs/0.8.3/setup/install) with RBAC options. This creates a ClusterRole named `stash-sidecar`.

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

You can find full working examples [here](/docs/0.8.3/guides/workloads).

## Next Steps

- Learn how to use Stash to backup a Kubernetes deployment [here](/docs/0.8.3/guides/backup).
- Learn about the details of Restic CRD [here](/docs/0.8.3/concepts/crds/restic).
- To restore a backup see [here](/docs/0.8.3/guides/restore).
- Learn about the details of Recovery CRD [here](/docs/0.8.3/concepts/crds/recovery).
- To run backup in offline mode see [here](/docs/0.8.3/guides/offline_backup)
- See the list of supported backends and how to configure them [here](/docs/0.8.3/guides/backends/overview).
- See working examples for supported workload types [here](/docs/0.8.3/guides/workloads).
- Thinking about monitoring your backup operations? Stash works [out-of-the-box with Prometheus](/docs/0.8.3/guides/monitoring/overview).
- Want to hack on Stash? Check our [contribution guidelines](/docs/0.8.3/CONTRIBUTING).
