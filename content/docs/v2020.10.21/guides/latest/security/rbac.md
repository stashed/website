---
title: RBAC | Stash
description: Using Stash in RBAC enabled cluster
menu:
  docs_v2020.10.21:
    identifier: security-rbac
    name: RBAC
    parent: security
    weight: 10
product_name: stash
menu_name: docs_v2020.10.21
section_menu_id: guides
info:
  catalog: v2020.10.21
  cli: v0.11.3
  community: v0.11.3
  elasticsearch:
  - 5.6.4-v3
  - 6.2.4-v3
  - 6.3.0-v3
  - 6.4.0-v3
  - 6.5.3-v3
  - 6.8.0-v3
  - 7.2.0-v3
  - 7.3.2-v3
  enterprise: v0.11.3
  installer: v0.11.3
  mongodb:
  - 3.4.17-v3
  - 3.4.22-v3
  - 3.6.13-v3
  - 3.6.8-v3
  - 4.0.11-v3
  - 4.0.3-v3
  - 4.0.5-v3
  - 4.1.13-v3
  - 4.1.4-v3
  - 4.1.7-v3
  - 4.2.3-v3
  mysql:
  - 5.7.25-v3
  - 8.0.14-v3
  - 8.0.3-v3
  percona-xtradb:
  - 5.7-v3
  postgres:
  - 10.14.0-v1
  - 11.9.0-v1
  - 12.4.0-v1
  - 9.6.19-v1
  version: v2020.10.21
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

These user facing roles supports [ClusterRole Aggregation](https://kubernetes.io/docs/admin/authorization/rbac/#aggregated-clusterroles) feature in Kubernetes 1.9 or later clusters.
