---
title: Changelog | Stash
description: Changelog
menu:
  docs_v2022.09.29:
    identifier: changelog-stash-0.4
    name: Changelog-0.4
    parent: welcome
    weight: 40
product_name: stash
menu_name: docs_v2022.09.29
section_menu_id: welcome
url: /docs/v2022.09.29/welcome/changelog-0.4/
aliases:
- /docs/v2022.09.29/CHANGELOG-0.4/
info:
  cli: v0.23.0
  community: v0.23.0
  elasticsearch:
  - 5.6.4-v19
  - 6.2.4-v19
  - 6.3.0-v19
  - 6.4.0-v19
  - 6.5.3-v19
  - 6.8.0-v19
  - 7.14.0-v5
  - 7.2.0-v19
  - 7.3.2-v19
  - 8.2.0-v2
  enterprise: v0.23.0
  etcd:
  - 3.5.0-v6
  installer: v2022.09.29
  kubedump:
  - 0.1.0-v2
  mariadb:
  - 10.5.8-v12
  mongodb:
  - 3.4.17-v19
  - 3.4.22-v19
  - 3.6.13-v19
  - 3.6.8-v19
  - 4.0.11-v19
  - 4.0.3-v19
  - 4.0.5-v19
  - 4.1.13-v19
  - 4.1.4-v19
  - 4.1.7-v19
  - 4.2.3-v19
  - 4.4.6-v10
  - 5.0.3-v7
  mysql:
  - 5.7.25-v19
  - 8.0.14-v19
  - 8.0.21-v13
  - 8.0.3-v19
  nats:
  - 2.6.1-v7
  - 2.8.2-v2
  percona-xtradb:
  - 5.7-v14
  postgres:
  - 10.14-v18
  - 11.9-v18
  - 12.4-v18
  - 13.1-v15
  - 14.0-v7
  - 9.6.19-v18
  redis:
  - 5.0.13-v7
  - 6.2.5-v7
  - 7.0.5
  ui-server: v0.5.0
  version: v2022.09.29
---

# Change Log

## [0.4.2](https://github.com/appscode/stash/tree/0.4.2) (2017-11-03)
[Full Changelog](https://github.com/appscode/stash/compare/0.5.1...0.4.2)

**Merged pull requests:**

- Upgrade restic binary to 0.7.3 [\#209](https://github.com/appscode/stash/pull/209) ([tamalsaha](https://github.com/tamalsaha))
- Fix RBAC permission for release 0.4 [\#208](https://github.com/appscode/stash/pull/208) ([tamalsaha](https://github.com/tamalsaha))
- Change `k8s.io/api/core/v1` pkg alias to core [\#204](https://github.com/appscode/stash/pull/204) ([tamalsaha](https://github.com/tamalsaha))
- Use client-go 5.0 [\#203](https://github.com/appscode/stash/pull/203) ([tamalsaha](https://github.com/tamalsaha))
- Add recovery CRD [\#201](https://github.com/appscode/stash/pull/201) ([diptadas](https://github.com/diptadas))


## [0.4.1](https://github.com/appscode/stash/tree/0.4.1) (2017-07-19)
[Full Changelog](https://github.com/appscode/stash/compare/0.4.0...0.4.1)

**Fixed bugs:**

- Fix Fake restic resource Url [\#137](https://github.com/appscode/stash/pull/137) ([sadlil](https://github.com/sadlil))

## [0.4.0](https://github.com/appscode/stash/tree/0.4.0) (2017-07-07)
[Full Changelog](https://github.com/appscode/stash/compare/0.3.1...0.4.0)

**Closed issues:**

- Use osm as the bucket manipulator [\#3](https://github.com/appscode/stash/issues/3)
- Update restic [\#133](https://github.com/appscode/stash/issues/133)
- Document required RBAC permissions [\#123](https://github.com/appscode/stash/issues/123)

**Merged pull requests:**

- Rename RepositorySecretName to StorageSecretName [\#135](https://github.com/appscode/stash/pull/135) ([tamalsaha](https://github.com/tamalsaha))
- Rename Volume to VolumeSource [\#134](https://github.com/appscode/stash/pull/134) ([tamalsaha](https://github.com/tamalsaha))
- Use VolumeSource instead of Volume for Local backend. [\#132](https://github.com/appscode/stash/pull/132) ([tamalsaha](https://github.com/tamalsaha))
