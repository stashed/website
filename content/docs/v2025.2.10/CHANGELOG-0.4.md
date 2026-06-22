---
title: Changelog | Stash
description: Changelog
menu:
  docs_v2025.2.10:
    identifier: changelog-stash-0.4
    name: Changelog-0.4
    parent: welcome
    weight: 40
product_name: stash
menu_name: docs_v2025.2.10
section_menu_id: welcome
url: /docs/v2025.2.10/welcome/changelog-0.4/
aliases:
- /docs/v2025.2.10/CHANGELOG-0.4/
info:
  cli: v0.39.0
  community: v0.39.0
  elasticsearch:
  - 5.6.4-v35
  - 6.2.4-v35
  - 6.3.0-v35
  - 6.4.0-v35
  - 6.5.3-v35
  - 6.8.0-v35
  - 7.14.0-v21
  - 7.2.0-v35
  - 7.3.2-v35
  - 8.2.0-v18
  enterprise: v0.39.0
  etcd:
  - 3.5.0-v22
  installer: v2025.2.10
  kubedump:
  - 0.2.0-v3
  mariadb:
  - 10.5.8-v29
  mongodb:
  - 3.4.17-v36
  - 3.4.22-v36
  - 3.6.13-v36
  - 3.6.8-v36
  - 4.0.11-v36
  - 4.0.3-v36
  - 4.0.5-v36
  - 4.1.13-v36
  - 4.1.4-v36
  - 4.1.7-v36
  - 4.2.3-v36
  - 4.4.6-v27
  - 5.0.15-v9
  - 5.0.3-v24
  - 6.0.5-v12
  mysql:
  - 5.7.25-v36
  - 8.0.14-v35
  - 8.0.21-v29
  - 8.0.3-v35
  nats:
  - 2.6.1-v23
  - 2.8.2-v18
  percona-xtradb:
  - 5.7-v30
  postgres:
  - 10.14-v34
  - 11.9-v34
  - 12.4-v34
  - 13.1-v31
  - 14.0-v23
  - 15.1-v15
  - 16.1-v4
  - 17.2-v2
  - 9.6.19-v34
  redis:
  - 5.0.13-v23
  - 6.2.5-v23
  - 7.0.5-v16
  ui-server: v0.20.0
  vault:
  - 1.10.3-v15
  version: v2025.2.10
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
