---
title: RBAC | Stash
description: RBAC
menu:
  docs_v2021.04.12:
    identifier: rbac-stash
    name: RBAC
    parent: v1alpha1-guides
    weight: 45
product_name: stash
menu_name: docs_v2021.04.12
section_menu_id: guides
info:
  cli: v0.12.3
  community: v0.12.3
  elasticsearch:
  - 5.6.4-v9
  - 6.2.4-v9
  - 6.3.0-v9
  - 6.4.0-v9
  - 6.5.3-v9
  - 6.8.0-v9
  - 7.2.0-v9
  - 7.3.2-v9
  enterprise: v0.12.3
  installer: v2021.04.12
  mariadb:
  - 10.5.8-v3
  mongodb:
  - 3.4.17-v8
  - 3.4.22-v8
  - 3.6.13-v8
  - 3.6.8-v8
  - 4.0.11-v8
  - 4.0.3-v8
  - 4.0.5-v8
  - 4.1.13-v8
  - 4.1.4-v8
  - 4.1.7-v8
  - 4.2.3-v8
  mysql:
  - 5.7.25-v9
  - 8.0.14-v9
  - 8.0.21-v3
  - 8.0.3-v9
  percona-xtradb:
  - 5.7-v4
  postgres:
  - 10.14-v7
  - 11.9-v7
  - 12.4-v7
  - 13.1-v4
  - 9.6.19-v7
  version: v2021.04.12
---

> New to Stash? Please start [here](/docs/v2021.04.12/concepts/README).

# Configuring RBAC

To use Stash in a RBAC enabled cluster, [install Stash](/docs/v2021.04.12/setup/README) with RBAC options. This creates a ClusterRole named `stash-sidecar`.

Sidecar container added to workloads makes various calls to Kubernetes api. ServiceAccounts used with Deployment, ReplicaSet, DaemonSet and ReplicationController workloads are automatically bound to `stash-sidecar` ClusterRole by Stash operator. Users should manually add the following RoleBinding to service accounts used with StatefulSet workloads to authorize these api calls.

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: statefulset-name-stash-sidecar
  namespace: statefulset-namespace
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: stash-sidecar
subjects:
- kind: ServiceAccount
  name: statefulset-sa
  namespace: statefulset-namespace
```

You can find full working examples [here](/docs/v2021.04.12/guides/v1alpha1/workloads).

## Next Steps

- Learn how to use Stash to backup a Kubernetes deployment [here](/docs/v2021.04.12/guides/v1alpha1/backup).
- Learn about the details of Restic CRD [here](/docs/v2021.04.12/concepts/crds/v1alpha1/restic).
- To restore a backup see [here](/docs/v2021.04.12/guides/v1alpha1/restore).
- Learn about the details of Recovery CRD [here](/docs/v2021.04.12/concepts/crds/v1alpha1/recovery).
- To run backup in offline mode see [here](/docs/v2021.04.12/guides/v1alpha1/offline_backup)
- See the list of supported backends and how to configure them [here](/docs/v2021.04.12/guides/v1alpha1/backends/overview).
- See working examples for supported workload types [here](/docs/v2021.04.12/guides/v1alpha1/workloads).
- Thinking about monitoring your backup operations? Stash works [out-of-the-box with Prometheus](/docs/v2021.04.12/guides/v1alpha1/monitoring/overview).
