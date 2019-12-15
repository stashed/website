---
title: Workloads | Stash
description: workloads of Stash
menu:
  docs_0.6.0:
    identifier: workloads-stash
    name: Workloads
    parent: guides
    weight: 20
product_name: stash
menu_name: docs_0.6.0
section_menu_id: guides
info:
  version: 0.6.0
---

> New to Stash? Please start [here](/docs/0.6.0/concepts/README).

# Supported Workloads

Stash supports the following types of Kubernetes workloads.

## Deployments
To backup a Deployment, create a Restic with matching selectors. You can find a full working demo in [examples folder](/docs/0.6.0/examples/workloads/deployment.yaml).

## ReplicaSets
To backup a ReplicaSet, create a Restic with matching selectors. You can find a full working demo in [examples folder](/docs/0.6.0/examples/workloads/replicaset.yaml).

## ReplicationControllers
To backup a ReplicationController, create a Restic with matching selectors. You can find a full working demo in [examples folder](/docs/0.6.0/examples/workloads/rc.yaml).

## DaemonSets
To backup a DaemonSet, create a Restic with matching selectors. You can find a full working demo in [examples folder](/docs/0.6.0/examples/workloads/daemonset.yaml). This example shows how Stash can be used to backup host paths on all nodes of a cluster. First run a DaemonSet without nodeSelectors. This DaemonSet acts as a vector for Restic sidecar and mounts host paths that are to be backed up. In this example, we use a `busybox` container for this. Now, create a Restic that has a matching selector. This Restic also `spec.volumeMounts` the said host path and points to the host path in `spec.fileGroups`.

## StatefulSets
Kubernetes does not support updating StatefulSet after they are created. It is recomanded to use initializer for StatefulSets. For details see [here](/docs/0.6.0/initializer).
Otherwise you need to add Stash sidecar container to your StatefulSet manually. You can see the relevant portions of a working example below:

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
      containers:
      - command:
        - sleep
        - "3600"
        image: busybox
        imagePullPolicy: IfNotPresent
        name: busybox
        resources: {}
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
        volumeMounts:
        - mountPath: /source/data
          name: source-data
      - args:
        - backup
        - --restic-name=stash-demo
        - --workload-kind=Statefulset
        - --workload-name=stash-demo
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
        image: appscode/stash:0.6.0
        imagePullPolicy: IfNotPresent
        name: stash
        resources: {}
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
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
      restartPolicy: Always
      volumes:
      volumes:
      - gitRepo:
          repository: https://github.com/appscode/stash-data.git
        name: source-data
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
      - hostPath:
          path: /data/stash-test/restic-repo
          type: ""
        name: stash-local
```

You can find the full working demo in [examples folder](/docs/0.6.0/examples/workloads/statefulset.yaml). The section you should change for your own StatefulSet are:

 - `--restic-name` flag should be set to the name of the Restic used as configuration.
 - `--workload-kind` flag specifies the kind of workload (Deployment/Replicaset/RepliationController/DaemonSet/StatefulSet).
 - `--workload-name` flag specifies the name of workload where sidecar pod is added.

To learn about the meaning of various flags, please visit [here](/docs/0.6.0/reference/stash_backup).

## Next Steps

- Learn how to use Stash to backup a Kubernetes deployment [here](/docs/0.6.0/guides/backup).
- Learn about the details of Restic CRD [here](/docs/0.6.0/concepts/crds/restic).
- To restore a backup see [here](/docs/0.6.0/guides/restore).
- Learn about the details of Recovery CRD [here](/docs/0.6.0/concepts/crds/recovery).
- To run backup in offline mode see [here](/docs/0.6.0/guides/offline_backup)
- See the list of supported backends and how to configure them [here](/docs/0.6.0/guides/backends).
- Thinking about monitoring your backup operations? Stash works [out-of-the-box with Prometheus](/docs/0.6.0/guides/monitoring).
- Learn about how to configure [RBAC roles](/docs/0.6.0/guides/rbac).
- Learn about how to configure Stash operator as workload initializer [here](/docs/0.6.0/guides/initializer).
- Wondering what features are coming next? Please visit [here](/ROADMAP.md).
- Want to hack on Stash? Check our [contribution guidelines](/docs/0.6.0/CONTRIBUTING).
