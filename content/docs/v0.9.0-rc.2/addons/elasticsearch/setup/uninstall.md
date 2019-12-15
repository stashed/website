---
title: Uninstall Elasticsearch Addon | Stash
description: An guide on how to uninstall Elasticsearch addon for Stash
menu:
  docs_v0.9.0-rc.2:
    identifier: stash-elasticsearch-uninstall
    name: Uninstall
    parent: stash-elasticsearch-setup
    weight: 20
product_name: stash
menu_name: docs_v0.9.0-rc.2
section_menu_id: stash-addons
info:
  catalog: v0.1.0
  cli: v0.2.0
  version: v0.9.0-rc.2
---

# Uninstall Elasticsearch addon for Stash

In order to uninstall Elasticsearch addon, follow the instruction given below.

<ul class="nav nav-tabs" id="installerTab" role="tablist">
  <li class="nav-item">
    <a class="nav-link active" id="script-tab" data-toggle="tab" href="#script" role="tab" aria-controls="script" aria-selected="true">Script</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" id="helm-tab" data-toggle="tab" href="#helm" role="tab" aria-controls="helm" aria-selected="false">Helm</a>
  </li>
</ul>
<div class="tab-content" id="installerTabContent">
  <div class="tab-pane fade show active" id="script" role="tabpanel" aria-labelledby="script-tab">

## Uninstall `stash-elasticsearch` addon YAMLs

Run the following script to uninstall `stash-elasticsearch` addon that was installed as Kubernetes YAMLs.

```console
curl -fsSL https://github.com/stashed/catalog/raw/{{< param "info.catalog" >}}/deploy/script.sh | bash -s -- --uninstall --catalog=stash-elasticsearch
```

</div>
<div class="tab-pane fade" id="helm" role="tabpanel" aria-labelledby="helm-tab">

## Uninstall `stash-elasticsearch` chart

Run the following script to uninstall `stash-elasticsearch` addon that was installed as a Helm chart.

```console
curl -fsSL https://github.com/stashed/catalog/raw/{{< param "info.catalog" >}}/deploy/chart.sh | bash -s -- --uninstall --catalog=stash-elasticsearch
```

</div>
</div>

## Customizing Uninstaller

In order to uninstall Elasticsearch addon only for a specific database version, use `--version` flag to specify the desired version.

```console
curl -fsSL https://github.com/stashed/catalog/raw/{{< param "info.catalog" >}}/deploy/chart.sh | bash -s -- --uninstall --catalog=stash-elasticsearch --version=6.5
```
