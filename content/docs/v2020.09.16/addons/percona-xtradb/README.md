---
title: Percona XtraDB Addon Overview | Stash
description: Percona XtraDB Addon Overview | Stash
menu:
  docs_v2020.09.16:
    identifier: stash-percona-xtradb-readme
    name: Readme
    parent: stash-percona-xtradb
    weight: -1
product_name: stash
menu_name: docs_v2020.09.16
section_menu_id: stash-addons
url: /docs/v2020.09.16/addons/percona-xtradb/
aliases:
- /docs/v2020.09.16/addons/percona-xtradb/README/
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
  - 3.4.17-v2
  - 3.4.22-v2
  - 3.6.13-v2
  - 3.6.8-v2
  - 4.0.11-v2
  - 4.0.3-v2
  - 4.0.5-v2
  - 4.1.13-v2
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

{{< notice type="warning" message="This is an Enterprise-only feature. Please install [Stash Enterprise Edition](/docs/v2020.09.16/setup/install/enterprise) to try this feature." >}}

# Stash Percona XtraDB Addon

Stash 0.9.0+ supports extending its functionality through addons. Stash Percona XtraDB addon enables Stash to backup and restore Percona XtraDB databases.

This guide will give you an overview of which Percona XtraDB versions are supported and how the docs are organized.

## Supported Percona XtraDB Versions

Stash supports backup and restore of the following Percona XtraDB versions:

{{< versionlist "percona-xtradb" "/docs/v2020.09.16/addons/percona-xtradb/guides/%s/clustered" >}}

## Documentation Overview

Stash Percona XtraDB documentations are organized as below:

- [How does it works?](/docs/v2020.09.16/addons/percona-xtradb/overview) gives an overview of how backup and restore process for Percona XtraDB database works in Stash.
- [Setup](/docs/v2020.09.16/addons/percona-xtradb/setup/install) shows how to install and uninstall Percona XtraDB addon for Stash.
- [Guides](/docs/v2020.09.16/addons/percona-xtradb/guides/5.7/clustered) contains step by step guides to backup and restore different versions of Percona XtraDB databases.
