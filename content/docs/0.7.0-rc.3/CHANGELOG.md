---
title: Changelog | Stash
description: Changelog
menu:
  docs_0.7.0-rc.3:
    identifier: changelog-stash
    name: Changelog
    parent: welcome
    weight: 10
product_name: stash
menu_name: docs_0.7.0-rc.3
section_menu_id: welcome
url: /docs/0.7.0-rc.3/welcome/changelog/
aliases:
- /docs/0.7.0-rc.3/CHANGELOG/
info:
  version: 0.7.0-rc.3
---

# Change Log

## [Unreleased](https://github.com/appscode/stash/tree/HEAD)

[Full Changelog](https://github.com/appscode/stash/compare/0.7.0-rc.2...HEAD)

**Fixed bugs:**

- Pod restart after each backup when Mutating Webhook enabled [\#396](https://github.com/appscode/stash/issues/396)
- Sidecar RoleBinding is not being created when Mutating Webhook is enabled  [\#395](https://github.com/appscode/stash/issues/395)
- Recovery to PVC restores data in subdirectory instead of root directory [\#392](https://github.com/appscode/stash/issues/392)
- Revendor webhook util and jsonpatch fixes [\#400](https://github.com/appscode/stash/pull/400) ([tamalsaha](https://github.com/tamalsaha))

**Closed issues:**

- hack/deploy/stash.sh: $? check does not work with set -e [\#403](https://github.com/appscode/stash/issues/403)

**Merged pull requests:**

- Prepare docs for 0.7.0-rc.3 [\#411](https://github.com/appscode/stash/pull/411) ([tamalsaha](https://github.com/tamalsaha))
- Add test for recovery [\#409](https://github.com/appscode/stash/pull/409) ([emruz-hossain](https://github.com/emruz-hossain))
- Skip setting ListKind [\#407](https://github.com/appscode/stash/pull/407) ([tamalsaha](https://github.com/tamalsaha))
- Add CRD Validation [\#406](https://github.com/appscode/stash/pull/406) ([tamalsaha](https://github.com/tamalsaha))
- Generate openapi spec for stash api [\#405](https://github.com/appscode/stash/pull/405) ([tamalsaha](https://github.com/tamalsaha))
- Fix install script for minikube 0.24.x \(Kube 1.8.0\) [\#404](https://github.com/appscode/stash/pull/404) ([tamalsaha](https://github.com/tamalsaha))
- Skip downloading onessl if already installed [\#401](https://github.com/appscode/stash/pull/401) ([tamalsaha](https://github.com/tamalsaha))
- Use Restic spec hash instead of resource version to restart pods [\#399](https://github.com/appscode/stash/pull/399) ([tamalsaha](https://github.com/tamalsaha))
- Check for valid owner object [\#397](https://github.com/appscode/stash/pull/397) ([tamalsaha](https://github.com/tamalsaha))
- Create repository crd for each Restic repository [\#394](https://github.com/appscode/stash/pull/394) ([emruz-hossain](https://github.com/emruz-hossain))
- Revendor webhook library [\#393](https://github.com/appscode/stash/pull/393) ([tamalsaha](https://github.com/tamalsaha))

## [0.7.0-rc.2](https://github.com/appscode/stash/tree/0.7.0-rc.2) (2018-03-24)
[Full Changelog](https://github.com/appscode/stash/compare/0.7.0-rc.1...0.7.0-rc.2)

**Fixed bugs:**

- Fix --enable-analytics flag [\#387](https://github.com/appscode/stash/pull/387) ([tamalsaha](https://github.com/tamalsaha))
- Fix flag parsing in tests [\#386](https://github.com/appscode/stash/pull/386) ([tamalsaha](https://github.com/tamalsaha))

**Merged pull requests:**

- Prepare docs for 0.7.0-rc.2 [\#391](https://github.com/appscode/stash/pull/391) ([tamalsaha](https://github.com/tamalsaha))
- Add variable for dockerRegistry [\#390](https://github.com/appscode/stash/pull/390) ([tamalsaha](https://github.com/tamalsaha))
- Reorg objects deleted in uninstall command [\#389](https://github.com/appscode/stash/pull/389) ([tamalsaha](https://github.com/tamalsaha))
- Fix Statefulset Example [\#385](https://github.com/appscode/stash/pull/385) ([rodrigozc](https://github.com/rodrigozc))
- Rename --analytics to --enable-analytics [\#384](https://github.com/appscode/stash/pull/384) ([tamalsaha](https://github.com/tamalsaha))
- Use separated appscode/kubernetes-webhook-util package [\#383](https://github.com/appscode/stash/pull/383) ([tamalsaha](https://github.com/tamalsaha))

## [0.7.0-rc.1](https://github.com/appscode/stash/tree/0.7.0-rc.1) (2018-03-21)
[Full Changelog](https://github.com/appscode/stash/compare/0.7.0-rc.0...0.7.0-rc.1)

**Fixed bugs:**

- Forget panics in 0.7.0-rc.0 [\#373](https://github.com/appscode/stash/issues/373)
- Don't enable mutator for StatefulSet updates [\#381](https://github.com/appscode/stash/pull/381) ([tamalsaha](https://github.com/tamalsaha))
- Stop using field selectors for CRDs [\#379](https://github.com/appscode/stash/pull/379) ([tamalsaha](https://github.com/tamalsaha))

**Closed issues:**

- "DeprecatedServiceAccount not present in src" while converting unversioned StatefulSet to v1beta1.StatefulSet  [\#371](https://github.com/appscode/stash/issues/371)
- \[0.6.x\] Helm chart broken due to undocumented '--docker-registry' and other arguments [\#354](https://github.com/appscode/stash/issues/354)
- \[0.7.0-rc.0\] Fails on start-up with 'cluster doesn't provide requestheader-client-ca-file' [\#353](https://github.com/appscode/stash/issues/353)
- Ability to backup volumes with ReadWriteOnce access mode [\#350](https://github.com/appscode/stash/issues/350)
- Convert Initializer to MutationWebhook [\#326](https://github.com/appscode/stash/issues/326)
- Recovery not working! [\#303](https://github.com/appscode/stash/issues/303)

**Merged pull requests:**

- Update the image tag in operator.yaml [\#382](https://github.com/appscode/stash/pull/382) ([tamalsaha](https://github.com/tamalsaha))
- Update docs to 0.7.0-rc.1 [\#380](https://github.com/appscode/stash/pull/380) ([tamalsaha](https://github.com/tamalsaha))
-  Add types for Repository apigroup [\#377](https://github.com/appscode/stash/pull/377) ([tamalsaha](https://github.com/tamalsaha))
- Add missing front matter [\#376](https://github.com/appscode/stash/pull/376) ([tamalsaha](https://github.com/tamalsaha))
- Check for check job before creating it [\#375](https://github.com/appscode/stash/pull/375) ([galexrt](https://github.com/galexrt))
- Add travis.yaml [\#370](https://github.com/appscode/stash/pull/370) ([tamalsaha](https://github.com/tamalsaha))
- Add --purge flag [\#369](https://github.com/appscode/stash/pull/369) ([tamalsaha](https://github.com/tamalsaha))
- Make it clear that installer is a single command [\#365](https://github.com/appscode/stash/pull/365) ([tamalsaha](https://github.com/tamalsaha))
- Update installer [\#364](https://github.com/appscode/stash/pull/364) ([tamalsaha](https://github.com/tamalsaha))
- Replace initializers with mutation webhook for workloads [\#363](https://github.com/appscode/stash/pull/363) ([emruz-hossain](https://github.com/emruz-hossain))
- Update chart to match RBAC best practices for charts [\#362](https://github.com/appscode/stash/pull/362) ([tamalsaha](https://github.com/tamalsaha))
- Add checks to installer script [\#361](https://github.com/appscode/stash/pull/361) ([tamalsaha](https://github.com/tamalsaha))
- Use admission hook helpers from kutil [\#360](https://github.com/appscode/stash/pull/360) ([tamalsaha](https://github.com/tamalsaha))
- Fix admission webhook flag [\#359](https://github.com/appscode/stash/pull/359) ([tamalsaha](https://github.com/tamalsaha))
- Support --enable-admission-webhook=false [\#358](https://github.com/appscode/stash/pull/358) ([tamalsaha](https://github.com/tamalsaha))
- Support multiple webhooks of same apiversion [\#357](https://github.com/appscode/stash/pull/357) ([tamalsaha](https://github.com/tamalsaha))
- Sync chart to stable charts repo [\#356](https://github.com/appscode/stash/pull/356) ([tamalsaha](https://github.com/tamalsaha))
- Use restic 0.8.3 [\#355](https://github.com/appscode/stash/pull/355) ([tamalsaha](https://github.com/tamalsaha))
- Update README.md [\#352](https://github.com/appscode/stash/pull/352) ([tamalsaha](https://github.com/tamalsaha))
- Set RollingUpdate for DaemonSet [\#349](https://github.com/appscode/stash/pull/349) ([tamalsaha](https://github.com/tamalsaha))

## [0.7.0-rc.0](https://github.com/appscode/stash/tree/0.7.0-rc.0) (2018-02-20)
[Full Changelog](https://github.com/appscode/stash/compare/0.6.4...0.7.0-rc.0)

**Closed issues:**

- offline backup is not supported for workload kind `Deployment`, `Replicaset` and `ReplicationController` with `replicas \> 1` [\#244](https://github.com/appscode/stash/issues/244)

**Merged pull requests:**

- Document user roles [\#348](https://github.com/appscode/stash/pull/348) ([tamalsaha](https://github.com/tamalsaha))
- Add changelog for 0.7.0-rc.0 [\#347](https://github.com/appscode/stash/pull/347) ([tamalsaha](https://github.com/tamalsaha))
- Add a parameter to allow disabling initializers [\#346](https://github.com/appscode/stash/pull/346) ([mcanevet](https://github.com/mcanevet))
- Update readme to point to 0.6.4 [\#345](https://github.com/appscode/stash/pull/345) ([tamalsaha](https://github.com/tamalsaha))
- Implement offline backup for multiple replica [\#335](https://github.com/appscode/stash/pull/335) ([emruz-hossain](https://github.com/emruz-hossain))

## [0.6.4](https://github.com/appscode/stash/tree/0.6.4) (2018-02-20)
[Full Changelog](https://github.com/appscode/stash/compare/0.6.3...0.6.4)

**Implemented enhancements:**

- Support custom CA cert with backend [\#288](https://github.com/appscode/stash/issues/288)

**Fixed bugs:**

- Backup count rises even when backup/init fails [\#293](https://github.com/appscode/stash/issues/293)

**Closed issues:**

- Use informer factory for backup scheduler [\#321](https://github.com/appscode/stash/issues/321)
- Double Deployment patch when deleting a Restic CRD? [\#281](https://github.com/appscode/stash/issues/281)
- Consider a simple 'enabled' switch for Restic CRD [\#279](https://github.com/appscode/stash/issues/279)
- Document HTTP endpoints [\#111](https://github.com/appscode/stash/issues/111)
- Support updating version of resitc side-car [\#72](https://github.com/appscode/stash/issues/72)

**Merged pull requests:**

- Update docs for 0.6.4 [\#344](https://github.com/appscode/stash/pull/344) ([tamalsaha](https://github.com/tamalsaha))
- Don't block deletion of owner by default [\#343](https://github.com/appscode/stash/pull/343) ([tamalsaha](https://github.com/tamalsaha))
- Don't block deletion of owner by default [\#342](https://github.com/appscode/stash/pull/342) ([tamalsaha](https://github.com/tamalsaha))
- Skip generating UpdateStatus method [\#341](https://github.com/appscode/stash/pull/341) ([tamalsaha](https://github.com/tamalsaha))
- Remove internal types [\#340](https://github.com/appscode/stash/pull/340) ([tamalsaha](https://github.com/tamalsaha))
- Use rbac/v1 apis [\#339](https://github.com/appscode/stash/pull/339) ([tamalsaha](https://github.com/tamalsaha))
- Add user roles [\#338](https://github.com/appscode/stash/pull/338) ([tamalsaha](https://github.com/tamalsaha))
- Use restic 0.8.2 [\#337](https://github.com/appscode/stash/pull/337) ([tamalsaha](https://github.com/tamalsaha))
- Use official code generator scripts [\#336](https://github.com/appscode/stash/pull/336) ([tamalsaha](https://github.com/tamalsaha))
- Update charts to support api registration [\#334](https://github.com/appscode/stash/pull/334) ([tamalsaha](https://github.com/tamalsaha))
- Fix e2e tests after webhook merger [\#333](https://github.com/appscode/stash/pull/333) ([tamalsaha](https://github.com/tamalsaha))
- Ensure stash can be run locally [\#332](https://github.com/appscode/stash/pull/332) ([tamalsaha](https://github.com/tamalsaha))
- Vendor client-go auth pkg [\#331](https://github.com/appscode/stash/pull/331) ([tamalsaha](https://github.com/tamalsaha))
- Update Grafana dashboard [\#330](https://github.com/appscode/stash/pull/330) ([galexrt](https://github.com/galexrt))
- Merge admission webhook and operator into one binary [\#329](https://github.com/appscode/stash/pull/329) ([tamalsaha](https://github.com/tamalsaha))
- Merge uninstall script into the stash.sh script [\#328](https://github.com/appscode/stash/pull/328) ([tamalsaha](https://github.com/tamalsaha))
- Implement informer factory for backup scheduler [\#325](https://github.com/appscode/stash/pull/325) ([emruz-hossain](https://github.com/emruz-hossain))
- Fixed abnormal pod recreation when Restic is deleted [\#322](https://github.com/appscode/stash/pull/322) ([emruz-hossain](https://github.com/emruz-hossain))
- Copy generic-admission-server into pkg [\#318](https://github.com/appscode/stash/pull/318) ([tamalsaha](https://github.com/tamalsaha))
- Use shared infromer factory [\#317](https://github.com/appscode/stash/pull/317) ([tamalsaha](https://github.com/tamalsaha))
- Use GetBaseVersion method from kutil [\#316](https://github.com/appscode/stash/pull/316) ([tamalsaha](https://github.com/tamalsaha))
- Implement Pause Restic [\#315](https://github.com/appscode/stash/pull/315) ([emruz-hossain](https://github.com/emruz-hossain))
- Fix webhook command description [\#314](https://github.com/appscode/stash/pull/314) ([tamalsaha](https://github.com/tamalsaha))
- Use rbac/v1beta1 api. [\#313](https://github.com/appscode/stash/pull/313) ([tamalsaha](https://github.com/tamalsaha))
- Support Create & Update operations in admission webhook [\#312](https://github.com/appscode/stash/pull/312) ([tamalsaha](https://github.com/tamalsaha))
- Merge webhook plugins into one. [\#311](https://github.com/appscode/stash/pull/311) ([tamalsaha](https://github.com/tamalsaha))
- Support private docker registry in installer [\#310](https://github.com/appscode/stash/pull/310) ([tamalsaha](https://github.com/tamalsaha))
- Compress go binaries [\#309](https://github.com/appscode/stash/pull/309) ([tamalsaha](https://github.com/tamalsaha))
- Rename --initializer flag to --enable-initializer [\#308](https://github.com/appscode/stash/pull/308) ([tamalsaha](https://github.com/tamalsaha))
- Remove STASH\_ROLE\_TYPE from installer scripts [\#307](https://github.com/appscode/stash/pull/307) ([tamalsaha](https://github.com/tamalsaha))
- Use rbac/v1 api [\#306](https://github.com/appscode/stash/pull/306) ([tamalsaha](https://github.com/tamalsaha))
- Use kubectl auth reconcile [\#305](https://github.com/appscode/stash/pull/305) ([tamalsaha](https://github.com/tamalsaha))
- Add --initializer flag to installer [\#304](https://github.com/appscode/stash/pull/304) ([tamalsaha](https://github.com/tamalsaha))
- Prepare docs for 0.7.0-alpha.0 [\#302](https://github.com/appscode/stash/pull/302) ([tamalsaha](https://github.com/tamalsaha))
- Change installer script [\#301](https://github.com/appscode/stash/pull/301) ([tamalsaha](https://github.com/tamalsaha))
- Added support for private docker registry [\#300](https://github.com/appscode/stash/pull/300) ([diptadas](https://github.com/diptadas))
- Add ValidatingAdmissionWebhook for Stash CRDs [\#299](https://github.com/appscode/stash/pull/299) ([tamalsaha](https://github.com/tamalsaha))
- Remove TPR to CRD migrator [\#298](https://github.com/appscode/stash/pull/298) ([tamalsaha](https://github.com/tamalsaha))
- Update dependencies to Kubernetes 1.9 [\#297](https://github.com/appscode/stash/pull/297) ([tamalsaha](https://github.com/tamalsaha))
- Write restic stderror in error events [\#296](https://github.com/appscode/stash/pull/296) ([diptadas](https://github.com/diptadas))
- Fixed backup count [\#295](https://github.com/appscode/stash/pull/295) ([diptadas](https://github.com/diptadas))
- Support self-signed ca cert for backends [\#294](https://github.com/appscode/stash/pull/294) ([emruz-hossain](https://github.com/emruz-hossain))

## [0.6.3](https://github.com/appscode/stash/tree/0.6.3) (2018-01-18)
[Full Changelog](https://github.com/appscode/stash/compare/0.6.2...0.6.3)

**Implemented enhancements:**

- Add Stash Backup Grafana dashboard to monitoring docs [\#285](https://github.com/appscode/stash/issues/285)
- Added Grafana Stash overview dashboard [\#286](https://github.com/appscode/stash/pull/286) ([galexrt](https://github.com/galexrt))

**Fixed bugs:**

- PushGateURL not given to sidecar container [\#283](https://github.com/appscode/stash/issues/283)
- Fix inline volumeSource marshalling for LocalSpec [\#289](https://github.com/appscode/stash/pull/289) ([tamalsaha](https://github.com/tamalsaha))

**Closed issues:**

- Test Failed: Invalid argument error in sidecar container [\#290](https://github.com/appscode/stash/issues/290)
- Verbosity \(--v\) flag not inherited to backup sidecars [\#282](https://github.com/appscode/stash/issues/282)

**Merged pull requests:**

- Cleanup headless service [\#292](https://github.com/appscode/stash/pull/292) ([diptadas](https://github.com/diptadas))
- Fixed parsing argument error [\#291](https://github.com/appscode/stash/pull/291) ([diptadas](https://github.com/diptadas))
- Pass through logger flags [\#287](https://github.com/appscode/stash/pull/287) ([tamalsaha](https://github.com/tamalsaha))
- Pass --pushgateway-url for injected containers. [\#284](https://github.com/appscode/stash/pull/284) ([tamalsaha](https://github.com/tamalsaha))

## [0.6.2](https://github.com/appscode/stash/tree/0.6.2) (2018-01-05)
[Full Changelog](https://github.com/appscode/stash/compare/0.6.1...0.6.2)

**Fixed bugs:**

- Created stash-sidecar clusterrole is missing statefulsets permission [\#272](https://github.com/appscode/stash/issues/272)
- Garbage collect s/a and rolebindings for \*Jobs [\#271](https://github.com/appscode/stash/issues/271)
- Fix RBAC roles in chart [\#276](https://github.com/appscode/stash/pull/276) ([tamalsaha](https://github.com/tamalsaha))
- Garbage collect service-accounts and role-bindings for jobs [\#275](https://github.com/appscode/stash/pull/275) ([diptadas](https://github.com/diptadas))
- Fix new restic format in upgrade docs [\#274](https://github.com/appscode/stash/pull/274) ([tamalsaha](https://github.com/tamalsaha))
- Add statefulsets to stash-sidecar ClusterRole creation [\#273](https://github.com/appscode/stash/pull/273) ([galexrt](https://github.com/galexrt))

**Closed issues:**

- Image kubectl not found because of Kubernetes version [\#266](https://github.com/appscode/stash/issues/266)

**Merged pull requests:**

- Prepare docs for 0.6.2 release [\#278](https://github.com/appscode/stash/pull/278) ([tamalsaha](https://github.com/tamalsaha))
- Update Helm chart to use newer 'fullname' template that avoids duplicate \(e.g. 'stash-stash-...'\) resource names [\#277](https://github.com/appscode/stash/pull/277) ([whereisaaron](https://github.com/whereisaaron))
- Reduce operator permissions for service accounts [\#270](https://github.com/appscode/stash/pull/270) ([tamalsaha](https://github.com/tamalsaha))
- Fix formatting of uninstall.md [\#269](https://github.com/appscode/stash/pull/269) ([tamalsaha](https://github.com/tamalsaha))

## [0.6.1](https://github.com/appscode/stash/tree/0.6.1) (2018-01-03)
[Full Changelog](https://github.com/appscode/stash/compare/0.6.0...0.6.1)

**Fixed bugs:**

- Error while running restic [\#256](https://github.com/appscode/stash/issues/256)

**Closed issues:**

- Unable to use non-aws S3 backend [\#226](https://github.com/appscode/stash/issues/226)

**Merged pull requests:**

- Prepare docs for 0.6.1 [\#268](https://github.com/appscode/stash/pull/268) ([tamalsaha](https://github.com/tamalsaha))

## [0.6.0](https://github.com/appscode/stash/tree/0.6.0) (2018-01-03)
[Full Changelog](https://github.com/appscode/stash/compare/0.4.2...0.6.0)

**Implemented enhancements:**

- Feature: Support offline consistent backups [\#225](https://github.com/appscode/stash/issues/225)
- Collect ideas on how to improve recovery process [\#131](https://github.com/appscode/stash/issues/131)
- Use log.LEVEL\(\) instead of fmt.Printf\(\) [\#252](https://github.com/appscode/stash/pull/252) ([galexrt](https://github.com/galexrt))

**Fixed bugs:**

- Fix ConfigMap Name in Leader Election [\#227](https://github.com/appscode/stash/issues/227)
- StatefulSet: Forbidden: pod updates may not add or remove containers [\#191](https://github.com/appscode/stash/issues/191)
- Events are not recording for Recovery [\#219](https://github.com/appscode/stash/issues/219)
- \[0.5.0\] Record backup event on kubernetes failure [\#212](https://github.com/appscode/stash/issues/212)
- Fix kubectl version parsing generation in GKE [\#267](https://github.com/appscode/stash/pull/267) ([tamalsaha](https://github.com/tamalsaha))

**Closed issues:**

- Replace fmt.Print\* with log statements [\#248](https://github.com/appscode/stash/issues/248)
- Dynamically create stash-sidecar ClusterRole in operator [\#220](https://github.com/appscode/stash/issues/220)
- LeaderElection part -2  [\#218](https://github.com/appscode/stash/issues/218)
- Reimplement CheckRecoveryJob using Job watcher [\#216](https://github.com/appscode/stash/issues/216)
- Enable --cache-dir [\#238](https://github.com/appscode/stash/issues/238)
- Upgrade procedure for 0.5.1 -\> 0.6.0 [\#237](https://github.com/appscode/stash/issues/237)
- Test RBAC setup [\#224](https://github.com/appscode/stash/issues/224)
- Record recovery status for individual FileGroup [\#213](https://github.com/appscode/stash/issues/213)
- Periodically run restic check [\#195](https://github.com/appscode/stash/issues/195)
- Handle Deployment etc with replicas \> 1 [\#140](https://github.com/appscode/stash/issues/140)
- Support Backblaze B2 as backend [\#125](https://github.com/appscode/stash/issues/125)
- Turn Stash operator into an Initializer [\#5](https://github.com/appscode/stash/issues/5)

**Merged pull requests:**

- Detect analytics client id using env vars [\#265](https://github.com/appscode/stash/pull/265) ([tamalsaha](https://github.com/tamalsaha))
- Repare docs for 0.6.0 release [\#264](https://github.com/appscode/stash/pull/264) ([tamalsaha](https://github.com/tamalsaha))
- Reorganize docs [\#263](https://github.com/appscode/stash/pull/263) ([tamalsaha](https://github.com/tamalsaha))
- Add support for B2 [\#262](https://github.com/appscode/stash/pull/262) ([tamalsaha](https://github.com/tamalsaha))
- Update restic website link [\#261](https://github.com/appscode/stash/pull/261) ([tamalsaha](https://github.com/tamalsaha))
- Update docs for unified LocalSpec [\#260](https://github.com/appscode/stash/pull/260) ([diptadas](https://github.com/diptadas))
- Unify LocalSpec and RecoveredVolume [\#259](https://github.com/appscode/stash/pull/259) ([diptadas](https://github.com/diptadas))
- Remove restic-dependency from recovery [\#258](https://github.com/appscode/stash/pull/258) ([diptadas](https://github.com/diptadas))
- Update restic version to 0.8.1 [\#257](https://github.com/appscode/stash/pull/257) ([tamalsaha](https://github.com/tamalsaha))
- Use cmp methods from kutil [\#255](https://github.com/appscode/stash/pull/255) ([tamalsaha](https://github.com/tamalsaha))
- Remove TryPatch methods [\#254](https://github.com/appscode/stash/pull/254) ([tamalsaha](https://github.com/tamalsaha))
- Log operator version on start [\#253](https://github.com/appscode/stash/pull/253) ([galexrt](https://github.com/galexrt))
- Use verb type for mutation [\#251](https://github.com/appscode/stash/pull/251) ([tamalsaha](https://github.com/tamalsaha))
- Use CreateOrPatchCronJob from kutil [\#250](https://github.com/appscode/stash/pull/250) ([tamalsaha](https://github.com/tamalsaha))
- Indicate mutation in PATCH helper method return [\#249](https://github.com/appscode/stash/pull/249) ([tamalsaha](https://github.com/tamalsaha))
- Simplify clientID generation for analytics [\#247](https://github.com/appscode/stash/pull/247) ([tamalsaha](https://github.com/tamalsaha))
- Set analytics clientID [\#246](https://github.com/appscode/stash/pull/246) ([tamalsaha](https://github.com/tamalsaha))
- Reorganize docs [\#245](https://github.com/appscode/stash/pull/245) ([tamalsaha](https://github.com/tamalsaha))
- Upgrade procedure for 0.5.1 to 0.6.0 [\#243](https://github.com/appscode/stash/pull/243) ([diptadas](https://github.com/diptadas))
- Fix retentionPolicyName not found error [\#242](https://github.com/appscode/stash/pull/242) ([diptadas](https://github.com/diptadas))
- Enable Restic cahce-dir flag [\#241](https://github.com/appscode/stash/pull/241) ([diptadas](https://github.com/diptadas))
- Use lower case workload.kind in prefix [\#240](https://github.com/appscode/stash/pull/240) ([diptadas](https://github.com/diptadas))
- Use RegisterCRDs helper [\#239](https://github.com/appscode/stash/pull/239) ([tamalsaha](https://github.com/tamalsaha))
- Update docs [\#236](https://github.com/appscode/stash/pull/236) ([diptadas](https://github.com/diptadas))
- Change left\_menu -\> menu\_name [\#235](https://github.com/appscode/stash/pull/235) ([sajibcse68](https://github.com/sajibcse68))
- Revendor dependencies [\#234](https://github.com/appscode/stash/pull/234) ([tamalsaha](https://github.com/tamalsaha))
- Add aliases for README file in front matter [\#233](https://github.com/appscode/stash/pull/233) ([sajibcse68](https://github.com/sajibcse68))
- Update bundles restic to 0.8.0 [\#232](https://github.com/appscode/stash/pull/232) ([tamalsaha](https://github.com/tamalsaha))
- Add Docs Front Matter for 0.5.1 [\#231](https://github.com/appscode/stash/pull/231) ([sajibcse68](https://github.com/sajibcse68))
- Revendor kutil [\#230](https://github.com/appscode/stash/pull/230) ([tamalsaha](https://github.com/tamalsaha))
- Implement offline backup [\#229](https://github.com/appscode/stash/pull/229) ([diptadas](https://github.com/diptadas))
- Fix Configmap Name in Leader Election [\#228](https://github.com/appscode/stash/pull/228) ([diptadas](https://github.com/diptadas))
- Run `restic check` once every 3 days [\#223](https://github.com/appscode/stash/pull/223) ([tamalsaha](https://github.com/tamalsaha))
- Record recovery status for individual FileGroup [\#222](https://github.com/appscode/stash/pull/222) ([tamalsaha](https://github.com/tamalsaha))
- Dynamically create stash-sidecar ClusterRole in operator [\#221](https://github.com/appscode/stash/pull/221) ([tamalsaha](https://github.com/tamalsaha))
- Make stash chart namespaced [\#210](https://github.com/appscode/stash/pull/210) ([tamalsaha](https://github.com/tamalsaha))
- Implement workload initializer in stash operator [\#207](https://github.com/appscode/stash/pull/207) ([diptadas](https://github.com/diptadas))
- Leader election for deployment, replica set and rc [\#206](https://github.com/appscode/stash/pull/206) ([diptadas](https://github.com/diptadas))
- Revise RetentionPolicy in Restic Api [\#205](https://github.com/appscode/stash/pull/205) ([diptadas](https://github.com/diptadas))
- Implement Recovery for Restic Backup [\#202](https://github.com/appscode/stash/pull/202) ([diptadas](https://github.com/diptadas))

## [0.4.2](https://github.com/appscode/stash/tree/0.4.2) (2017-11-03)
[Full Changelog](https://github.com/appscode/stash/compare/0.5.1...0.4.2)

**Merged pull requests:**

- Upgrade restic binary to 0.7.3 [\#209](https://github.com/appscode/stash/pull/209) ([tamalsaha](https://github.com/tamalsaha))
- Fix RBAC permission for release 0.4 [\#208](https://github.com/appscode/stash/pull/208) ([tamalsaha](https://github.com/tamalsaha))
- Change `k8s.io/api/core/v1` pkg alias to core [\#204](https://github.com/appscode/stash/pull/204) ([tamalsaha](https://github.com/tamalsaha))
- Use client-go 5.0 [\#203](https://github.com/appscode/stash/pull/203) ([tamalsaha](https://github.com/tamalsaha))
- Add recovery CRD [\#201](https://github.com/appscode/stash/pull/201) ([diptadas](https://github.com/diptadas))

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

## [0.3.1](https://github.com/appscode/stash/tree/0.3.1) (2017-07-04)
[Full Changelog](https://github.com/appscode/stash/compare/0.3.0...0.3.1)

**Merged pull requests:**

- Add tests for swift [\#130](https://github.com/appscode/stash/pull/130) ([tamalsaha](https://github.com/tamalsaha))

## [0.3.0](https://github.com/appscode/stash/tree/0.3.0) (2017-07-04)
[Full Changelog](https://github.com/appscode/stash/compare/0.2.0...0.3.0)

**Fixed bugs:**

- Fix GCS [\#122](https://github.com/appscode/stash/issues/122)

**Closed issues:**

- Support resource [\#128](https://github.com/appscode/stash/issues/128)
- Document FindRestic will match first one [\#119](https://github.com/appscode/stash/issues/119)
- Document e2e test setup process. [\#108](https://github.com/appscode/stash/issues/108)
- Fix charts [\#87](https://github.com/appscode/stash/issues/87)

**Merged pull requests:**

- Support setting compute resources for sidecar [\#129](https://github.com/appscode/stash/pull/129) ([tamalsaha](https://github.com/tamalsaha))
- Fix RBAC docs [\#127](https://github.com/appscode/stash/pull/127) ([tamalsaha](https://github.com/tamalsaha))
- Document swift [\#124](https://github.com/appscode/stash/pull/124) ([tamalsaha](https://github.com/tamalsaha))

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

## [0.1.0](https://github.com/appscode/stash/tree/0.1.0) (2017-06-27)
**Implemented enhancements:**

- Allow modifying the cron expression [\#21](https://github.com/appscode/stash/issues/21)
- Use RBAC objects for operator. [\#64](https://github.com/appscode/stash/issues/64)
- Support Azure as backup destination [\#35](https://github.com/appscode/stash/issues/35)
- Support GCS as backup destination [\#34](https://github.com/appscode/stash/issues/34)
- Change Destination definition to point to S3 [\#33](https://github.com/appscode/stash/issues/33)
- TODOs [\#22](https://github.com/appscode/stash/issues/22)
- Send performance stats to Prometheus [\#9](https://github.com/appscode/stash/issues/9)

**Fixed bugs:**

- Bubble up errors to the caller. [\#24](https://github.com/appscode/stash/issues/24)
- Fix registration of wrong group [\#39](https://github.com/appscode/stash/pull/39) ([sadlil](https://github.com/sadlil))

**Closed issues:**

- Add /snapshots endpoint in operator [\#81](https://github.com/appscode/stash/issues/81)
- CLI: restic-ctl [\#8](https://github.com/appscode/stash/issues/8)
- Sanitize metric labels [\#68](https://github.com/appscode/stash/issues/68)
- Mount an empty directory to write local files. [\#61](https://github.com/appscode/stash/issues/61)
- Support BackBlaze as backup destination [\#60](https://github.com/appscode/stash/issues/60)
- Support Swift as backup destination [\#59](https://github.com/appscode/stash/issues/59)
- Add e2e tests using Ginkgo [\#57](https://github.com/appscode/stash/issues/57)
- Review analytics [\#55](https://github.com/appscode/stash/issues/55)
- Support updated Kube object versions [\#42](https://github.com/appscode/stash/issues/42)
- Update restic to 0.6.x [\#32](https://github.com/appscode/stash/issues/32)
- Add analytics [\#31](https://github.com/appscode/stash/issues/31)
- HTTP api to exposing restic repository data [\#7](https://github.com/appscode/stash/issues/7)
- Provision new restic repositories [\#6](https://github.com/appscode/stash/issues/6)
- Proposal: Imeplement Restic TPR Resource for Kubernetes [\#1](https://github.com/appscode/stash/issues/1)

**Merged pull requests:**

- Add e2e tests for major cloud providers [\#84](https://github.com/appscode/stash/pull/84) ([tamalsaha](https://github.com/tamalsaha))
- Add /snapshots endpoint in operator [\#82](https://github.com/appscode/stash/pull/82) ([tamalsaha](https://github.com/tamalsaha))
- Handle update conflicts [\#78](https://github.com/appscode/stash/pull/78) ([tamalsaha](https://github.com/tamalsaha))
- Test e2e tests [\#76](https://github.com/appscode/stash/pull/76) ([tamalsaha](https://github.com/tamalsaha))
- Delete old testify tests [\#75](https://github.com/appscode/stash/pull/75) ([tamalsaha](https://github.com/tamalsaha))
- Create a cli wrapper for restic [\#74](https://github.com/appscode/stash/pull/74) ([tamalsaha](https://github.com/tamalsaha))
- Revise EnsureXXXSidecar methods [\#73](https://github.com/appscode/stash/pull/73) ([tamalsaha](https://github.com/tamalsaha))
- Add ginkgo based e2e tests [\#70](https://github.com/appscode/stash/pull/70) ([tamalsaha](https://github.com/tamalsaha))
- Push metrics to Prometheus push gateway [\#67](https://github.com/appscode/stash/pull/67) ([tamalsaha](https://github.com/tamalsaha))
- Use go-sh to execute restic commands [\#63](https://github.com/appscode/stash/pull/63) ([tamalsaha](https://github.com/tamalsaha))
- Add scratchDir & prefixHostname flags [\#62](https://github.com/appscode/stash/pull/62) ([tamalsaha](https://github.com/tamalsaha))
- Support remote backends [\#58](https://github.com/appscode/stash/pull/58) ([tamalsaha](https://github.com/tamalsaha))
- Organize backup code. [\#54](https://github.com/appscode/stash/pull/54) ([tamalsaha](https://github.com/tamalsaha))
- Synchronize scheduler reconfiguration [\#53](https://github.com/appscode/stash/pull/53) ([tamalsaha](https://github.com/tamalsaha))
- Fix unit tests [\#51](https://github.com/appscode/stash/pull/51) ([tamalsaha](https://github.com/tamalsaha))
- Check docker image tag before starting operator [\#45](https://github.com/appscode/stash/pull/45) ([tamalsaha](https://github.com/tamalsaha))
- Expose metrics from operator [\#44](https://github.com/appscode/stash/pull/44) ([tamalsaha](https://github.com/tamalsaha))
- Add analytics [\#41](https://github.com/appscode/stash/pull/41) ([aerokite](https://github.com/aerokite))
- Use V1alpha1SchemeGroupVersion for Restik [\#40](https://github.com/appscode/stash/pull/40) ([aerokite](https://github.com/aerokite))
- Fix status update [\#38](https://github.com/appscode/stash/pull/38) ([saumanbiswas](https://github.com/saumanbiswas))
- Upgrade restic version to 0.6.1 [\#37](https://github.com/appscode/stash/pull/37) ([tamalsaha](https://github.com/tamalsaha))
- Change api version to v1alpha1 [\#30](https://github.com/appscode/stash/pull/30) ([tamalsaha](https://github.com/tamalsaha))
- Rename function and structure [\#29](https://github.com/appscode/stash/pull/29) ([saumanbiswas](https://github.com/saumanbiswas))
- Rename Backup into Restik [\#28](https://github.com/appscode/stash/pull/28) ([saumanbiswas](https://github.com/saumanbiswas))
- Move api from k8s-addons [\#27](https://github.com/appscode/stash/pull/27) ([saumanbiswas](https://github.com/saumanbiswas))
- Bubble up errors to caller [\#26](https://github.com/appscode/stash/pull/26) ([saumanbiswas](https://github.com/saumanbiswas))
- Allow modifying the cron expression [\#25](https://github.com/appscode/stash/pull/25) ([saumanbiswas](https://github.com/saumanbiswas))
- Use unversioned time [\#23](https://github.com/appscode/stash/pull/23) ([tamalsaha](https://github.com/tamalsaha))
- Restik chart [\#20](https://github.com/appscode/stash/pull/20) ([saumanbiswas](https://github.com/saumanbiswas))
- example added [\#19](https://github.com/appscode/stash/pull/19) ([saumanbiswas](https://github.com/saumanbiswas))
- Move restik api and client to k8s-addons [\#18](https://github.com/appscode/stash/pull/18) ([saumanbiswas](https://github.com/saumanbiswas))
- Error print fix [\#17](https://github.com/appscode/stash/pull/17) ([saumanbiswas](https://github.com/saumanbiswas))
- Check group registration [\#16](https://github.com/appscode/stash/pull/16) ([saumanbiswas](https://github.com/saumanbiswas))
- Restik docs [\#15](https://github.com/appscode/stash/pull/15) ([saumanbiswas](https://github.com/saumanbiswas))
- Restik unit test, e2e test [\#14](https://github.com/appscode/stash/pull/14) ([saumanbiswas](https://github.com/saumanbiswas))
- Restik create delete initial implementation [\#12](https://github.com/appscode/stash/pull/12) ([saumanbiswas](https://github.com/saumanbiswas))
- Build docker image [\#11](https://github.com/appscode/stash/pull/11) ([tamalsaha](https://github.com/tamalsaha))
- Clone skeleton from appscode/k3pc [\#10](https://github.com/appscode/stash/pull/10) ([tamalsaha](https://github.com/tamalsaha))
- Fix e2e tests [\#83](https://github.com/appscode/stash/pull/83) ([tamalsaha](https://github.com/tamalsaha))
- Mount scratchDir with operator [\#80](https://github.com/appscode/stash/pull/80) ([tamalsaha](https://github.com/tamalsaha))
- Fix scheduler  [\#79](https://github.com/appscode/stash/pull/79) ([tamalsaha](https://github.com/tamalsaha))
- Create RBAC objects for operator [\#69](https://github.com/appscode/stash/pull/69) ([tamalsaha](https://github.com/tamalsaha))
- Mount labels using Downward api [\#66](https://github.com/appscode/stash/pull/66) ([tamalsaha](https://github.com/tamalsaha))
- Vendor go-sh dependency [\#65](https://github.com/appscode/stash/pull/65) ([tamalsaha](https://github.com/tamalsaha))
- Update e2e tests [\#52](https://github.com/appscode/stash/pull/52) ([tamalsaha](https://github.com/tamalsaha))
- Run watchers for preferred api group version kind [\#50](https://github.com/appscode/stash/pull/50) ([tamalsaha](https://github.com/tamalsaha))
- Build restic from source by default [\#49](https://github.com/appscode/stash/pull/49) ([tamalsaha](https://github.com/tamalsaha))
- Watch individual object types. [\#48](https://github.com/appscode/stash/pull/48) ([tamalsaha](https://github.com/tamalsaha))
- Various code cleanup [\#47](https://github.com/appscode/stash/pull/47) ([tamalsaha](https://github.com/tamalsaha))
- Reorganize cron controller [\#46](https://github.com/appscode/stash/pull/46) ([tamalsaha](https://github.com/tamalsaha))
- Run push gateway as a side-car for restik operator. [\#43](https://github.com/appscode/stash/pull/43) ([tamalsaha](https://github.com/tamalsaha))
- Use client-go [\#36](https://github.com/appscode/stash/pull/36) ([tamalsaha](https://github.com/tamalsaha))



\* *This Change Log was automatically generated by [github_changelog_generator](https://github.com/skywinder/Github-Changelog-Generator)*