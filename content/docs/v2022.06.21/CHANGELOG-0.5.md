---
title: Changelog | Stash
description: Changelog
menu:
  docs_v2022.06.21:
    identifier: changelog-stash-0.5
    name: Changelog-0.5
    parent: welcome
    weight: 50
product_name: stash
menu_name: docs_v2022.06.21
section_menu_id: welcome
url: /docs/v2022.06.21/welcome/changelog-0.5/
aliases:
- /docs/v2022.06.21/CHANGELOG-0.5/
info:
  cli: v0.21.0
  community: v0.21.0
  elasticsearch:
  - 5.6.4-v18
  - 6.2.4-v18
  - 6.3.0-v18
  - 6.4.0-v18
  - 6.5.3-v18
  - 6.8.0-v18
  - 7.14.0-v4
  - 7.2.0-v18
  - 7.3.2-v18
  - 8.2.0-v1
  enterprise: v0.21.0
  etcd:
  - 3.5.0-v5
  installer: v2022.06.21
  kubedump:
  - 0.1.0-v1
  mariadb:
  - 10.5.8-v11
  mongodb:
  - 3.4.17-v17
  - 3.4.22-v17
  - 3.6.13-v17
  - 3.6.8-v17
  - 4.0.11-v17
  - 4.0.3-v17
  - 4.0.5-v17
  - 4.1.13-v17
  - 4.1.4-v17
  - 4.1.7-v17
  - 4.2.3-v17
  - 4.4.6-v8
  - 5.0.3-v5
  mysql:
  - 5.7.25-v18
  - 8.0.14-v18
  - 8.0.21-v12
  - 8.0.3-v18
  nats:
  - 2.6.1-v6
  - 2.8.2-v1
  percona-xtradb:
  - 5.7-v13
  postgres:
  - 10.14-v16
  - 11.9-v16
  - 12.4-v16
  - 13.1-v13
  - 14.0-v5
  - 9.6.19-v16
  redis:
  - 5.0.13-v6
  - 6.2.5-v6
  ui-server: v0.3.0
  version: v2022.06.21
---

# Change Log

