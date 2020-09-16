---
title: Uninstall Stash Community Edition
description: Uninstallation guide for Stash Community edition
menu:
  docs_v2020.09.16:
    identifier: uninstall-stash-community
    name: Community Edition
    parent: uninstallation-guide
    weight: 10
product_name: stash
menu_name: docs_v2020.09.16
section_menu_id: setup
info:
  catalog: v2020.09.16
  cli: v0.11.0
  community: v0.11.0
  elasticsearch:
  - 5.6.4-v2
  - 6.2.4-v2
  - 6.3.0-v2
  - 6.4.0-v2
  - 6.5.3-v2
  - 6.8.0-v2
  - 7.2.0-v2
  - 7.3.2-v2
  enterprise: v0.11.0
  mongodb:
  - 3.4.1-v2
  - 3.4.2-v2
  - 3.6.1-v2
  - 3.6.8-v2
  - 4.0.11-v2
  - 4.0.3-v2
  - 4.0.5-v2
  - 4.1.1-v2
  - 4.1.4-v2
  - 4.1.7-v2
  - 4.2.3-v2
  mysql:
  - 5.7.25-v2
  - 8.0.14-v2
  - 8.0.3-v2
  percona-xtradb:
  - 5.7-v2
  postgres:
  - 10.14.0
  - 11.9.0
  - 12.4.0
  - 9.6.19
  version: v2020.09.16
---

# Uninstall Stash Community Edition

To uninstall Stash Community edition, run the following command:

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

In Helm 3, release names are [scoped to a namespace](https://v3.helm.sh/docs/faq/#release-names-are-now-scoped-to-the-namespace). So, provide the namespace you used to install the operator when installing.

```bash
$ helm uninstall stash --namespace kube-system
```

</div>
<div class="tab-pane fade" id="helm2" role="tabpanel" aria-labelledby="helm2-tab">

## Using Helm 2

```bash
$ helm delete stash
```

</div>
<div class="tab-pane fade" id="script" role="tabpanel" aria-labelledby="script-tab">

## Using YAML (with helm 3)

If you prefer to not use Helm, you can generate YAMLs from Stash chart and uninstall using `kubectl`.

```bash
$ helm template stash appscode/stash --namespace kube-system | kubectl delete -f -
```

</div>
</div>
