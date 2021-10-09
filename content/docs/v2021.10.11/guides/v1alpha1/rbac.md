---
title: RBAC | Stash
description: RBAC
menu:
  docs_v2021.10.11:
    identifier: rbac-stash
    name: RBAC
    parent: v1alpha1-guides
    weight: 45
product_name: stash
menu_name: docs_v2021.10.11
section_menu_id: guides
info:
  cli: v0.16.0
  community: v0.16.0
  elasticsearch:
  - 5.6.4-v13
  - 6.2.4-v13
  - 6.3.0-v13
  - 6.4.0-v13
  - 6.5.3-v13
  - 6.8.0-v13
  - 7.2.0-v13
  - 7.3.2-v13
  enterprise: v0.16.0
  etcd:
  - 3.5.0
  installer: v2021.10.11
  mariadb:
  - 10.5.8-v6
  mongodb:
  - 3.4.17-v12
  - 3.4.22-v12
  - 3.6.13-v12
  - 3.6.8-v12
  - 4.0.11-v12
  - 4.0.3-v12
  - 4.0.5-v12
  - 4.1.13-v12
  - 4.1.4-v12
  - 4.1.7-v12
  - 4.2.3-v12
  - 4.4.6-v3
  - 5.0.3
  mysql:
  - 5.7.25-v13
  - 8.0.14-v13
  - 8.0.21-v7
  - 8.0.3-v13
  nats:
  - 2.6.1
  percona-xtradb:
  - 5.7-v8
  postgres:
  - 10.14-v11
  - 11.9-v11
  - 12.4-v11
  - 13.1-v8
  - "14.0"
  - 9.6.19-v11
  redis:
  - 5.0.13-v1
  - 6.2.5-v1
  version: v2021.10.11
---

> New to Stash? Please start [here](/docs/v2021.10.11/concepts/README).

# Configuring RBAC

To use Stash in a RBAC enabled cluster, [install Stash](/docs/v2021.10.11/setup/README) with RBAC options. This creates a ClusterRole named `stash-sidecar`.

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

You can find full working examples [here](/docs/v2021.10.11/guides/v1alpha1/workloads).

## Next Steps

- Learn how to use Stash to backup a Kubernetes deployment [here](/docs/v2021.10.11/guides/v1alpha1/backup).
- Learn about the details of Restic CRD [here](/docs/v2021.10.11/concepts/crds/v1alpha1/restic).
- To restore a backup see [here](/docs/v2021.10.11/guides/v1alpha1/restore).
- Learn about the details of Recovery CRD [here](/docs/v2021.10.11/concepts/crds/v1alpha1/recovery).
- To run backup in offline mode see [here](/docs/v2021.10.11/guides/v1alpha1/offline_backup)
- See the list of supported backends and how to configure them [here](/docs/v2021.10.11/guides/v1alpha1/backends/overview).
- See working examples for supported workload types [here](/docs/v2021.10.11/guides/v1alpha1/workloads).
- Thinking about monitoring your backup operations? Stash works [out-of-the-box with Prometheus](/docs/v2021.10.11/guides/v1alpha1/monitoring/overview).
