---
title: RBAC | Stash
description: Using Stash in RBAC enabled cluster
menu:
  docs_v2024.12.18:
    identifier: security-rbac
    name: RBAC
    parent: security
    weight: 10
product_name: stash
menu_name: docs_v2024.12.18
section_menu_id: guides
info:
  cli: v0.37.0
  community: v0.37.0
  elasticsearch:
  - 5.6.4-v33
  - 6.2.4-v33
  - 6.3.0-v33
  - 6.4.0-v33
  - 6.5.3-v33
  - 6.8.0-v33
  - 7.14.0-v19
  - 7.2.0-v33
  - 7.3.2-v33
  - 8.2.0-v16
  enterprise: v0.37.0
  etcd:
  - 3.5.0-v20
  installer: v2024.12.18
  kubedump:
  - 0.1.0-v16
  mariadb:
  - 10.5.8-v27
  mongodb:
  - 3.4.17-v34
  - 3.4.22-v34
  - 3.6.13-v34
  - 3.6.8-v34
  - 4.0.11-v34
  - 4.0.3-v34
  - 4.0.5-v34
  - 4.1.13-v34
  - 4.1.4-v34
  - 4.1.7-v34
  - 4.2.3-v34
  - 4.4.6-v25
  - 5.0.15-v7
  - 5.0.3-v22
  - 6.0.5-v10
  mysql:
  - 5.7.25-v34
  - 8.0.14-v33
  - 8.0.21-v27
  - 8.0.3-v33
  nats:
  - 2.6.1-v21
  - 2.8.2-v16
  perconaxtradb:
  - 5.7-v28
  postgres:
  - 10.14-v32
  - 11.9-v32
  - 12.4-v32
  - 13.1-v29
  - 14.0-v21
  - 15.1-v13
  - 16.1-v2
  - "17.2"
  - 9.6.19-v32
  redis:
  - 5.0.13-v21
  - 6.2.5-v21
  - 7.0.5-v14
  ui-server: v0.18.0
  vault:
  - 1.10.3-v13
  version: v2024.12.18
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
