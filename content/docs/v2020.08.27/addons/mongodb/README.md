---
title: MongoDB Addon Overview | Stash
description: MongoDB Addon Overview | Stash
menu:
  docs_v2020.08.27:
    identifier: stash-mongodb-readme
    name: Readme
    parent: stash-mongodb
    weight: -1
product_name: stash
menu_name: docs_v2020.08.27
section_menu_id: stash-addons
url: /docs/v2020.08.27/addons/mongodb/
aliases:
- /docs/v2020.08.27/addons/mongodb/README/
info:
  catalog: v2020.08.27
  cli: v0.10.0
  community: v0.10.0
  enterprise: v0.10.0
  version: v2020.08.27
---

# Stash MongoDB Addon

Stash 0.9.0+ supports extending its functionality through addons. Stash MongoDB addon enables Stash to backup and restore MongoDB databases.

This guide will give you an overview of which MongoDB versions are supported and how the docs are organized.

## Supported MongoDB Versions

Stash supports backup and restore of the following MongoDB versions:

- [3.4.17](/docs/v2020.08.27/addons/mongodb/guides/3.4.17-v1/mongodb)
- [3.4.22](/docs/v2020.08.27/addons/mongodb/guides/3.4.22-v1/mongodb)
- [3.6.8](/docs/v2020.08.27/addons/mongodb/guides/3.6.8-v1/mongodb)
- [3.6.13](/docs/v2020.08.27/addons/mongodb/guides/3.6.13-v1/mongodb)
- [4.0.3](/docs/v2020.08.27/addons/mongodb/guides/4.0.3-v1/mongodb)
- [4.0.5](/docs/v2020.08.27/addons/mongodb/guides/4.0.5-v1/mongodb)
- [4.0.11](/docs/v2020.08.27/addons/mongodb/guides/4.0.11-v1/mongodb)
- [4.1.4](/docs/v2020.08.27/addons/mongodb/guides/4.1.4-v1/mongodb)
- [4.1.7](/docs/v2020.08.27/addons/mongodb/guides/4.1.7-v1/mongodb)
- [4.1.13](/docs/v2020.08.27/addons/mongodb/guides/4.1.13-v1/mongodb)
- [4.2.3](/docs/v2020.08.27/addons/mongodb/guides/4.2.3-v1/mongodb)

>Version **M.M** actually represents the latest patch of **M.M.P** series. For example, version **4.1** actually represents **4.1.13** as it is the latest supported patch of **4.1** series. Now, if **4.1.14** is released then **4.1** will represents **4.1.14**.

## Documentation Overview

Stash MongoDB documentations are organized as below:

- [How does it works?](/docs/v2020.08.27/addons/mongodb/overview) gives an overview of how backup and restore process for MongoDB database works in Stash.
- [Setup](/docs/v2020.08.27/addons/mongodb/setup/install) shows how to install and uninstall MongoDB addon for Stash.
- [Guides](/docs/v2020.08.27/addons/mongodb/guides/3.6/mongodb) contains step by step guides to backup and restore different versions of MongoDB databases.
