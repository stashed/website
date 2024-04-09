---
title: Changelog | Stash
description: Changelog
menu:
  docs_v2024.4.8:
    identifier: changelog-stash-v2022.05.18
    name: Changelog-v2022.05.18
    parent: welcome
    weight: 20220518
product_name: stash
menu_name: docs_v2024.4.8
section_menu_id: welcome
url: /docs/v2024.4.8/welcome/changelog-v2022.05.18/
aliases:
- /docs/v2024.4.8/CHANGELOG-v2022.05.18/
info:
  cli: v0.34.0
  community: v0.34.0
  elasticsearch:
  - 5.6.4-v31
  - 6.2.4-v31
  - 6.3.0-v31
  - 6.4.0-v31
  - 6.5.3-v31
  - 6.8.0-v31
  - 7.14.0-v17
  - 7.2.0-v31
  - 7.3.2-v31
  - 8.2.0-v14
  enterprise: v0.34.0
  etcd:
  - 3.5.0-v18
  installer: v2024.4.8
  kubedump:
  - 0.1.0-v14
  mariadb:
  - 10.5.8-v25
  mongodb:
  - 3.4.17-v31
  - 3.4.22-v31
  - 3.6.13-v31
  - 3.6.8-v31
  - 4.0.11-v31
  - 4.0.3-v31
  - 4.0.5-v31
  - 4.1.13-v31
  - 4.1.4-v31
  - 4.1.7-v31
  - 4.2.3-v31
  - 4.4.6-v22
  - 5.0.15-v4
  - 5.0.3-v19
  - 6.0.5-v7
  mysql:
  - 5.7.25-v31
  - 8.0.14-v31
  - 8.0.21-v25
  - 8.0.3-v31
  nats:
  - 2.6.1-v19
  - 2.8.2-v14
  percona-xtradb:
  - 5.7-v26
  postgres:
  - 10.14-v30
  - 11.9-v30
  - 12.4-v30
  - 13.1-v27
  - 14.0-v19
  - 15.1-v11
  - "16.1"
  - 9.6.19-v30
  redis:
  - 5.0.13-v19
  - 6.2.5-v19
  - 7.0.5-v12
  ui-server: v0.15.0
  vault:
  - 1.10.3-v11
  version: v2024.4.8
---

# Stash v2022.05.18 (2022-05-18)


## [stashed/apimachinery](https://github.com/stashed/apimachinery)

### [v0.20.1](https://github.com/stashed/apimachinery/releases/tag/v0.20.1)

- [9d802e3a](https://github.com/stashed/apimachinery/commit/9d802e3a) Make commit



## [stashed/enterprise](https://github.com/stashed/enterprise)

### [v0.20.1](https://github.com/stashed/enterprise/releases/tag/v0.20.1)

- [948438a9](https://github.com/stashed/enterprise/commit/948438a9) Prepare for release v0.20.1 (#174)
- [e2fde1aa](https://github.com/stashed/enterprise/commit/e2fde1aa) Fix ImagePullSecrets not passing to the backup job properly (#172)
- [6f871d57](https://github.com/stashed/enterprise/commit/6f871d57) Update BackupConfiguration webhook to make the target immutable (#171)



## [stashed/installer](https://github.com/stashed/installer)

### [v2022.05.18](https://github.com/stashed/installer/releases/tag/v2022.05.18)

- [3b45a21b](https://github.com/stashed/installer/commit/3b45a21b) Prepare for release v2022.05.18 (#255)
- [fc961396](https://github.com/stashed/installer/commit/fc961396) Add support for Elasticsearch 8.2.0 (#254)
- [41561f81](https://github.com/stashed/installer/commit/41561f81) Update registry templates to support custom default registry (ghcr.io)



## [stashed/nats](https://github.com/stashed/nats)

### [2.6.1-v5](https://github.com/stashed/nats/releases/tag/2.6.1-v5)

- [3fe0e88](https://github.com/stashed/nats/commit/3fe0e88) Prepare for release 2.6.1-v4 (#49)


### [2.8.2](https://github.com/stashed/nats/releases/tag/2.8.2)




## [stashed/stash](https://github.com/stashed/stash)

### [v0.20.1](https://github.com/stashed/stash/releases/tag/v0.20.1)

- [9771f5db](https://github.com/stashed/stash/commit/9771f5db) Prepare for release v0.20.1 (#1446)
- [58168343](https://github.com/stashed/stash/commit/58168343) Merge pull request #1445 from stashed/fix-imagepull-secrets
- [d6290dd7](https://github.com/stashed/stash/commit/d6290dd7) Fix ImagePullSecrets not passing to the backup job properly
- [86342a7e](https://github.com/stashed/stash/commit/86342a7e) Update BackupConfiguration webhook to make the target immutable (#1444)




