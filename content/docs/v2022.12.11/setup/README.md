---
title: Setup | Stash
description: Setup guides for Stash by AppsCode
menu:
  docs_v2022.12.11:
    identifier: setup-readme
    name: Readme
    parent: setup
    weight: -1
product_name: stash
menu_name: docs_v2022.12.11
section_menu_id: setup
url: /docs/v2022.12.11/setup/
aliases:
- /docs/v2022.12.11/setup/README/
info:
  cli: v0.24.0
  community: v0.24.0
  elasticsearch:
  - 5.6.4-v20
  - 6.2.4-v20
  - 6.3.0-v20
  - 6.4.0-v20
  - 6.5.3-v20
  - 6.8.0-v20
  - 7.14.0-v6
  - 7.2.0-v20
  - 7.3.2-v20
  - 8.2.0-v3
  enterprise: v0.24.0
  etcd:
  - 3.5.0-v7
  installer: v2022.12.11
  kubedump:
  - 0.1.0-v3
  mariadb:
  - 10.5.8-v13
  mongodb:
  - 3.4.17-v20
  - 3.4.22-v20
  - 3.6.13-v20
  - 3.6.8-v20
  - 4.0.11-v20
  - 4.0.3-v20
  - 4.0.5-v20
  - 4.1.13-v20
  - 4.1.4-v20
  - 4.1.7-v20
  - 4.2.3-v20
  - 4.4.6-v11
  - 5.0.3-v8
  mysql:
  - 5.7.25-v20
  - 8.0.14-v20
  - 8.0.21-v14
  - 8.0.3-v20
  nats:
  - 2.6.1-v8
  - 2.8.2-v3
  percona-xtradb:
  - 5.7-v15
  postgres:
  - 10.14-v19
  - 11.9-v19
  - 12.4-v19
  - 13.1-v16
  - 14.0-v8
  - "15.1"
  - 9.6.19-v19
  redis:
  - 5.0.13-v8
  - 6.2.5-v8
  - 7.0.5-v1
  ui-server: v0.6.0
  vault:
  - 1.10.3
  version: v2022.12.11
---

# Setup

<div style="text-align: center;">
  <a class="button is-link is-medium is-active has-text-weight-normal" href="/docs/v2022.12.11/setup/install/community/" style="background:#00A651; width: 18rem;">Install Community Edition</a>
  <a class="button is-info is-medium is-active has-text-weight-normal" href="/docs/v2022.12.11/setup/install/enterprise/"  style="background:#FC6011; width: 18rem;">Try Enterprise Edition</a>
  <a style="margin-top: 10px; display: block;" href="/docs/v2022.12.11/concepts/what-is-stash/overview/">Compare Editions</a>
</div>
<br>

The setup section contains instructions for installing the Stash and its various components in Kubernetes. This section has been divided into the following sub-sections:

- **Install Stash:** Installation instructions for Stash and its various components.
  - [Community Edition](/docs/v2022.12.11/setup/install/community/): Installation instructions for Stash Community Edition.
  - [Enterprise Edition](/docs/v2022.12.11/setup/install/enterprise/): Installation instructions for Stash Enterprise Edition.
  - [Stash kubectl Plugin](/docs/v2022.12.11/setup/install/kubectl-plugin/): Installation instructions for Stash `kubectl` plugin.
  - [Troubleshooting](/docs/v2022.12.11/setup/install/troubleshooting/): Troubleshooting guide for various installation problems.
- **Uninstall Stash:** Uninstallation instructions for Stash and its various components.
  - [Community Edition](/docs/v2022.12.11/setup/uninstall/community/): Uninstallation instructions for Stash Community Edition.
  - [Enterprise Edition](/docs/v2022.12.11/setup/uninstall/enterprise/): Uninstallation instructions for Stash Enterprise Edition.
  - [Stash kubectl Plugin](/docs/v2022.12.11/setup/uninstall/kubectl-plugin/): Uninstallation instructions for Stash `kubectl` plugin.
- [Upgrading Stash](/docs/v2022.12.11/setup/upgrade/): Instruction for updating Stash license and upgrading between various Stash versions.
