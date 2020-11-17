---
title: RBAC | Stash
description: RBAC
menu:
  docs_v2020.11.17:
    identifier: rbac-stash
    name: RBAC
    parent: v1alpha1-guides
    weight: 45
product_name: stash
menu_name: docs_v2020.11.17
section_menu_id: guides
info:
  catalog: v2020.11.17
  cli: v0.11.7
  community: v0.11.7
  elasticsearch:
  - 5.6.4-v4
  - 6.2.4-v4
  - 6.3.0-v4
  - 6.4.0-v4
  - 6.5.3-v4
  - 6.8.0-v4
  - 7.2.0-v4
  - 7.3.2-v4
  enterprise: v0.11.7
  installer: v0.11.7
  mongodb:
  - 3.4.17-v4
  - 3.4.22-v4
  - 3.6.13-v4
  - 3.6.8-v4
  - 4.0.11-v4
  - 4.0.3-v4
  - 4.0.5-v4
  - 4.1.13-v4
  - 4.1.4-v4
  - 4.1.7-v4
  - 4.2.3-v4
  mysql:
  - 5.7.25-v4
  - 8.0.14-v4
  - 8.0.3-v4
  percona-xtradb:
  - 5.7.0
  postgres:
  - 10.14.0-v3
  - 11.9.0-v3
  - 12.4.0-v3
  - 13.1.0
  - 9.6.19-v3
  version: v2020.11.17
---

> New to Stash? Please start [here](/docs/v2020.11.17/concepts/README).

# Configuring RBAC

To use Stash in a RBAC enabled cluster, [install Stash](/docs/v2020.11.17/setup/README) with RBAC options. This creates a ClusterRole named `stash-sidecar`.

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

You can find full working examples [here](/docs/v2020.11.17/guides/v1alpha1/workloads).

## Next Steps

- Learn how to use Stash to backup a Kubernetes deployment [here](/docs/v2020.11.17/guides/v1alpha1/backup).
- Learn about the details of Restic CRD [here](/docs/v2020.11.17/concepts/crds/v1alpha1/restic).
- To restore a backup see [here](/docs/v2020.11.17/guides/v1alpha1/restore).
- Learn about the details of Recovery CRD [here](/docs/v2020.11.17/concepts/crds/v1alpha1/recovery).
- To run backup in offline mode see [here](/docs/v2020.11.17/guides/v1alpha1/offline_backup)
- See the list of supported backends and how to configure them [here](/docs/v2020.11.17/guides/v1alpha1/backends/overview).
- See working examples for supported workload types [here](/docs/v2020.11.17/guides/v1alpha1/workloads).
- Thinking about monitoring your backup operations? Stash works [out-of-the-box with Prometheus](/docs/v2020.11.17/guides/v1alpha1/monitoring/overview).
