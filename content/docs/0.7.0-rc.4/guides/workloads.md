---
title: Workloads | Stash
description: workloads of Stash
menu:
  docs_0.7.0-rc.4:
    identifier: workloads-stash
    name: Workloads
    parent: guides
    weight: 20
product_name: stash
menu_name: docs_0.7.0-rc.4
section_menu_id: guides
info:
  version: 0.7.0-rc.4
---

> New to Stash? Please start [here](/docs/0.7.0-rc.4/concepts/README).

# Supported Workloads

Stash supports the following types of Kubernetes workloads.

## Deployments
To backup a Deployment, create a Restic with matching selectors. You can find a full working demo in [examples folder](/docs/0.7.0-rc.4/examples/workloads/deployment.yaml).

## ReplicaSets
To backup a ReplicaSet, create a Restic with matching selectors. You can find a full working demo in [examples folder](/docs/0.7.0-rc.4/examples/workloads/replicaset.yaml).

## ReplicationControllers
To backup a ReplicationController, create a Restic with matching selectors. You can find a full working demo in [examples folder](/docs/0.7.0-rc.4/examples/workloads/rc.yaml).

## DaemonSets
To backup a DaemonSet, create a Restic with matching selectors. You can find a full working demo in [examples folder](/docs/0.7.0-rc.4/examples/workloads/daemonset.yaml). This example shows how Stash can be used to backup host paths on all nodes of a cluster. First run a DaemonSet without nodeSelectors. This DaemonSet acts as a vector for Restic sidecar and mounts host paths that are to be backed up. In this example, we use a `busybox` container for this. Now, create a Restic that has a matching selector. This Restic also `spec.volumeMounts` the said host path and points to the host path in `spec.fileGroups`.

## StatefulSets
Kubernetes does not support adding sidecar to a StatefulSet after it is created. It is recommended to enable **mutating webhook** by providing `--enable-mutating-webhook` flag while installing stash. To know more about how to provide various flag while installing stash see [here](/docs/0.7.0-rc.4/setup/install#customizing-installer).

If you don't want to enable **mutating webhook** then you have to add Stash sidecar container to your StatefulSet manually. You can see the relevant portions of a working example below:

```yaml
apiVersion: apps/v1beta1
kind: StatefulSet
metadata:
  labels:
    app: statefulset-demo
  name: workload
  namespace: default
spec:
  replicas: 1
  serviceName: headless
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
      - image: appscode/stash:0.7.0-rc.4
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
          repository: https://github.com/appscode/stash-data.git
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

You can find the full working demo in [examples folder](/docs/0.7.0-rc.4/examples/workloads/statefulset.yaml). The section you should change for your own StatefulSet are:

 - `--restic-name` flag should be set to the name of the Restic used as configuration.
 - `--workload-kind` flag specifies the kind of workload (Deployment/Replicaset/RepliationController/DaemonSet/StatefulSet).
 - `--workload-name` flag specifies the name of workload where sidecar pod is added.

To learn about the meaning of various flags, please visit [here](/docs/0.7.0-rc.4/reference/stash_backup).

## Next Steps

- Learn how to use Stash to backup a Kubernetes deployment [here](/docs/0.7.0-rc.4/guides/backup).
- Learn about the details of Restic CRD [here](/docs/0.7.0-rc.4/concepts/crds/restic).
- To restore a backup see [here](/docs/0.7.0-rc.4/guides/restore).
- Learn about the details of Recovery CRD [here](/docs/0.7.0-rc.4/concepts/crds/recovery).
- To run backup in offline mode see [here](/docs/0.7.0-rc.4/guides/offline_backup)
- See the list of supported backends and how to configure them [here](/docs/0.7.0-rc.4/guides/backends).
- Thinking about monitoring your backup operations? Stash works [out-of-the-box with Prometheus](/docs/0.7.0-rc.4/guides/monitoring).
- Learn about how to configure [RBAC roles](/docs/0.7.0-rc.4/guides/rbac).
- Wondering what features are coming next? Please visit [here](/docs/0.7.0-rc.4/roadmap).
- Want to hack on Stash? Check our [contribution guidelines](/docs/0.7.0-rc.4/CONTRIBUTING).
