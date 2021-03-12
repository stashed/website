---
title: Uninstall Percona XtraDB Addon | Stash
description: An guide on how to uninstall Percona XtraDB addon for Stash
menu:
  docs_v2021.03.11:
    identifier: stash-percona-xtradb-uninstall
    name: Uninstall
    parent: stash-percona-xtradb-setup
    weight: 20
product_name: stash
menu_name: docs_v2021.03.11
section_menu_id: stash-addons
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

{{< notice type="warning" message="This is an Enterprise-only feature. Please install [Stash Enterprise Edition](/docs/v2021.03.11/setup/install/enterprise) to try this feature." >}}

# Uninstall Percona XtraDB addon for Stash

In order to uninstall Percona XtraDB addon, follow the instruction given below.

<ul class="nav nav-tabs" id="installerTab" role="tablist">
  <li class="nav-item">
    <a class="nav-link active" id="helm3-tab" data-toggle="tab" href="#helm3" role="tab" aria-controls="helm3" aria-selected="true">Helm 3</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" id="helm2-tab" data-toggle="tab" href="#helm2" role="tab" aria-controls="helm2" aria-selected="false">Helm 2</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" id="script-tab" data-toggle="tab" href="#script" role="tab" aria-controls="script" aria-selected="false">YAML</a>
  </li>
</ul>
<div class="tab-content" id="installerTabContent">
  <div class="tab-pane fade show active" id="helm3" role="tabpanel" aria-labelledby="helm3-tab">

## Using Helm 3

Run the following script to uninstall `stash-percona-xtradb` addon that was installed as a Helm chart using Helm 3.

```bash
curl -fsSL https://github.com/stashed/catalog/raw/{{< param "info.catalog" >}}/deploy/helm3.sh | bash -s -- --uninstall --catalog=stash-percona-xtradb
```

</div>
<div class="tab-pane fade" id="helm2" role="tabpanel" aria-labelledby="helm2-tab">

## Using Helm 2

Run the following script to uninstall `stash-percona-xtradb` addon that was installed as a Helm chart using Helm 2.

```bash
curl -fsSL https://github.com/stashed/catalog/raw/{{< param "info.catalog" >}}/deploy/helm2.sh | bash -s -- --uninstall --catalog=stash-percona-xtradb
```

</div>
<div class="tab-pane fade" id="script" role="tabpanel" aria-labelledby="script-tab">

## Using YAML

Run the following script to uninstall `stash-percona-xtradb` addon that was installed as Kubernetes YAMLs.

```bash
curl -fsSL https://github.com/stashed/catalog/raw/{{< param "info.catalog" >}}/deploy/script.sh | bash -s -- --uninstall --catalog=stash-percona-xtradb
```

</div>
</div>

## Customizing Uninstaller

In order to uninstall Percona XtraDB addon only for a specific database version, use `--version` flag to specify the desired version.

```bash
curl -fsSL https://github.com/stashed/catalog/raw/{{< param "info.catalog" >}}/deploy/helm3.sh | bash -s -- --uninstall --catalog=stash-percona-xtradb --version=5.7
```
