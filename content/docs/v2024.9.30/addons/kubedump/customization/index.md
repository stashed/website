---
title: Backup Customization | Stash
description: Customizing Backup Process with Stash
menu:
  docs_v2024.9.30:
    identifier: stash-kubedump-customization
    name: Customizing Backup Process
    parent: stash-kubedump
    weight: 50
product_name: stash
menu_name: docs_v2024.9.30
section_menu_id: stash-addons
info:
  cli: v0.36.0
  community: v0.36.0
  elasticsearch:
  - 5.6.4-v32
  - 6.2.4-v32
  - 6.3.0-v32
  - 6.4.0-v32
  - 6.5.3-v32
  - 6.8.0-v32
  - 7.14.0-v18
  - 7.2.0-v32
  - 7.3.2-v32
  - 8.2.0-v15
  enterprise: v0.36.0
  etcd:
  - 3.5.0-v19
  installer: v2024.9.30
  kubedump:
  - 0.1.0-v15
  mariadb:
  - 10.5.8-v26
  mongodb:
  - 3.4.17-v33
  - 3.4.22-v33
  - 3.6.13-v33
  - 3.6.8-v33
  - 4.0.11-v33
  - 4.0.3-v33
  - 4.0.5-v33
  - 4.1.13-v33
  - 4.1.4-v33
  - 4.1.7-v33
  - 4.2.3-v33
  - 4.4.6-v24
  - 5.0.15-v6
  - 5.0.3-v21
  - 6.0.5-v9
  mysql:
  - 5.7.25-v32
  - 8.0.14-v32
  - 8.0.21-v26
  - 8.0.3-v32
  nats:
  - 2.6.1-v20
  - 2.8.2-v15
  perconaxtradb:
  - 5.7-v27
  postgres:
  - 10.14-v31
  - 11.9-v31
  - 12.4-v31
  - 13.1-v28
  - 14.0-v20
  - 15.1-v12
  - 16.1-v1
  - 9.6.19-v31
  redis:
  - 5.0.13-v20
  - 6.2.5-v20
  - 7.0.5-v13
  ui-server: v0.17.0
  vault:
  - 1.10.3-v12
  version: v2024.9.30
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

To know more about the available options for retention policies, please visit [here](/docs/v2024.9.30/concepts/crds/backupconfiguration/#specretentionpolicy).
