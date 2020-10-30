---
title: Uninstall PostgreSQL Addon | Stash
description: An guide on how to uninstall PostgreSQL addon for Stash
menu:
  docs_v2020.10.30:
    identifier: stash-postgres-uninstall
    name: Uninstall
    parent: stash-postgres-setup
    weight: 20
product_name: stash
menu_name: docs_v2020.10.30
section_menu_id: stash-addons
info:
  catalog: v2020.10.30
  cli: v0.11.5
  community: v0.11.5
  elasticsearch:
  - 5.6.4-v3
  - 6.2.4-v3
  - 6.3.0-v3
  - 6.4.0-v3
  - 6.5.3-v3
  - 6.8.0-v3
  - 7.2.0-v3
  - 7.3.2-v3
  enterprise: v0.11.5
  installer: v0.11.5
  mongodb:
  - 3.4.17-v3
  - 3.4.22-v3
  - 3.6.13-v3
  - 3.6.8-v3
  - 4.0.11-v3
  - 4.0.3-v3
  - 4.0.5-v3
  - 4.1.13-v3
  - 4.1.4-v3
  - 4.1.7-v3
  - 4.2.3-v3
  mysql:
  - 5.7.25-v3
  - 8.0.14-v3
  - 8.0.3-v3
  percona-xtradb:
  - 5.7-v3
  postgres:
  - 10.14.0-v1
  - 11.9.0-v1
  - 12.4.0-v1
  - 9.6.19-v1
  version: v2020.10.30
---

{{< notice type="warning" message="This is an Enterprise-only feature. Please install [Stash Enterprise Edition](/docs/v2020.10.30/setup/install/enterprise) to try this feature." >}}

# Uninstall PostgreSQL addon for Stash

In order to uninstall PostgreSQL addon, follow the instruction given below.

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

Run the following script to uninstall `stash-postgres` addon that was installed as a Helm chart using Helm 3.

```bash
curl -fsSL https://github.com/stashed/catalog/raw/{{< param "info.catalog" >}}/deploy/helm3.sh | bash -s -- --uninstall --catalog=stash-postgres
```

</div>
<div class="tab-pane fade" id="helm2" role="tabpanel" aria-labelledby="helm2-tab">

## Using Helm 2

Run the following script to uninstall `stash-postgres` addon that was installed as a Helm chart using Helm 2.

```bash
curl -fsSL https://github.com/stashed/catalog/raw/{{< param "info.catalog" >}}/deploy/helm2.sh | bash -s -- --uninstall --catalog=stash-postgres
```

</div>
<div class="tab-pane fade" id="script" role="tabpanel" aria-labelledby="script-tab">

## Using YAML

Run the following script to uninstall `stash-postgres` addon that was installed as Kubernetes YAMLs.

```bash
curl -fsSL https://github.com/stashed/catalog/raw/{{< param "info.catalog" >}}/deploy/script.sh | bash -s -- --uninstall --catalog=stash-postgres
```

</div>
</div>

## Customizing Uninstaller

In order to uninstall PostgreSQL addon only for a specific database version, use `--version` flag to specify the desired version.

```bash
curl -fsSL https://github.com/stashed/catalog/raw/{{< param "info.catalog" >}}/deploy/helm3.sh | bash -s -- --uninstall --catalog=stash-postgres --version=11.2
```
