---
title: Setup | Stash
description: Setup guides for Stash by AppsCode
menu:
  docs_v2022.03.29:
    identifier: setup-readme
    name: Readme
    parent: setup
    weight: -1
product_name: stash
menu_name: docs_v2022.03.29
section_menu_id: setup
url: /docs/v2022.03.29/setup/
aliases:
- /docs/v2022.03.29/setup/README/
info:
  cli: v0.19.0
  community: v0.19.0
  elasticsearch:
  - 5.6.4-v16
  - 6.2.4-v16
  - 6.3.0-v16
  - 6.4.0-v16
  - 6.5.3-v16
  - 6.8.0-v16
  - 7.14.0-v2
  - 7.2.0-v16
  - 7.3.2-v16
  enterprise: v0.19.0
  etcd:
  - 3.5.0-v3
  installer: v2022.03.29
  mariadb:
  - 10.5.8-v9
  mongodb:
  - 3.4.17-v15
  - 3.4.22-v15
  - 3.6.13-v15
  - 3.6.8-v15
  - 4.0.11-v15
  - 4.0.3-v15
  - 4.0.5-v15
  - 4.1.13-v15
  - 4.1.4-v15
  - 4.1.7-v15
  - 4.2.3-v15
  - 4.4.6-v6
  - 5.0.3-v3
  mysql:
  - 5.7.25-v16
  - 8.0.14-v16
  - 8.0.21-v10
  - 8.0.3-v16
  nats:
  - 2.6.1-v3
  percona-xtradb:
  - 5.7-v11
  postgres:
  - 10.14-v14
  - 11.9-v14
  - 12.4-v14
  - 13.1-v11
  - 14.0-v3
  - 9.6.19-v14
  redis:
  - 5.0.13-v4
  - 6.2.5-v4
  ui-server: v0.2.0
  version: v2022.03.29
---

# Setup

<div style="text-align: center;">
  <a class="button is-link is-medium is-active has-text-weight-normal" href="/docs/v2022.03.29/setup/install/community" style="background:#00A651; width: 18rem;">Install Community Edition</a>
  <a class="button is-info is-medium is-active has-text-weight-normal" href="/docs/v2022.03.29/setup/install/enterprise"  style="background:#FC6011; width: 18rem;">Try Enterprise Edition</a>
  <a style="margin-top: 10px; display: block;" href="/docs/v2022.03.29/concepts/what-is-stash/overview">Compare Editions</a>
</div>
<br>

The setup section contains instructions for installing the Stash and its various components in Kubernetes. This section has been divided into the following sub-sections:

- **Install Stash:** Installation instructions for Stash and its various components.
  - [Community Edition](/docs/v2022.03.29/setup/install/community): Installation instructions for Stash Community Edition.
  - [Enterprise Edition](/docs/v2022.03.29/setup/install/enterprise): Installation instructions for Stash Enterprise Edition.
  - [Stash kubectl Plugin](/docs/v2022.03.29/setup/install/kubectl_plugin): Installation instructions for Stash `kubectl` plugin.
  - [Troubleshooting](/docs/v2022.03.29/setup/install/troubleshoting): Troubleshooting guide for various installation problems.
- **Uninstall Stash:** Uninstallation instructions for Stash and its various components.
  - [Community Edition](/docs/v2022.03.29/setup/uninstall/community): Uninstallation instructions for Stash Community Edition.
  - [Enterprise Edition](/docs/v2022.03.29/setup/uninstall/enterprise): Uninstallation instructions for Stash Enterprise Edition.
  - [Stash kubectl Plugin](/docs/v2022.03.29/setup/uninstall/kubectl_plugin): Uninstallation instructions for Stash `kubectl` plugin.
- [Upgrading Stash](/docs/v2022.03.29/setup/upgrade/): Instruction for updating Stash license and upgrading between various Stash versions.
