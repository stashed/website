---
title: Workloads | Stash
description: workloads of Stash
menu:
  docs_v2021.03.11:
    identifier: workloads-stash
    name: Workloads
    parent: v1alpha1-guides
    weight: 25
product_name: stash
menu_name: docs_v2021.03.11
section_menu_id: guides
info:
  catalog: v2021.03.11
  cli: v0.11.11
  community: v0.11.11
  elasticsearch:
  - 5.6.4-v7
  - 6.2.4-v7
  - 6.3.0-v7
  - 6.4.0-v7
  - 6.5.3-v7
  - 6.8.0-v7
  - 7.2.0-v7
  - 7.3.2-v7
  enterprise: v0.11.11
  installer: v0.11.11
  mariadb:
  - 10.5.8-v1
  mongodb:
  - 3.4.17-v6
  - 3.4.22-v6
  - 3.6.13-v6
  - 3.6.8-v6
  - 4.0.11-v6
  - 4.0.3-v6
  - 4.0.5-v6
  - 4.1.13-v6
  - 4.1.4-v6
  - 4.1.7-v6
  - 4.2.3-v6
  mysql:
  - 5.7.25-v7
  - 8.0.14-v7
  - 8.0.21-v1
  - 8.0.3-v7
  percona-xtradb:
  - 5.7.0-v2
  postgres:
  - 10.14.0-v5
  - 11.9.0-v5
  - 12.4.0-v5
  - 13.1.0-v2
  - 9.6.19-v5
  version: v2021.03.11
---

> New to Stash? Please start [here](/docs/v2021.03.11/concepts/README).

# Supported Workloads

Stash supports the following types of Kubernetes workloads.

## Deployments
To backup a Deployment, create a Restic with matching selectors. You can find a full working demo in [examples folder](/docs/v2021.03.11/examples/workloads/deployment.yaml).

## ReplicaSets
To backup a ReplicaSet, create a Restic with matching selectors. You can find a full working demo in [examples folder](/docs/v2021.03.11/examples/workloads/replicaset.yaml).

## ReplicationControllers
To backup a ReplicationController, create a Restic with matching selectors. You can find a full working demo in [examples folder](/docs/v2021.03.11/examples/workloads/rc.yaml).

## DaemonSets
To backup a DaemonSet, create a Restic with matching selectors. You can find a full working demo in [examples folder](/docs/v2021.03.11/examples/workloads/daemonset.yaml). This example shows how Stash can be used to backup host paths on all nodes of a cluster. First run a DaemonSet without nodeSelectors. This DaemonSet acts as a vector for Restic sidecar and mounts host paths that are to be backed up. In this example, we use a `busybox` container for this. Now, create a Restic that has a matching selector. This Restic also `spec.volumeMounts` the said host path and points to the host path in `spec.fileGroups`.

## StatefulSets
Kubernetes does not support adding sidecar to a StatefulSet after it is created. It is recommended to enable **mutating webhook** when installing stash. To know more about how to provide various flag while installing Stash.

If you don't want to enable **mutating webhook** then you have to add Stash sidecar container to your StatefulSet manually. You can see the relevant portions of a working example below:

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  labels:
    app: statefulset-demo
  name: workload
  namespace: default
spec:
  replicas: 1
  serviceName: headless
  selector:
    matchLabels:
      app: statefulset-demo
  template:
    metadata:
      labels:
        app: statefulset-demo
      name: busybox
    spec:
      serviceAccountName: statefulset-demo
      containers:
      - image: busybox
        name: busybox
        imagePullPolicy: IfNotPresent
        command:
        - sleep
        - "3600"
        resources: {}
        volumeMounts:
        - mountPath: /source/data
          name: source-data
      - image: appscode/stash:{{< param "info.version" >}}
        name: stash
        imagePullPolicy: IfNotPresent
        args:
        - backup
        - --restic-name=statefulset-restic
        - --workload-kind=Statefulset
        - --workload-name=workload
        - --run-via-cron=true
        - --v=3
        env:
        - name: NODE_NAME
          valueFrom:
            fieldRef:
              apiVersion: v1
              fieldPath: spec.nodeName
        - name: POD_NAME
          valueFrom:
            fieldRef:
              apiVersion: v1
              fieldPath: metadata.name
        resources: {}
        volumeMounts:
        - mountPath: /tmp
          name: stash-scratchdir
        - mountPath: /etc/stash
          name: stash-podinfo
        - mountPath: /source/data
          name: source-data
          readOnly: true
        - mountPath: /safe/data
          name: stash-local
      volumes:
      - gitRepo:
          repository: https://github.com/stashed/stash-data.git
        name: source-data
      - hostPath:
          path: /data/stash-test/restic-repo
          type: ""
        name: stash-local
      - emptyDir: {}
        name: stash-scratchdir
      - downwardAPI:
          defaultMode: 420
          items:
          - fieldRef:
              apiVersion: v1
              fieldPath: metadata.labels
            path: labels
        name: stash-podinfo
```

You can find the full working demo in [examples folder](/docs/v2021.03.11/examples/workloads/statefulset.yaml). The section you should change for your own StatefulSet are:

 - `--restic-name` flag should be set to the name of the Restic used as configuration.
 - `--workload-kind` flag specifies the kind of workload (Deployment/Replicaset/RepliationController/DaemonSet/StatefulSet).
 - `--workload-name` flag specifies the name of workload where sidecar pod is added.

## Next Steps

- Learn how to use Stash to backup a Kubernetes deployment [here](/docs/v2021.03.11/guides/v1alpha1/backup).
- Learn about the details of Restic CRD [here](/docs/v2021.03.11/concepts/crds/v1alpha1/restic).
- To restore a backup see [here](/docs/v2021.03.11/guides/v1alpha1/restore).
- Learn about the details of Recovery CRD [here](/docs/v2021.03.11/concepts/crds/v1alpha1/recovery).
- To run backup in offline mode see [here](/docs/v2021.03.11/guides/v1alpha1/offline_backup)
- See the list of supported backends and how to configure them [here](/docs/v2021.03.11/guides/v1alpha1/backends/overview).
- Thinking about monitoring your backup operations? Stash works [out-of-the-box with Prometheus](/docs/v2021.03.11/guides/v1alpha1/monitoring/overview).
- Learn about how to configure [RBAC roles](/docs/v2021.03.11/guides/v1alpha1/rbac).
