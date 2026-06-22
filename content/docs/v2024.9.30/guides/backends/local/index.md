---
title: Local Backend | Stash
description: Configure Stash to Use Local Backend.
menu:
  docs_v2024.9.30:
    identifier: backend-local
    name: Kubernetes Volumes
    parent: backend
    weight: 20
product_name: stash
menu_name: docs_v2024.9.30
section_menu_id: guides
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
  percona-xtradb:
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

# Local Backend

`Local` backend refers to a local path inside `stash` sidecar container. Any Kubernetes supported [persistent volume](https://kubernetes.io/docs/concepts/storage/volumes/) such as [PersistentVolumeClaim](https://kubernetes.io/docs/concepts/storage/volumes/#persistentvolumeclaim), [HostPath](https://kubernetes.io/docs/concepts/storage/volumes/#hostpath), [EmptyDir](https://kubernetes.io/docs/concepts/storage/volumes/#emptydir) (for testing only), [NFS](https://kubernetes.io/docs/concepts/storage/volumes/#nfs),  [gcePersistentDisk](https://kubernetes.io/docs/concepts/storage/volumes/#gcepersistentdisk) etc. can be used as local backend.

In order to use Kubernetes volumes as backend, you have to create a `Secret` and a `Repository` object pointing to the desired volume.

### Create Storage Secret

To configure storage secret for local backend, following secret keys are needed:

| Key               | Type       | Description                                                |
| ----------------- | ---------- | ---------------------------------------------------------- |
| `RESTIC_PASSWORD` | `Required` | Password that will be used to encrypt the backup snapshots |

Create storage secret as below,

```bash
$ echo -n 'changeit' > RESTIC_PASSWORD
$ kubectl create secret generic -n demo local-secret --from-file=./RESTIC_PASSWORD
secret/local-secret created
```

### Create Repository

Now, you have to create a `Repository` crd that uses Kubernetes volume as a backend. You have to provide the storage secret that we have created earlier in `spec.backend.storageSecretName` field.

Following parameters are available for `Local` backend.

| Parameter            | Type       | Description                                                                                                                                                                                                                              |
| -------------------- | ---------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `local.mountPath`    | `Required` | Path where this volume will be mounted inside the sidecar container. Example: `/safe/data`. <br> <strong>We have put `stash` binary  in the root directory. Hence, you can not use `/stash` or `/stash/*` as `local.mountPath` </strong> |
| `local.subPath`      | `Optional` | Sub-path inside the referenced volume where the backed up snapshot will be stored instead of its root.                                                                                                                                   |
| `local.VolumeSource` | `Required` | Any Kubernetes volume. Can be specified inlined. Example: `hostPath`.                                                                                                                                                                    |

Here, we are going to show some sample `Repository` crds that uses different Kubernetes volume as a backend.

##### HostPath volume as Backend

Below, the YAML of a sample `Repository` crd that uses a `hostPath` volume as a backend.

```yaml
apiVersion: stash.appscode.com/v1alpha1
kind: Repository
metadata:
  name: local-repo-with-hostpath
  namespace: demo
spec:
  backend:
    local:
      mountPath: /safe/data
      hostPath:
        path: /data/stash-test/repo
    storageSecretName: local-secret
```

Create the `Repository` we have shown above using the following command,

```bash
$ kubectl apply -f https://github.com/stashed/docs/raw/{{< param "info.version" >}}/docs/guides/backends/local/examples/hostPath.yaml
repository/local-repo-with-hostpath created
```

>Note that by default, Stash runs as `non-root` user. `hostPath` volume is writable only for `root` user. So, in order to use `hostPath` volume as backend, either you have to run Stash as `root` user using securityContext or you have to change the permission of the `hostPath` to make it writable for `non-root` users.

##### PersistentVolumeClaim as Backend

Below, the YAML of a sample `Repository` crd that uses a `PersistentVolumeClaim` as a backend.

```yaml
apiVersion: stash.appscode.com/v1alpha1
kind: Repository
metadata:
  name: local-repo-with-pvc
  namespace: demo
spec:
  backend:
    local:
      mountPath: /safe/data
      persistentVolumeClaim:
        claimName: repo-pvc
    storageSecretName: local-secret
```

Create the `Repository` we have shown above using the following command,

```bash
$ kubectl apply -f https://github.com/stashed/docs/raw/{{< param "info.version" >}}/docs/guides/backends/local/examples/pvc.yaml
repository/local-repo-with-pvc created
```

##### NFS volume as Backend

Below, the YAML of a sample `Repository` crd that uses an `NFS` volume as a backend.

```yaml
apiVersion: stash.appscode.com/v1alpha1
kind: Repository
metadata:
  name: local-repo-with-nfs
  namespace: demo
spec:
  backend:
    local:
      mountPath: /safe/data
      nfs:
        server: "nfs-service.storage.svc.cluster.local" # use you own NFS server address
        path: "/" # this path is relative to "/exports" path of NFS server
    storageSecretName: local-secret
```

Create the `Repository` we have shown above using the following command,

```bash
$ kubectl apply -f https://github.com/stashed/docs/raw/{{< param "info.version" >}}/docs/guides/backends/local/examples/nfs.yaml
repository/local-repo-with-nfs created
```

>For NFS backend, Stash may have to run the network volume accessor deployments in privileged mode to provide Snapshot listing facility. In this case, please configure network volume accessors by following the instruction [here](/docs/v2024.9.30/setup/install/troubleshooting/#configuring-network-volume-accessor).

##### GCE PersitentDisk as Backend

Below, the YAML of a sample `Repository` crd that uses a [gcePersistentDisk](https://kubernetes.io/docs/concepts/storage/volumes/#gcepersistentdisk) as a backend.

```yaml
apiVersion: stash.appscode.com/v1alpha1
kind: Repository
metadata:
  name: local-repo-with-gcepersistentdisk
  namespace: demo
spec:
  backend:
    local:
      mountPath: /safe/data
      gcePersistentDisk:
        pdName: stash-repo
        fsType: ext4
    storageSecretName: local-secret
```

Create the `Repository` we have shown above using the following command,

```bash
$ kubectl apply -f https://github.com/stashed/docs/raw/{{< param "info.version" >}}/docs/guides/backends/local/examples/gcePersistentDisk.yaml
repository/local-repo-with-gcepersistentdisk created
```

>In order to use `gcePersistentDisk` volume as backend, the node where stash container is running must be a GCE VM and the VM must be in same GCE project and zone as the Persistent Disk.

##### AWS EBS volume as Backend

Below, the YAML of a sample `Repository` crd that uses an [awsElasticBlockStore](https://kubernetes.io/docs/concepts/storage/volumes/#awselasticblockstore) as a backend.

```yaml
apiVersion: stash.appscode.com/v1alpha1
kind: Repository
metadata:
  name: local-repo-with-awsebs
  namespace: demo
spec:
  backend:
    local:
      mountPath: /safe/data
      awsElasticBlockStore: # This AWS EBS volume must already exist.
        volumeID: <volume-id>
        fsType: ext4
    storageSecretName: local-secret
```

Create the `Repository` we have shown above using the following command,

```bash
$ kubectl apply -f https://github.com/stashed/docs/raw/{{< param "info.version" >}}/docs/guides/backends/local/examples/awsElasticBlockStore.yaml
repository/local-repo-with-awsebs created
```

>In order to use `awsElasticBlockStore` volume as backend, the pod where stash container is running must be running on an AWS EC2 instance and the instance must be in the same region and availability-zone as the EBS volume.

##### Azure Disk as Backend

Below, the YAML of a sample `Repository` crd that uses an [azureDisk](https://kubernetes.io/docs/concepts/storage/volumes/#azuredisk) as a backend.

```yaml
apiVersion: stash.appscode.com/v1alpha1
kind: Repository
metadata:
  name: local-repo-with-azuredisk
  namespace: demo
spec:
  backend:
    local:
      mountPath: /safe/data
      azureDisk:
        diskName: stash.vhd
        diskURI: https://someaccount.blob.microsoft.net/vhds/stash.vhd
    storageSecretName: local-secret
```

Create the `Repository` we have shown above using the following command,

```bash
$ kubectl apply -f https://github.com/stashed/docs/raw/{{< param "info.version" >}}/docs/guides/backends/local/examples/azureDisk.yaml
repository/local-repo-with-azuredisk created
```

##### StorageOS as Backend

Below, the YAML of a sample `Repository` crd that uses a [storageOS](https://kubernetes.io/docs/concepts/storage/volumes/#storageos) volume as a backend.

```yaml
apiVersion: stash.appscode.com/v1alpha1
kind: Repository
metadata:
  name: local-repo-with-storageos
  namespace: demo
spec:
  backend:
    local:
      mountPath: /safe/data
      storageos:
        volumeName: stash-vol01 # The `stash-vol01` volume must already exist within StorageOS in the `demo` namespace.
        fsType: ext4
    storageSecretName: local-secret
```

Create the `Repository` we have shown above using the following command,

```bash
$ kubectl apply -f https://github.com/stashed/docs/raw/{{< param "info.version" >}}/docs/guides/backends/local/examples/storageOS.yaml
repository/local-repo-with-storageos created
```

##### EmptyDir volume as Backend

Below, the YAML of a sample `Repository` crd that uses an [emptyDir](https://kubernetes.io/docs/concepts/storage/volumes/#emptydir) as a backend.

```yaml
apiVersion: stash.appscode.com/v1alpha1
kind: Repository
metadata:
  name: local-repo-with-emptydir
  namespace: demo
spec:
  backend:
    local:
      mountPath: /safe/data
      emptyDir: {}
    storageSecretName: local-secret
```

Create the `Repository` we have shown above using the following command,

```bash
$ kubectl apply -f https://github.com/stashed/docs/raw/{{< param "info.version" >}}/docs/guides/backends/local/examples/emptyDir.yaml
repository/local-repo-with-emptydir created
```

>**Warning:** Data of an `emptyDir` volume is not persistent. If you delete the pod that runs the respective stash container, you will lose all the backed up data. You should use this kind of volumes only to test backup process.

## Next Steps

- Learn how to use Stash to backup workloads data from [here](/docs/v2024.9.30/guides/workloads/overview/).
- Learn how to use Stash to backup databases from [here](/docs/v2024.9.30/guides/addons/overview/).
- Learn how to use Stash to backup stand-alone PVC from [here](/docs/v2024.9.30/guides/volumes/overview/).
