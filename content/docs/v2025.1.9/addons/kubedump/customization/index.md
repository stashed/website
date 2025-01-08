---
title: Backup Customization | Stash
description: Customizing Backup Process with Stash
menu:
  docs_v2025.1.9:
    identifier: stash-kubedump-customization
    name: Customizing Backup Process
    parent: stash-kubedump
    weight: 50
product_name: stash
menu_name: docs_v2025.1.9
section_menu_id: stash-addons
info:
  cli: v0.38.0
  community: v0.38.0
  elasticsearch:
  - 5.6.4-v34
  - 6.2.4-v34
  - 6.3.0-v34
  - 6.4.0-v34
  - 6.5.3-v34
  - 6.8.0-v34
  - 7.14.0-v20
  - 7.2.0-v34
  - 7.3.2-v34
  - 8.2.0-v17
  enterprise: v0.38.0
  etcd:
  - 3.5.0-v21
  installer: v2025.1.9
  kubedump:
  - 0.2.0-v2
  mariadb:
  - 10.5.8-v28
  mongodb:
  - 3.4.17-v35
  - 3.4.22-v35
  - 3.6.13-v35
  - 3.6.8-v35
  - 4.0.11-v35
  - 4.0.3-v35
  - 4.0.5-v35
  - 4.1.13-v35
  - 4.1.4-v35
  - 4.1.7-v35
  - 4.2.3-v35
  - 4.4.6-v26
  - 5.0.15-v8
  - 5.0.3-v23
  - 6.0.5-v11
  mysql:
  - 5.7.25-v35
  - 8.0.14-v34
  - 8.0.21-v28
  - 8.0.3-v34
  nats:
  - 2.6.1-v22
  - 2.8.2-v17
  percona-xtradb:
  - 5.7-v29
  postgres:
  - 10.14-v33
  - 11.9-v33
  - 12.4-v33
  - 13.1-v30
  - 14.0-v22
  - 15.1-v14
  - 16.1-v3
  - 17.2-v1
  - 9.6.19-v33
  redis:
  - 5.0.13-v22
  - 6.2.5-v22
  - 7.0.5-v15
  ui-server: v0.19.0
  vault:
  - 1.10.3-v14
  version: v2025.1.9
---

# Customizing Backup Process

Stash provides rich customization supports for the backup and restores process to meet the requirements of various cluster configurations. This guide will show you some examples of these customizations.

In this section, we are going to show you how to customize the backup process. Here, we are going to show some examples of filtering resources using a label selector, running the backup process as a specific user, using multiple retention policies, etc.

> Note: YAML files used in this tutorial are stored [here](https://github.com/stashed/docs/tree/{{< param "info.version" >}}/docs/addons/kubedump/customization/examples).

### Filtering resources

You can use a label selector to backup the YAML for the resources that have particular labels. You just have to pass the `labelSelector` parameter under `task.params` section with your desired label selector.

```yaml
apiVersion: stash.appscode.com/v1beta1
kind: BackupConfiguration
metadata:
  name: cluster-resources-backup
  namespace: demo
spec:
  schedule: "*/5 * * * *"
  task:
    name: kubedump-backup-0.1.0
    params:
      - name: labelSelector
        value: "k8s-app=kube-dns"
  repository:
    name: cluster-resource-storage
  runtimeSettings:
    pod:
      serviceAccountName: cluster-resource-reader
  retentionPolicy:
    name: keep-last-5
    keepLast: 5
    prune: true
```

The above backup process will backup only the resources that has `k8s-app: kube-dns` label. Here, is a sample of the resources backed up by the above BackupConfiguration.

```bash
❯ tree  $HOME/Downloads/stash
/home/emruz/Downloads/stash
└── latest
    └── tmp
        └── resources
            └── namespaces
                └── kube-system
                    ├── Deployment
                    │   └── coredns.yaml
                    ├── Endpoints
                    │   └── kube-dns.yaml
                    ├── EndpointSlice
                    │   └── kube-dns-m2s5c.yaml
                    ├── Pod
                    │   ├── coredns-558bd4d5db-hdsw9.yaml
                    │   └── coredns-558bd4d5db-wk9tx.yaml
                    ├── ReplicaSet
                    │   └── coredns-558bd4d5db.yaml
                    └── Service
                        └── kube-dns.yaml

11 directories, 7 files
```

### Passing arguments

You can pass arguments to the backup process using `task.params` section.

The following example shows how passes `sanitize` argument value to `false` which tells the backup process not to remove decorators (i.e. `status`, `managedFields` etc.) from the YAML files.

```yaml
apiVersion: stash.appscode.com/v1beta1
kind: BackupConfiguration
metadata:
  name: cluster-resources-backup
  namespace: demo
spec:
  schedule: "*/5 * * * *"
  task:
    name: kubedump-backup-0.1.0
    params:
      - name: sanitize
        value: "false"
  repository:
    name: cluster-resource-storage
  runtimeSettings:
    pod:
      serviceAccountName: cluster-resource-reader
  retentionPolicy:
    name: keep-last-5
    keepLast: 5
    prune: true
```

### Running backup job as a specific user

If your cluster requires running the backup job as a specific user, you can provide `securityContext` under `runtimeSettings.pod` section. The below example shows how you can run the backup job as the root user.

```yaml
apiVersion: stash.appscode.com/v1beta1
kind: BackupConfiguration
metadata:
  name: cluster-resources-backup
  namespace: demo
spec:
  schedule: "*/5 * * * *"
  task:
    name: kubedump-backup-0.1.0
  repository:
    name: cluster-resource-storage
  runtimeSettings:
    pod:
      serviceAccountName: cluster-resource-reader
      securityContext:
        runAsUser: 0
        runAsGroup: 0
  retentionPolicy:
    name: keep-last-5
    keepLast: 5
    prune: true
```

### Specifying Memory/CPU limit/request for the backup job

If you want to specify the Memory/CPU limit/request for your backup job, you can specify `resources` field under `runtimeSettings.container` section.

```yaml
apiVersion: stash.appscode.com/v1beta1
kind: BackupConfiguration
metadata:
  name: cluster-resources-backup
  namespace: demo
spec:
  schedule: "*/5 * * * *"
  task:
    name: kubedump-backup-0.1.0
  repository:
    name: cluster-resource-storage
  runtimeSettings:
    pod:
      serviceAccountName: cluster-resource-reader
    container:
      resources:
        requests:
          cpu: "200m"
          memory: "1Gi"
        limits:
          cpu: "200m"
          memory: "1Gi"
  retentionPolicy:
    name: keep-last-5
    keepLast: 5
    prune: true
```

### Using multiple retention policies

You can also specify multiple retention policies for your backed up data. For example, you may want to keep a few daily snapshots, a few weekly snapshots, and few monthly snapshots, etc. You just need to pass the desired number with the respective key under the `retentionPolicy` section.

```yaml
apiVersion: stash.appscode.com/v1beta1
kind: BackupConfiguration
metadata:
  name: cluster-resources-backup
  namespace: demo
spec:
  schedule: "*/5 * * * *"
  task:
    name: kubedump-backup-0.1.0
  repository:
    name: cluster-resource-storage
  runtimeSettings:
    pod:
      serviceAccountName: cluster-resource-reader
  retentionPolicy:
    name: cluster-resource-retention
    keepLast: 5
    keepDaily: 10
    keepWeekly: 20
    keepMonthly: 50
    keepYearly: 100
    prune: true
```

To know more about the available options for retention policies, please visit [here](/docs/v2025.1.9/concepts/crds/backupconfiguration/#specretentionpolicy).
