---
title: Troubleshooting Stash Installation
description: Troubleshooting guide for Stash installation
menu:
  docs_v2025.6.30:
    identifier: install-stash-troubleshoot
    name: Troubleshooting
    parent: installation-guide
    weight: 40
product_name: stash
menu_name: docs_v2025.6.30
section_menu_id: setup
info:
  cli: v0.40.0
  community: v0.40.0
  elasticsearch:
  - 5.6.4-v36
  - 6.2.4-v36
  - 6.3.0-v36
  - 6.4.0-v36
  - 6.5.3-v36
  - 6.8.0-v36
  - 7.14.0-v22
  - 7.2.0-v36
  - 7.3.2-v36
  - 8.2.0-v19
  enterprise: v0.40.0
  etcd:
  - 3.5.0-v23
  installer: v2025.6.30
  kubedump:
  - 0.2.0-v4
  mariadb:
  - 10.5.8-v30
  mongodb:
  - 3.4.17-v37
  - 3.4.22-v37
  - 3.6.13-v37
  - 3.6.8-v37
  - 4.0.11-v37
  - 4.0.3-v37
  - 4.0.5-v37
  - 4.1.13-v37
  - 4.1.4-v37
  - 4.1.7-v37
  - 4.2.3-v37
  - 4.4.6-v28
  - 5.0.15-v10
  - 5.0.3-v25
  - 6.0.5-v13
  mysql:
  - 5.7.25-v37
  - 8.0.14-v36
  - 8.0.21-v30
  - 8.0.3-v36
  nats:
  - 2.6.1-v24
  - 2.8.2-v19
  percona-xtradb:
  - 5.7-v31
  postgres:
  - 10.14-v35
  - 11.9-v35
  - 12.4-v35
  - 13.1-v32
  - 14.0-v24
  - 15.1-v16
  - 16.1-v5
  - 17.2-v3
  - 9.6.19-v35
  redis:
  - 5.0.13-v24
  - 6.2.5-v24
  - 7.0.5-v17
  ui-server: v0.21.0
  vault:
  - 1.10.3-v16
  version: v2025.6.30
---

## Installing in GKE Cluster

If you are installing Stash on a GKE cluster, you will need cluster admin permissions to install Stash operator. Run the following command to grant admin permision to the cluster.

```bash
$ kubectl create clusterrolebinding "cluster-admin-$(whoami)" \
  --clusterrole=cluster-admin                                 \
  --user="$(gcloud config get-value core/account)"
```

In addition, if your GKE cluster is a [private cluster](https://cloud.google.com/kubernetes-engine/docs/how-to/private-clusters), you will need to either add an additional firewall rule that allows master nodes access port `8443/tcp` on worker nodes, or change the existing rule that allows access to ports `443/tcp` and `10250/tcp` to also allow access to port `8443/tcp`. The procedure to add or modify firewall rules is described in the official GKE documentation for private clusters mentioned before.

## Configuring Network Volume Accessor

For network volume such as NFS, Stash needs to deploy a helper deployment in the same namespace as the Repository that uses the NFS as backend to provide Snapshot listing facility. We call this helper deployment network volume accessor. You can configure its resources, user id, privileged permission etc. Run the following command to enable network volume accessor,


<ul class="nav nav-tabs" id="installerTab" role="tablist">
  <li class="nav-item">
    <a class="nav-link active" id="new-installer-tab" data-toggle="tab" href="#new-installation-tab" role="tab" aria-controls="new-installation-tab" aria-selected="true">New Installation</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" id="existing-installation" data-toggle="tab" href="#existing-installation-tab" role="tab" aria-controls="existing-installation-tab" aria-selected="false">Existing Installation</a>
  </li>
</ul>
<div class="tab-content" id="installerTabContent">
  <div class="tab-pane fade show active" id="new-installation-tab" role="tabpanel" aria-labelledby="new-installation-tab">

### New Installation

If you haven't installed Stash yet, run the following command to configure the network volume accessor during installation

```bash
$ helm upgrade -i stash oci://ghcr.io/appscode-charts/stash \
  --version {{< param "info.version" >}} \
  --namespace stash --create-namespace \
  --set features.enterprise=true \
  --set stash-enterprise.netVolAccessor.cpu=200m \
  --set stash-enterprise.netVolAccessor.memory=128Mi \
  --set stash-enterprise.netVolAccessor.runAsUser=0 \
  --set stash-enterprise.netVolAccessor.privileged=true \
  --set-file global.license=/path/to/license-file.txt \
  --wait --burst-limit=10000 --debug
```

</div>
<div class="tab-pane fade" id="existing-installation-tab" role="tabpanel" aria-labelledby="existing-installation-tab">

### Existing Installation

If you have installed Stash already in your cluster but didn't configure the network volume accessor, you can use `helm upgrade` command to configure it in the existing installation.

```bash
$ helm upgrade -i stash oci://ghcr.io/appscode-charts/stash \
  --version {{< param "info.version" >}} \
  --namespace stash --create-namespace \
  --reuse-values \
  --set features.enterprise=true \
  --set stash-enterprise.netVolAccessor.cpu=200m \
  --set stash-enterprise.netVolAccessor.memory=128Mi \
  --set stash-enterprise.netVolAccessor.runAsUser=0 \
  --set stash-enterprise.netVolAccessor.privileged=true \
  --set-file global.license=/path/to/license-file.txt \
  --wait --burst-limit=10000 --debug
```
</div>
</div>



## Detect Stash version

To detect Stash version, exec into the operator pod and run `stash version` command.

```bash
$ POD_NAMESPACE=kube-system
$ POD_NAME=$(kubectl get pods -n $POD_NAMESPACE -l app.kubernetes.io/name=stash-community -o jsonpath={.items[0].metadata.name})
$ kubectl exec $POD_NAME -c operator -n $POD_NAMESPACE -- /stash version

Version = {{< param "info.version" >}}
VersionStrategy = tag
Os = alpine
Arch = amd64
CommitHash = 85b0f16ab1b915633e968aac0ee23f877808ef49
GitBranch = release-0.5
GitTag = {{< param "info.version" >}}
CommitTimestamp = 2020-08-10T05:24:23

$ kubectl exec -it $POD_NAME -c operator -n $POD_NAMESPACE restic version
restic 0.9.6
compiled with go1.9 on linux/amd64
```
