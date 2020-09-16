---
title: Elasticsearch Addon Overview | Stash
description: Elasticsearch Addon Overview | Stash
menu:
  docs_v2020.09.16:
    identifier: stash-elasticsearch-readme
    name: Readme
    parent: stash-elasticsearch
    weight: -1
product_name: stash
menu_name: docs_v2020.09.16
section_menu_id: stash-addons
url: /docs/v2020.09.16/addons/elasticsearch/
aliases:
- /docs/v2020.09.16/addons/elasticsearch/README/
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

# Stash Elasticsearch Addon

Stash 0.9.0+ supports extending its functionality through addons. Stash Elasticsearch addon enables Stash to backup and restore Elasticsearch databases.

This guide will give you an overview of which Elasticsearch versions are supported and how the docs are organized.

## Supported Elasticsearch Versions

Stash supports backup and restore of the following Elasticsearch versions:

{{< versionlist "elasticsearch" "/docs/v2020.09.16/addons/elasticsearch/guides/%s/elasticsearch" >}}

>Version **M.M** actually represents the latest patch of **M.M.P** series. For example, version **7.3** actually represents **7.3.2** as it is the latest supported patch of **7.3** series. Now, if **7.3.3** is released then **7.3** will represents **7.3.3**.

## Documentation Overview

Stash Elasticsearch documentations are organized as below:

- [How does it works?](/docs/v2020.09.16/addons/elasticsearch/overview) gives an overview of how backup and restore process for Elasticsearch database works in Stash.
- [Setup](/docs/v2020.09.16/addons/elasticsearch/setup/install) shows how to install and uninstall Elasticsearch addon for Stash.
- [Guides](/docs/v2020.09.16/addons/elasticsearch/guides/6.5/elasticsearch) contains step by step guides to backup and restore different versions of Elasticsearch databases.
