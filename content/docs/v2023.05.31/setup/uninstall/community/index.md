---
title: Uninstall Stash Community Edition
description: Uninstallation guide for Stash Community edition
menu:
  docs_v2023.05.31:
    identifier: uninstall-stash-community
    name: Community Edition
    parent: uninstallation-guide
    weight: 10
product_name: stash
menu_name: docs_v2023.05.31
section_menu_id: setup
info:
  cli: v0.30.0
  community: v0.30.0
  elasticsearch:
  - 5.6.4-v26
  - 6.2.4-v26
  - 6.3.0-v26
  - 6.4.0-v26
  - 6.5.3-v26
  - 6.8.0-v26
  - 7.14.0-v12
  - 7.2.0-v26
  - 7.3.2-v26
  - 8.2.0-v9
  enterprise: v0.30.0
  etcd:
  - 3.5.0-v13
  installer: v2023.05.31
  kubedump:
  - 0.1.0-v9
  mariadb:
  - 10.5.8-v19
  mongodb:
  - 3.4.17-v26
  - 3.4.22-v26
  - 3.6.13-v26
  - 3.6.8-v26
  - 4.0.11-v26
  - 4.0.3-v26
  - 4.0.5-v26
  - 4.1.13-v26
  - 4.1.4-v26
  - 4.1.7-v26
  - 4.2.3-v26
  - 4.4.6-v17
  - 5.0.3-v14
  - 6.0.5-v2
  mysql:
  - 5.7.25-v26
  - 8.0.14-v26
  - 8.0.21-v20
  - 8.0.3-v26
  nats:
  - 2.6.1-v14
  - 2.8.2-v9
  percona-xtradb:
  - 5.7-v21
  postgres:
  - 10.14-v25
  - 11.9-v25
  - 12.4-v25
  - 13.1-v22
  - 14.0-v14
  - 15.1-v6
  - 9.6.19-v25
  redis:
  - 5.0.13-v14
  - 6.2.5-v14
  - 7.0.5-v7
  ui-server: v0.12.0
  vault:
  - 1.10.3-v6
  version: v2023.05.31
---

# Uninstall Stash Community Edition

To uninstall Stash Community edition, run the following command:

<ul class="nav nav-tabs" id="installerTab" role="tablist">
  <li class="nav-item">
    <a class="nav-link active" id="helm3-tab" data-toggle="tab" href="#helm3" role="tab" aria-controls="helm3" aria-selected="true">Helm 3</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" id="script-tab" data-toggle="tab" href="#script" role="tab" aria-controls="script" aria-selected="false">YAML</a>
  </li>
</ul>
<div class="tab-content" id="installerTabContent">
  <div class="tab-pane fade show active" id="helm3" role="tabpanel" aria-labelledby="helm3-tab">

### Using Helm 3

In Helm 3, release names are [scoped to a namespace](https://v3.helm.sh/docs/faq/#release-names-are-now-scoped-to-the-namespace). So, provide the namespace you used to install the operator when installing.

```bash
$ helm uninstall stash --namespace kube-system
```

</div>
<div class="tab-pane fade" id="script" role="tabpanel" aria-labelledby="script-tab">

### Using YAML (with helm 3)

If you prefer to not use Helm, you can generate YAMLs from Stash chart and uninstall using `kubectl`.

```bash
$ helm template stash appscode/stash -n kube-system \
--set features.community=true                       \
--set global.license="nothing"                      \
--set global.skipCleaner=true | kubectl delete -f -
```

</div>
</div>

## Delete CRDs

The above uninstallation process will uninstall the Stash operator. However, it will keep the Stash registered CRDs so that you don't lose your Stash objects i.e. `BackupConfiguration`, `Repository`, etc. during re-installation. If you want to remove the Stash CRDs too, please run the following command.

```bash
kubectl delete crd -l=app.kubernetes.io/name=stash
```

If you wan't to delete the `AppBinding` CRD, run the following command.

```bash
 kubectl delete crd -l=app.kubernetes.io/name=catalog
```
