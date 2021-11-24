---
title: NATS Addon Overview | Stash
description: NATS Addon Overview | Stash
menu:
  docs_v2021.11.24:
    identifier: stash-nats-readme
    name: Readme
    parent: stash-nats
    weight: -1
product_name: stash
menu_name: docs_v2021.11.24
section_menu_id: stash-addons
url: /docs/v2021.11.24/addons/nats/
aliases:
- /docs/v2021.11.24/addons/nats/README/
info:
  cli: v0.17.0
  community: v0.17.0
  elasticsearch:
  - 5.6.4-v14
  - 6.2.4-v14
  - 6.3.0-v14
  - 6.4.0-v14
  - 6.5.3-v14
  - 6.8.0-v14
  - 7.14.0
  - 7.2.0-v14
  - 7.3.2-v14
  enterprise: v0.17.0
  etcd:
  - 3.5.0-v1
  installer: v2021.11.24
  mariadb:
  - 10.5.8-v7
  mongodb:
  - 3.4.17-v13
  - 3.4.22-v13
  - 3.6.13-v13
  - 3.6.8-v13
  - 4.0.11-v13
  - 4.0.3-v13
  - 4.0.5-v13
  - 4.1.13-v13
  - 4.1.4-v13
  - 4.1.7-v13
  - 4.2.3-v13
  - 4.4.6-v4
  - 5.0.3-v1
  mysql:
  - 5.7.25-v14
  - 8.0.14-v14
  - 8.0.21-v8
  - 8.0.3-v14
  nats:
  - 2.6.1-v1
  percona-xtradb:
  - 5.7-v9
  postgres:
  - 10.14-v12
  - 11.9-v12
  - 12.4-v12
  - 13.1-v9
  - 14.0-v1
  - 9.6.19-v12
  redis:
  - 5.0.13-v2
  - 6.2.5-v2
  version: v2021.11.24
---

{{< notice type="warning" message="This is an Enterprise-only feature. Please install [Stash Enterprise Edition](/docs/v2021.11.24/setup/install/enterprise) to try this feature." >}}

# Stash NATS Addon

Stash `{{< param "info.version" >}}` supports extending its functionality through addons. Stash NATS addon enables Stash to backup and restore NATS streams.

This guide will give you an overview of which NATS versions are supported and how the docs are organized.

## Supported NATS Versions

Stash has the following addon versions for NATS:

{{< versionlist "nats">}}

Here, the addon follows `M.M.P` versioning scheme where `M.M.P` (Major.Minor.Patch) represents the respective NATS version.

## Addon Version Compatibility

This addon with matching major version with the NATS version should be able to take backup of that NATS streams. For example, NATS addon with version `2.x.x` should be able take backup of any NATS of `2.x.x` series. However, this might not be true for some versions. In that case, we will have separate addon for that version.

## Documentation Overview

Stash NATS documentations are organized as below:

- [How does it works?](/docs/v2021.11.24/addons/nats/overview/) gives an overview of how backup and restore process for NATS works in Stash.
- [Helm managed NATS](/docs/v2021.11.24/addons/nats/helm/) shows how to backup and restore a Helm managed NATS.
- **Different authentications:** shows how to backup and restore NATS using different authentication methods.
  - [Basic Authentication](/docs/v2021.11.24/addons/nats/authentications/basic-auth/)
  - [Token Authentication](/docs/v2021.11.24/addons/nats/authentications/token-auth/)
  - [Nkey Authentication](/docs/v2021.11.24/addons/nats/authentications/nkey-auth/)
  - [JWT Authentication](/docs/v2021.11.24/addons/nats/authentications/jwt-auth/)
- [TLS secured NATS](/docs/v2021.11.24/addons/nats/tls/) shows how to backup and restore TLS secured NATS.
- [Customizing Backup & Restore Process](/docs/v2021.11.24/addons/nats/customization/) shows how to customize the backup & restore process.
