---
title: Setup | Stash
description: Setup guides for Stash by AppsCode
menu:
  docs_v2023.05.31:
    identifier: setup-readme
    name: Readme
    parent: setup
    weight: -1
product_name: stash
menu_name: docs_v2023.05.31
section_menu_id: setup
url: /docs/v2023.05.31/setup/
aliases:
- /docs/v2023.05.31/setup/README/
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

# Setup

<div style="text-align: center;">
  <a class="button is-link is-medium is-active has-text-weight-normal" href="/docs/v2023.05.31/setup/install/community/" style="background:#00A651; width: 18rem;">Install Community Edition</a>
  <a class="button is-info is-medium is-active has-text-weight-normal" href="/docs/v2023.05.31/setup/install/enterprise/"  style="background:#FC6011; width: 18rem;">Try Enterprise Edition</a>
  <a style="margin-top: 10px; display: block;" href="/docs/v2023.05.31/concepts/what-is-stash/overview/">Compare Editions</a>
</div>
<br>

The setup section contains instructions for installing the Stash and its various components in Kubernetes. This section has been divided into the following sub-sections:

- **Install Stash:** Installation instructions for Stash and its various components.
  - [Community Edition](/docs/v2023.05.31/setup/install/community/): Installation instructions for Stash Community Edition.
  - [Enterprise Edition](/docs/v2023.05.31/setup/install/enterprise/): Installation instructions for Stash Enterprise Edition.
  - [Stash kubectl Plugin](/docs/v2023.05.31/setup/install/kubectl-plugin/): Installation instructions for Stash `kubectl` plugin.
  - [Troubleshooting](/docs/v2023.05.31/setup/install/troubleshooting/): Troubleshooting guide for various installation problems.
- **Uninstall Stash:** Uninstallation instructions for Stash and its various components.
  - [Community Edition](/docs/v2023.05.31/setup/uninstall/community/): Uninstallation instructions for Stash Community Edition.
  - [Enterprise Edition](/docs/v2023.05.31/setup/uninstall/enterprise/): Uninstallation instructions for Stash Enterprise Edition.
  - [Stash kubectl Plugin](/docs/v2023.05.31/setup/uninstall/kubectl-plugin/): Uninstallation instructions for Stash `kubectl` plugin.
- [Upgrading Stash](/docs/v2023.05.31/setup/upgrade/): Instruction for updating Stash license and upgrading between various Stash versions.
