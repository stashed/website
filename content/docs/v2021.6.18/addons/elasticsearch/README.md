---
title: Elasticsearch Addon Overview | Stash
description: Elasticsearch Addon Overview | Stash
menu:
  docs_v2021.6.18:
    identifier: stash-elasticsearch-readme
    name: Readme
    parent: stash-elasticsearch
    weight: -1
product_name: stash
menu_name: docs_v2021.6.18
section_menu_id: stash-addons
url: /docs/v2021.6.18/addons/elasticsearch/
aliases:
- /docs/v2021.6.18/addons/elasticsearch/README/
info:
  cli: v0.14.0
  community: v0.14.0
  elasticsearch:
  - 5.6.4-v10
  - 6.2.4-v10
  - 6.3.0-v10
  - 6.4.0-v10
  - 6.5.3-v10
  - 6.8.0-v10
  - 7.2.0-v10
  - 7.3.2-v10
  enterprise: v0.14.0
  installer: v2021.6.18
  mariadb:
  - 10.5.8-v3
  mongodb:
  - 3.4.17-v9
  - 3.4.22-v9
  - 3.6.13-v9
  - 3.6.8-v9
  - 4.0.11-v9
  - 4.0.3-v9
  - 4.0.5-v9
  - 4.1.13-v9
  - 4.1.4-v9
  - 4.1.7-v9
  - 4.2.3-v9
  - 4.4.6
  mysql:
  - 5.7.25-v10
  - 8.0.14-v10
  - 8.0.21-v4
  - 8.0.3-v10
  percona-xtradb:
  - 5.7-v5
  postgres:
  - 10.14-v8
  - 11.9-v8
  - 12.4-v8
  - 13.1-v5
  - 9.6.19-v8
  version: v2021.6.18
---

{{< notice type="warning" message="This is an Enterprise-only feature. Please install [Stash Enterprise Edition](/docs/v2021.6.18/setup/install/enterprise) to try this feature." >}}

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

- [How does it works?](/docs/v2021.6.18/addons/elasticsearch/overview/) gives an overview of how backup and restore process for Elasticsearch database works in Stash.
- [KubeDB managed Elasticsearch](/docs/v2021.6.18/addons/elasticsearch/kubedb/) shows how to backup and restore a KubeDB managed Elasticsearch database.
- [Auto-Backup](/docs/v2021.6.18/addons/elasticsearch/auto-backup/) shows how to configure a generic backup template for all the Elasticsearch databases of a cluster.
- [Customizing Backup & Restore Process](/docs/v2021.6.18/addons/elasticsearch/customization/) shows how to customize the backup & restore process.
