---
title: Elasticsearch Addon Overview | Stash
description: Elasticsearch Addon Overview | Stash
menu:
  docs_v2020.11.06:
    identifier: stash-elasticsearch-readme
    name: Readme
    parent: stash-elasticsearch
    weight: -1
product_name: stash
menu_name: docs_v2020.11.06
section_menu_id: stash-addons
url: /docs/v2020.11.06/addons/elasticsearch/
aliases:
- /docs/v2020.11.06/addons/elasticsearch/README/
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

# Stash Elasticsearch Addon

Stash 0.9.0+ supports extending its functionality through addons. Stash Elasticsearch addon enables Stash to backup and restore Elasticsearch databases.

This guide will give you an overview of which Elasticsearch versions are supported and how the docs are organized.

## Supported Elasticsearch Versions

Stash supports backup and restore of the following Elasticsearch versions:

{{< versionlist "elasticsearch" "/docs/v2020.11.06/addons/elasticsearch/guides/%s/elasticsearch" >}}

>Version **M.M** actually represents the latest patch of **M.M.P** series. For example, version **7.3** actually represents **7.3.2** as it is the latest supported patch of **7.3** series. Now, if **7.3.3** is released then **7.3** will represents **7.3.3**.

## Documentation Overview

Stash Elasticsearch documentations are organized as below:

- [How does it works?](/docs/v2020.11.06/addons/elasticsearch/overview) gives an overview of how backup and restore process for Elasticsearch database works in Stash.
- [Setup](/docs/v2020.11.06/addons/elasticsearch/setup/install) shows how to install and uninstall Elasticsearch addon for Stash.
- [Guides](/docs/v2020.11.06/addons/elasticsearch/guides/6.5/elasticsearch) contains step by step guides to backup and restore different versions of Elasticsearch databases.