## [0.5.1](https://github.com/appscode/stash/tree/0.5.1) (2017-10-10)
[Full Changelog](https://github.com/appscode/stash/compare/0.5.0...0.5.1)

**Fixed bugs:**

- invalid header field value for key Authorization - DO s3 bucket [\#189](https://github.com/appscode/stash/issues/189)
- Kops + AWS: cannot unmarshal array into Go value of type types.ContainerJSON [\#147](https://github.com/appscode/stash/issues/147)

**Closed issues:**

- Cut a new release with restic 0.7.1 [\#145](https://github.com/appscode/stash/issues/145)
- Use fixed Hostname for ReplicaSet etc [\#165](https://github.com/appscode/stash/issues/165)
- Update docs for restic tags [\#143](https://github.com/appscode/stash/issues/143)
- Document how to use with kubectl [\#142](https://github.com/appscode/stash/issues/142)

**Merged pull requests:**

- Correctly detect "default" service account [\#200](https://github.com/appscode/stash/pull/200) ([tamalsaha](https://github.com/tamalsaha))
- Clarify that --tag foo,tag bar style tags are not supported. [\#199](https://github.com/appscode/stash/pull/199) ([tamalsaha](https://github.com/tamalsaha))
- Set hostname based on resource type [\#198](https://github.com/appscode/stash/pull/198) ([tamalsaha](https://github.com/tamalsaha))
- Document how to detect operator version [\#196](https://github.com/appscode/stash/pull/196) ([tamalsaha](https://github.com/tamalsaha))
- Manage RoleBinding for rbac enabled cluster [\#197](https://github.com/appscode/stash/pull/197) ([tamalsaha](https://github.com/tamalsaha))

## [0.5.0](https://github.com/appscode/stash/tree/0.5.0) (2017-10-10)
[Full Changelog](https://github.com/appscode/stash/compare/0.5.0-beta.3...0.5.0)

**Closed issues:**

- Apply restic.appscode.com/config annotations on Pod templates [\#141](https://github.com/appscode/stash/issues/141)

**Merged pull requests:**

- Revendor forked robfig/cron [\#139](https://github.com/appscode/stash/pull/139) ([tamalsaha](https://github.com/tamalsaha))

## [0.5.0-beta.3](https://github.com/appscode/stash/tree/0.5.0-beta.3) (2017-10-10)
[Full Changelog](https://github.com/appscode/stash/compare/0.5.0-beta.2...0.5.0-beta.3)

**Merged pull requests:**

- Use workqueue for scheduler [\#194](https://github.com/appscode/stash/pull/194) ([tamalsaha](https://github.com/tamalsaha))

## [0.5.0-beta.2](https://github.com/appscode/stash/tree/0.5.0-beta.2) (2017-10-09)
[Full Changelog](https://github.com/appscode/stash/compare/0.5.0-beta.1...0.5.0-beta.2)

**Merged pull requests:**

- Add tests for DO [\#193](https://github.com/appscode/stash/pull/193) ([tamalsaha](https://github.com/tamalsaha))
- Update tutorial [\#186](https://github.com/appscode/stash/pull/186) ([diptadas](https://github.com/diptadas))

## [0.5.0-beta.1](https://github.com/appscode/stash/tree/0.5.0-beta.1) (2017-10-09)
[Full Changelog](https://github.com/appscode/stash/compare/0.5.0-beta.0...0.5.0-beta.1)

**Fixed bugs:**

- \[Bug\] Success/Fail prometheus metrics inverted condition [\#175](https://github.com/appscode/stash/issues/175)

**Closed issues:**

- `should backup new DaemonSet` failed [\#155](https://github.com/appscode/stash/issues/155)
- Use DaemonSet update \(1.6\) [\#154](https://github.com/appscode/stash/issues/154)

**Merged pull requests:**

- Fix prometheus metrics collection [\#192](https://github.com/appscode/stash/pull/192) ([tamalsaha](https://github.com/tamalsaha))
- Fix StatefulSet tests [\#190](https://github.com/appscode/stash/pull/190) ([tamalsaha](https://github.com/tamalsaha))
- Replace reflect.Equal with github.com/google/go-cmp [\#188](https://github.com/appscode/stash/pull/188) ([tamalsaha](https://github.com/tamalsaha))
- Skip ReplicaSet owned by Deployments [\#187](https://github.com/appscode/stash/pull/187) ([tamalsaha](https://github.com/tamalsaha))

## [0.5.0-beta.0](https://github.com/appscode/stash/tree/0.5.0-beta.0) (2017-10-09)
[Full Changelog](https://github.com/appscode/stash/compare/0.4.1...0.5.0-beta.0)

**Implemented enhancements:**

- Migrate TPR to CRD [\#160](https://github.com/appscode/stash/pull/160) ([sadlil](https://github.com/sadlil))

**Fixed bugs:**

- Error in request: v1.ListOptions is not suitable for converting to "v1" [\#153](https://github.com/appscode/stash/issues/153)
- Fix client-go updates [\#159](https://github.com/appscode/stash/pull/159) ([sadlil](https://github.com/sadlil))

**Closed issues:**

- Switch to CustomResourceDefinitions [\#97](https://github.com/appscode/stash/issues/97)
- Use client-go 4.0.0 [\#56](https://github.com/appscode/stash/issues/56)

**Merged pull requests:**

- Prepare docs for 5.0.0-beta.0 [\#185](https://github.com/appscode/stash/pull/185) ([tamalsaha](https://github.com/tamalsaha))
- Set namespaceIndex as indexer [\#184](https://github.com/appscode/stash/pull/184) ([tamalsaha](https://github.com/tamalsaha))
- Fix e2e tests [\#183](https://github.com/appscode/stash/pull/183) ([tamalsaha](https://github.com/tamalsaha))
- Use workqueue [\#182](https://github.com/appscode/stash/pull/182) ([tamalsaha](https://github.com/tamalsaha))
- Use Deployment from apps/v1beta1 [\#181](https://github.com/appscode/stash/pull/181) ([tamalsaha](https://github.com/tamalsaha))
- Delete \*.generated.go files for ugorji [\#180](https://github.com/appscode/stash/pull/180) ([tamalsaha](https://github.com/tamalsaha))
- Use WaitForCRDReady from kutil [\#179](https://github.com/appscode/stash/pull/179) ([tamalsaha](https://github.com/tamalsaha))
- Only watch apps/v1beta1 Deployment [\#178](https://github.com/appscode/stash/pull/178) ([tamalsaha](https://github.com/tamalsaha))
- Move kutil to client package [\#177](https://github.com/appscode/stash/pull/177) ([tamalsaha](https://github.com/tamalsaha))
- Generate ugorji stuff [\#176](https://github.com/appscode/stash/pull/176) ([tamalsaha](https://github.com/tamalsaha))
- Prepare docs for 0.5.0 [\#174](https://github.com/appscode/stash/pull/174) ([tamalsaha](https://github.com/tamalsaha))
- Install stash as a critical addon [\#173](https://github.com/appscode/stash/pull/173) ([tamalsaha](https://github.com/tamalsaha))
- Set RESTIC\_VER to 0.7.3 [\#172](https://github.com/appscode/stash/pull/172) ([tamalsaha](https://github.com/tamalsaha))
- Refresh charts to match recent convention [\#171](https://github.com/appscode/stash/pull/171) ([tamalsaha](https://github.com/tamalsaha))
- Update kutil [\#170](https://github.com/appscode/stash/pull/170) ([tamalsaha](https://github.com/tamalsaha))
- Fix deployment name in tutorial [\#169](https://github.com/appscode/stash/pull/169) ([the-redback](https://github.com/the-redback))
- Fix command in Developer-guide [\#168](https://github.com/appscode/stash/pull/168) ([the-redback](https://github.com/the-redback))
- Use apis/v1alpha1 instead of internal version [\#167](https://github.com/appscode/stash/pull/167) ([tamalsaha](https://github.com/tamalsaha))
- Remove resource:path [\#166](https://github.com/appscode/stash/pull/166) ([tamalsaha](https://github.com/tamalsaha))
- Move analytics collector to root command [\#164](https://github.com/appscode/stash/pull/164) ([tamalsaha](https://github.com/tamalsaha))
- Use kubernetes/code-generator [\#163](https://github.com/appscode/stash/pull/163) ([tamalsaha](https://github.com/tamalsaha))
- Revendor k8s.io/apiextensions-apiserver [\#162](https://github.com/appscode/stash/pull/162) ([tamalsaha](https://github.com/tamalsaha))
- Update kutil dependency [\#158](https://github.com/appscode/stash/pull/158) ([tamalsaha](https://github.com/tamalsaha))
- Use CheckAPIVersion\(\) [\#157](https://github.com/appscode/stash/pull/157) ([tamalsaha](https://github.com/tamalsaha))
- Use PATCH api instead of UPDATE [\#156](https://github.com/appscode/stash/pull/156) ([tamalsaha](https://github.com/tamalsaha))
- Check version using semver library [\#152](https://github.com/appscode/stash/pull/152) ([tamalsaha](https://github.com/tamalsaha))
- Support adding Sidecar containers for StatefulSet. [\#151](https://github.com/appscode/stash/pull/151) ([tamalsaha](https://github.com/tamalsaha))
- Update client-go to 4.0.0 [\#150](https://github.com/appscode/stash/pull/150) ([tamalsaha](https://github.com/tamalsaha))
- Update build commands for restic. [\#149](https://github.com/appscode/stash/pull/149) ([tamalsaha](https://github.com/tamalsaha))
- Update client-go to 3.0.0 from 3.0.0-beta [\#148](https://github.com/appscode/stash/pull/148) ([tamalsaha](https://github.com/tamalsaha))
- Add uninstall.sh script [\#144](https://github.com/appscode/stash/pull/144) ([tamalsaha](https://github.com/tamalsaha))
- Fix typos of tutorial.md file [\#138](https://github.com/appscode/stash/pull/138) ([sajibcse68](https://github.com/sajibcse68))
