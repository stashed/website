---
title: Uninstall MySQL Addon | Stash
description: An guide on how to uninstall MySQL addon for Stash
menu:
  docs_v2020.11.06:
    identifier: stash-mysql-uninstall
    name: Uninstall
    parent: stash-mysql-setup
    weight: 20
product_name: stash
menu_name: docs_v2020.11.06
section_menu_id: stash-addons
info:
  catalog: v2020.11.06
  cli: v0.11.6
  community: v0.11.6
  elasticsearch:
  - 5.6.4-v4
  - 6.2.4-v4
  - 6.3.0-v4
  - 6.4.0-v4
  - 6.5.3-v4
  - 6.8.0-v4
  - 7.2.0-v4
  - 7.3.2-v4
  enterprise: v0.11.6
  installer: v0.11.6
  mongodb:
  - 3.4.17-v4
  - 3.4.22-v4
  - 3.6.13-v4
  - 3.6.8-v4
  - 4.0.11-v4
  - 4.0.3-v4
  - 4.0.5-v4
  - 4.1.13-v4
  - 4.1.4-v4
  - 4.1.7-v4
  - 4.2.3-v4
  mysql:
  - 5.7.25-v4
  - 8.0.14-v4
  - 8.0.3-v4
  percona-xtradb:
  - 5.7-v4
  postgres:
  - 10.14.0-v2
  - 11.9.0-v2
  - 12.4.0-v2
  - 9.6.19-v2
  version: v2020.11.06
---

{{< notice type="warning" message="This is an Enterprise-only feature. Please install [Stash Enterprise Edition](/docs/v2020.11.06/setup/install/enterprise) to try this feature." >}}

# Uninstall MySQL addon for Stash

In order to uninstall MySQL addon, follow the instruction given below.

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

Run the following script to uninstall `stash-mysql` addon that was installed as a Helm chart using Helm 3.

```bash
curl -fsSL https://github.com/stashed/catalog/raw/{{< param "info.catalog" >}}/deploy/helm3.sh | bash -s -- --uninstall --catalog=stash-mysql
```

</div>
<div class="tab-pane fade" id="helm2" role="tabpanel" aria-labelledby="helm2-tab">

## Using Helm 2

Run the following script to uninstall `stash-mysql` addon that was installed as a Helm chart using Helm 2.

```bash
curl -fsSL https://github.com/stashed/catalog/raw/{{< param "info.catalog" >}}/deploy/helm2.sh | bash -s -- --uninstall --catalog=stash-mysql
```

</div>
<div class="tab-pane fade" id="script" role="tabpanel" aria-labelledby="script-tab">

## Using YAML

Run the following script to uninstall `stash-mysql` addon that was installed as Kubernetes YAMLs.

```bash
curl -fsSL https://github.com/stashed/catalog/raw/{{< param "info.catalog" >}}/deploy/script.sh | bash -s -- --uninstall --catalog=stash-mysql
```

</div>
</div>

## Customizing Uninstaller

In order to uninstall MySQL addon only for a specific database version, use `--version` flag to specify the desired version.

```bash
curl -fsSL https://github.com/stashed/catalog/raw/{{< param "info.catalog" >}}/deploy/helm3.sh | bash -s -- --uninstall --catalog=stash-mysql --version=8.0.14
```
