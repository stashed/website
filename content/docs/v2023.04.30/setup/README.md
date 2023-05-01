---
title: Setup | Stash
description: Setup guides for Stash by AppsCode
menu:
  docs_v2023.04.30:
    identifier: setup-readme
    name: Readme
    parent: setup
    weight: -1
product_name: stash
menu_name: docs_v2023.04.30
section_menu_id: setup
url: /docs/v2023.04.30/setup/
aliases:
- /docs/v2023.04.30/setup/README/
info:
  cli: v0.29.0
  community: v0.29.0
  elasticsearch:
  - 5.6.4-v25
  - 6.2.4-v25
  - 6.3.0-v25
  - 6.4.0-v25
  - 6.5.3-v25
  - 6.8.0-v25
  - 7.14.0-v11
  - 7.2.0-v25
  - 7.3.2-v25
  - 8.2.0-v8
  enterprise: v0.29.0
  etcd:
  - 3.5.0-v12
  installer: v2023.04.30
  kubedump:
  - 0.1.0-v8
  mariadb:
  - 10.5.8-v18
  mongodb:
  - 3.4.17-v25
  - 3.4.22-v25
  - 3.6.13-v25
  - 3.6.8-v25
  - 4.0.11-v25
  - 4.0.3-v25
  - 4.0.5-v25
  - 4.1.13-v25
  - 4.1.4-v25
  - 4.1.7-v25
  - 4.2.3-v25
  - 4.4.6-v16
  - 5.0.3-v13
  - 6.0.5-v1
  mysql:
  - 5.7.25-v25
  - 8.0.14-v25
  - 8.0.21-v19
  - 8.0.3-v25
  nats:
  - 2.6.1-v13
  - 2.8.2-v8
  percona-xtradb:
  - 5.7-v20
  postgres:
  - 10.14-v24
  - 11.9-v24
  - 12.4-v24
  - 13.1-v21
  - 14.0-v13
  - 15.1-v5
  - 9.6.19-v24
  redis:
  - 5.0.13-v13
  - 6.2.5-v13
  - 7.0.5-v6
  ui-server: v0.11.0
  vault:
  - 1.10.3-v5
  version: v2023.04.30
---

# Setup

<div style="text-align: center;">
  <a class="button is-link is-medium is-active has-text-weight-normal" href="/docs/v2023.04.30/setup/install/community/" style="background:#00A651; width: 18rem;">Install Community Edition</a>
  <a class="button is-info is-medium is-active has-text-weight-normal" href="/docs/v2023.04.30/setup/install/enterprise/"  style="background:#FC6011; width: 18rem;">Try Enterprise Edition</a>
  <a style="margin-top: 10px; display: block;" href="/docs/v2023.04.30/concepts/what-is-stash/overview/">Compare Editions</a>
</div>
<br>

The setup section contains instructions for installing the Stash and its various components in Kubernetes. This section has been divided into the following sub-sections:

- **Install Stash:** Installation instructions for Stash and its various components.
  - [Community Edition](/docs/v2023.04.30/setup/install/community/): Installation instructions for Stash Community Edition.
  - [Enterprise Edition](/docs/v2023.04.30/setup/install/enterprise/): Installation instructions for Stash Enterprise Edition.
  - [Stash kubectl Plugin](/docs/v2023.04.30/setup/install/kubectl-plugin/): Installation instructions for Stash `kubectl` plugin.
  - [Troubleshooting](/docs/v2023.04.30/setup/install/troubleshooting/): Troubleshooting guide for various installation problems.
- **Uninstall Stash:** Uninstallation instructions for Stash and its various components.
  - [Community Edition](/docs/v2023.04.30/setup/uninstall/community/): Uninstallation instructions for Stash Community Edition.
  - [Enterprise Edition](/docs/v2023.04.30/setup/uninstall/enterprise/): Uninstallation instructions for Stash Enterprise Edition.
  - [Stash kubectl Plugin](/docs/v2023.04.30/setup/uninstall/kubectl-plugin/): Uninstallation instructions for Stash `kubectl` plugin.
- [Upgrading Stash](/docs/v2023.04.30/setup/upgrade/): Instruction for updating Stash license and upgrading between various Stash versions.
