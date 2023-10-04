---
title: RBAC | Stash
description: Using Stash in RBAC enabled cluster
menu:
  docs_v2023.10.9:
    identifier: security-rbac
    name: RBAC
    parent: security
    weight: 10
product_name: stash
menu_name: docs_v2023.10.9
section_menu_id: guides
info:
  cli: v0.32.0
  community: v0.32.0
  elasticsearch:
  - 5.6.4-v28
  - 6.2.4-v28
  - 6.3.0-v28
  - 6.4.0-v28
  - 6.5.3-v28
  - 6.8.0-v28
  - 7.14.0-v14
  - 7.2.0-v28
  - 7.3.2-v28
  - 8.2.0-v11
  enterprise: v0.32.0
  etcd:
  - 3.5.0-v15
  installer: v2023.10.9
  kubedump:
  - 0.1.0-v11
  mariadb:
  - 10.5.8-v21
  mongodb:
  - 3.4.17-v28
  - 3.4.22-v28
  - 3.6.13-v28
  - 3.6.8-v28
  - 4.0.11-v28
  - 4.0.3-v28
  - 4.0.5-v28
  - 4.1.13-v28
  - 4.1.4-v28
  - 4.1.7-v28
  - 4.2.3-v28
  - 4.4.6-v19
  - 5.0.15-v1
  - 5.0.3-v16
  - 6.0.5-v4
  mysql:
  - 5.7.25-v28
  - 8.0.14-v28
  - 8.0.21-v22
  - 8.0.3-v28
  nats:
  - 2.6.1-v16
  - 2.8.2-v11
  percona-xtradb:
  - 5.7-v23
  postgres:
  - 10.14-v27
  - 11.9-v27
  - 12.4-v27
  - 13.1-v24
  - 14.0-v16
  - 15.1-v8
  - 9.6.19-v27
  redis:
  - 5.0.13-v16
  - 6.2.5-v16
  - 7.0.5-v9
  ui-server: v0.13.0
  vault:
  - 1.10.3-v8
  version: v2023.10.9
---

# Stash with RBAC Enabled Cluster

Stash comes with built-in support for RBAC enabled cluster. Stash installer create a `ClusterRole` and `RoleBinding` giving necessary permission to the operator.

## Operator Permissions

Stash operator needs the following RBAC permissions,

| API Groups                   | Resources                                                      | Permissions                                 |
| ---------------------------- | -------------------------------------------------------------- | ------------------------------------------- |
| apiextensions.k8s.io         | customresourcedefinitions                                      | *                                           |
| apiextensions.k8s.io         | apiservices                                                    | get, patch, delete                          |
| admissionregistration.k8s.io | mutatingwebhookconfigurations, validatingwebhookconfigurations | get, list, watch, patch, delete             |
| stash.appscode.com           | *                                                              | *                                           |
| appcatalog.appscode.com      | *                                                              | *                                           |
| apps                         | daemonsets, deployments, replicasets, statefulsets             | get, list, watch, patch                     |
| batch                        | jobs, cronjobs                                                 | get, list, watch, create, patch, delete     |
| ""                           | namespaces, replicationcontrollers                             | get, list, watch, patch                     |
| ""                           | configmaps                                                     | get, list, watch,create, update, delete     |
| ""                           | persistentvolumeclaims                                         | get, list, watch, create, patch             |
| ""                           | services, endpoints                                            | get                                         |
| ""                           | secrets, events                                                | get, list, create, patch                    |
| ""                           | nodes                                                          | list                                        |
| ""                           | pods, pods/exec                                                | get, list, create, delete, deletecollection |
| ""                           | serviceaccounts                                                | get, create, patch, delete                  |
| rbac.authorization.k8s.io    | clusterroles, roles, rolebindings, clusterrolebindings         | get, create, delete, patch                  |
| apps.openshift.io            | deploymentconfigs                                              | get, list, watch, patch                     |
| policy                       | podsecuritypolicies                                            | use                                         |
| snapshot.storage.k8s.io      | volumesnapshots, volumesnapshotcontents, volumesnapshotclasses | get, list, watch, create, patch, delete     |
| storage.k8s.io               | storageclasses                                                 | get                                         |

Here,

- `""` in API Group column means `core` API groups.
- `*` in Resources colum means all resources.
- `*` in Permission colum means all permissions.

## User facing ClusterRoles

Stash introduces custom resources, such as, `BackupConfiguration`,`BackupBatch`, `BackupSession`,  `Repository`, `RestoreSession`, `RestoreBatch`, `Function`, and `Task` etc. Stash installer will create 2 user facing cluster roles:

| ClusterRole         | Aggregates To | Desription                                                                                            |
| ------------------- | ------------- | ----------------------------------------------------------------------------------------------------- |
| appscode:stash:edit | admin, edit   | Allows edit access to Stash CRDs, intended to be granted within a namespace using a RoleBinding.      |
| appscode:stash:view | view          | Allows read-only access to Stash CRDs, intended to be granted within a namespace using a RoleBinding. |

These user facing roles supports [ClusterRole Aggregation](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#aggregated-clusterroles) feature in Kubernetes 1.9 or later clusters.
