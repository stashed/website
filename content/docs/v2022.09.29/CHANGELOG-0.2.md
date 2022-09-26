---
title: Changelog | Stash
description: Changelog
menu:
  docs_v2022.09.29:
    identifier: changelog-stash-0.2
    name: Changelog-0.2
    parent: welcome
    weight: 20
product_name: stash
menu_name: docs_v2022.09.29
section_menu_id: welcome
url: /docs/v2022.09.29/welcome/changelog-0.2/
aliases:
- /docs/v2022.09.29/CHANGELOG-0.2/
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

## [0.2.0](https://github.com/appscode/stash/tree/0.2.0) (2017-06-30)
[Full Changelog](https://github.com/appscode/stash/compare/0.1.0...0.2.0)

**Implemented enhancements:**

- Don't run forget if missing retention policy [\#100](https://github.com/appscode/stash/issues/100)
- Move prefix-hostname to Restic tpr [\#96](https://github.com/appscode/stash/issues/96)

**Fixed bugs:**

- Mount source volume [\#112](https://github.com/appscode/stash/issues/112)
- Test restic URL is generated correctly when optional parts are missing [\#98](https://github.com/appscode/stash/issues/98)
- Handle updated restic selectors [\#95](https://github.com/appscode/stash/issues/95)

**Closed issues:**

- Link to sidecar flags. [\#109](https://github.com/appscode/stash/issues/109)
- Link back to tutorial from docs pages. [\#107](https://github.com/appscode/stash/issues/107)
- Document various implications of Restic update [\#103](https://github.com/appscode/stash/issues/103)
- Add retention policy options [\#101](https://github.com/appscode/stash/issues/101)
- Handle updating local backend. [\#105](https://github.com/appscode/stash/issues/105)
- Set Temp dir ENV var [\#102](https://github.com/appscode/stash/issues/102)
- Cleanup documentation [\#86](https://github.com/appscode/stash/issues/86)
- Updating Local backend does not update pods. [\#71](https://github.com/appscode/stash/issues/71)

**Merged pull requests:**

- Part 6 - Update docs [\#121](https://github.com/appscode/stash/pull/121) ([tamalsaha](https://github.com/tamalsaha))
- Update docs [\#120](https://github.com/appscode/stash/pull/120) ([tamalsaha](https://github.com/tamalsaha))
- Various bug fixes [\#118](https://github.com/appscode/stash/pull/118) ([tamalsaha](https://github.com/tamalsaha))
- Update pitch [\#117](https://github.com/appscode/stash/pull/117) ([tamalsaha](https://github.com/tamalsaha))
- Part 5 - User Guide [\#114](https://github.com/appscode/stash/pull/114) ([tamalsaha](https://github.com/tamalsaha))
- Part 4- User Guide [\#113](https://github.com/appscode/stash/pull/113) ([tamalsaha](https://github.com/tamalsaha))
- Part 3 - User Guide [\#110](https://github.com/appscode/stash/pull/110) ([tamalsaha](https://github.com/tamalsaha))
- Update user guide [\#94](https://github.com/appscode/stash/pull/94) ([tamalsaha](https://github.com/tamalsaha))
- Create separate restic for each type of backend. [\#92](https://github.com/appscode/stash/pull/92) ([tamalsaha](https://github.com/tamalsaha))
- Remove selectors so that `template.metadata.labels` are used [\#91](https://github.com/appscode/stash/pull/91) ([tamalsaha](https://github.com/tamalsaha))
- Update Stash chart [\#89](https://github.com/appscode/stash/pull/89) ([tamalsaha](https://github.com/tamalsaha))
- Various changes to RetentionPolicy	 [\#116](https://github.com/appscode/stash/pull/116) ([tamalsaha](https://github.com/tamalsaha))
- Set TMPDIR env var for restic [\#115](https://github.com/appscode/stash/pull/115) ([tamalsaha](https://github.com/tamalsaha))
- Part - 2 of User guide [\#99](https://github.com/appscode/stash/pull/99) ([tamalsaha](https://github.com/tamalsaha))
- Update Prometheus job name to use restic ns & name [\#93](https://github.com/appscode/stash/pull/93) ([tamalsaha](https://github.com/tamalsaha))
- Add docs for commands [\#90](https://github.com/appscode/stash/pull/90) ([tamalsaha](https://github.com/tamalsaha))
- Fix dev guide [\#88](https://github.com/appscode/stash/pull/88) ([tamalsaha](https://github.com/tamalsaha))
