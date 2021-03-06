---
title: MongoDB Addon Overview | Stash
description: MongoDB Addon Overview | Stash
menu:
  docs_v2020.10.21:
    identifier: stash-mongodb-readme
    name: Readme
    parent: stash-mongodb
    weight: -1
product_name: stash
menu_name: docs_v2020.10.21
section_menu_id: stash-addons
url: /docs/v2020.10.21/addons/mongodb/
aliases:
- /docs/v2020.10.21/addons/mongodb/README/
info:
  catalog: v2020.10.21
  cli: v0.11.3
  community: v0.11.3
  elasticsearch:
  - 5.6.4-v3
  - 6.2.4-v3
  - 6.3.0-v3
  - 6.4.0-v3
  - 6.5.3-v3
  - 6.8.0-v3
  - 7.2.0-v3
  - 7.3.2-v3
  enterprise: v0.11.3
  installer: v0.11.3
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
  version: v2020.10.21
---

{{< notice type="warning" message="This is an Enterprise-only feature. Please install [Stash Enterprise Edition](/docs/v2020.10.21/setup/install/enterprise) to try this feature." >}}

# Stash MongoDB Addon

Stash 0.9.0+ supports extending its functionality through addons. Stash MongoDB addon enables Stash to backup and restore MongoDB databases.

This guide will give you an overview of which MongoDB versions are supported and how the docs are organized.

## Supported MongoDB Versions

Stash supports backup and restore of the following MongoDB versions:

{{< versionlist "mongodb" "/docs/v2020.10.21/addons/mongodb/guides/%s/mongodb" >}}

>Version **M.M** actually represents the latest patch of **M.M.P** series. For example, version **4.1** actually represents **4.1.13** as it is the latest supported patch of **4.1** series. Now, if **4.1.14** is released then **4.1** will represents **4.1.14**.

## Documentation Overview

Stash MongoDB documentations are organized as below:

- [How does it works?](/docs/v2020.10.21/addons/mongodb/overview) gives an overview of how backup and restore process for MongoDB database works in Stash.
- [Setup](/docs/v2020.10.21/addons/mongodb/setup/install) shows how to install and uninstall MongoDB addon for Stash.
- [Guides](/docs/v2020.10.21/addons/mongodb/guides/3.6/mongodb) contains step by step guides to backup and restore different versions of MongoDB databases.
