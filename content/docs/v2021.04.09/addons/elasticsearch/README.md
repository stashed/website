---
title: Elasticsearch Addon Overview | Stash
description: Elasticsearch Addon Overview | Stash
menu:
  docs_v2021.04.09:
    identifier: stash-elasticsearch-readme
    name: Readme
    parent: stash-elasticsearch
    weight: -1
product_name: stash
menu_name: docs_v2021.04.09
section_menu_id: stash-addons
url: /docs/v2021.04.09/addons/elasticsearch/
aliases:
- /docs/v2021.04.09/addons/elasticsearch/README/
info:
  cli: v0.12.2
  community: v0.12.2
  elasticsearch:
  - 5.6.4-v8
  - 6.2.4-v8
  - 6.3.0-v8
  - 6.4.0-v8
  - 6.5.3-v8
  - 6.8.0-v8
  - 7.2.0-v8
  - 7.3.2-v8
  enterprise: v0.12.2
  installer: v2021.04.09
  mariadb:
  - 10.5.8-v2
  mongodb:
  - 3.4.17-v7
  - 3.4.22-v7
  - 3.6.13-v7
  - 3.6.8-v7
  - 4.0.11-v7
  - 4.0.3-v7
  - 4.0.5-v7
  - 4.1.13-v7
  - 4.1.4-v7
  - 4.1.7-v7
  - 4.2.3-v7
  mysql:
  - 5.7.25-v8
  - 8.0.14-v8
  - 8.0.21-v2
  - 8.0.3-v8
  percona-xtradb:
  - 5.7-v3
  postgres:
  - 10.14-v6
  - 11.9-v6
  - 12.4-v6
  - 13.1-v3
  - 9.6.19-v6
  version: v2021.04.09
---

{{< notice type="warning" message="This is an Enterprise-only feature. Please install [Stash Enterprise Edition](/docs/v2021.04.09/setup/install/enterprise) to try this feature." >}}

# Stash Elasticsearch Addon

Stash 0.9.0+ supports extending its functionality through addons. Stash Elasticsearch addon enables Stash to backup and restore Elasticsearch databases.

This guide will give you an overview of which Elasticsearch versions are supported and how the docs are organized.

## Available Elasticsearch Addon Versions

Stash has the following addon versions for Elasticsearch:

{{< versionlist "elasticsearch">}}

Here, the addon follows `M.M.P` versioning scheme where `M.M.P` (Major.Minor.Patch) represents the respective database version.

## Addon Version Compatibility

Any addon with matching major version with the database version should be able to take backup of that database. For example, Elasticsearch addon with version `7.x.x` should be able take backup of any Elasticsearch of `7.x.x` series. However, this might not be true for some versions. In that case, we will have separate addon for that version.

## Documentation Overview

Stash Elasticsearch documentations are organized as below:

- [How does it works?](/docs/v2021.04.09/addons/elasticsearch/overview/) gives an overview of how backup and restore process for Elasticsearch database works in Stash.
- [KubeDB managed Elasticsearch](/docs/v2021.04.09/addons/elasticsearch/kubedb/) shows how to backup and restore a KubeDB managed Elasticsearch database.
- [Auto-Backup](/docs/v2021.04.09/addons/elasticsearch/auto-backup/) shows how to configure a generic backup template for all the Elasticsearch databases of a cluster.
- [Customizing Backup & Restore Process](/docs/v2021.04.09/addons/elasticsearch/customization/) shows how to customize the backup & restore process.
