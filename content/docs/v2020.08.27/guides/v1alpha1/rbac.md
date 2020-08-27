---
title: RBAC | Stash
description: RBAC
menu:
  docs_v2020.08.27:
    identifier: rbac-stash
    name: RBAC
    parent: v1alpha1-guides
    weight: 45
product_name: stash
menu_name: docs_v2020.08.27
section_menu_id: guides
info:
  catalog: v2020.08.27
  cli: v0.10.0
  community: v0.10.0
  enterprise: v0.10.0
  version: v2020.08.27
---

> New to Stash? Please start [here](/docs/v2020.08.27/concepts/README).

# Configuring RBAC

To use Stash in a RBAC enabled cluster, [install Stash](/docs/v2020.08.27/setup/README) with RBAC options. This creates a ClusterRole named `stash-sidecar`.

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

You can find full working examples [here](/docs/v2020.08.27/guides/v1alpha1/workloads).

## Next Steps

- Learn how to use Stash to backup a Kubernetes deployment [here](/docs/v2020.08.27/guides/v1alpha1/backup).
- Learn about the details of Restic CRD [here](/docs/v2020.08.27/concepts/crds/v1alpha1/restic).
- To restore a backup see [here](/docs/v2020.08.27/guides/v1alpha1/restore).
- Learn about the details of Recovery CRD [here](/docs/v2020.08.27/concepts/crds/v1alpha1/recovery).
- To run backup in offline mode see [here](/docs/v2020.08.27/guides/v1alpha1/offline_backup)
- See the list of supported backends and how to configure them [here](/docs/v2020.08.27/guides/v1alpha1/backends/overview).
- See working examples for supported workload types [here](/docs/v2020.08.27/guides/v1alpha1/workloads).
- Thinking about monitoring your backup operations? Stash works [out-of-the-box with Prometheus](/docs/v2020.08.27/guides/v1alpha1/monitoring/overview).
