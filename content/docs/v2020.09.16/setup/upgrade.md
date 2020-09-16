---
title: Upgrade | Stash
description: Stash Upgrade
menu:
  docs_v2020.09.16:
    identifier: upgrade-stash
    name: Upgrade
    parent: setup
    weight: 30
product_name: stash
menu_name: docs_v2020.09.16
section_menu_id: setup
info:
  catalog: v2020.09.16
  cli: v0.11.0
  community: v0.11.0
  elasticsearch:
  - 5.6.4-v2
  - 6.2.4-v2
  - 6.3.0-v2
  - 6.4.0-v2
  - 6.5.3-v2
  - 6.8.0-v2
  - 7.2.0-v2
  - 7.3.2-v2
  enterprise: v0.11.0
  mongodb:
  - 3.4.1-v2
  - 3.4.2-v2
  - 3.6.1-v2
  - 3.6.8-v2
  - 4.0.11-v2
  - 4.0.3-v2
  - 4.0.5-v2
  - 4.1.1-v2
  - 4.1.4-v2
  - 4.1.7-v2
  - 4.2.3-v2
  mysql:
  - 5.7.25-v2
  - 8.0.14-v2
  - 8.0.3-v2
  percona-xtradb:
  - 5.7-v2
  postgres:
  - 10.14.0
  - 11.9.0
  - 12.4.0
  - 9.6.19
  version: v2020.09.16
---

# Upgrading Stash

This guide will show you how to upgrade Stash operator. Here, we are going to show how to update the license and how to upgrade between two Stash versions.

## Updating License

Stash support updating license without requiring any re-installation or restart. Stash creates a Secret named `<helm release name>-license` with the license file. You just need to update the Secret. The changes will propagate automatically to the operator and it will use the updated license going forward.

Follow the below instructions to update the license:

- Get a new license and save it into a file.
- Then, run the following upgrade command based on your installation.

<ul class="nav nav-tabs" id="installerTab" role="tablist">
  <li class="nav-item">
    <a class="nav-link active" id="helm3-tab" data-toggle="tab" href="#helm3" role="tab" aria-controls="helm3" aria-selected="true">Helm 3</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" id="helm2-tab" data-toggle="tab" href="#helm2" role="tab" aria-controls="helm2" aria-selected="false">Helm 2</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" id="script-tab" data-toggle="tab" href="#script" role="tab" aria-controls="script" aria-selected="false">YAML</a>
  </li>
</ul>
<div class="tab-content" id="installerTabContent">
  <div class="tab-pane fade show active" id="helm3" role="tabpanel" aria-labelledby="helm3-tab">

## Using Helm 3

```bash
$ helm upgrade stash-enterprise -n kube-system appscode/stash-enterprise  \
    --reuse-values                                                        \
    --set-file license=/path/to/new/license.txt
```

</div>
<div class="tab-pane fade" id="helm2" role="tabpanel" aria-labelledby="helm2-tab">

## Using Helm 2

```bash
$ helm upgrade stash-enterprise appscode/stash-enterprise  \
    --reuse-values                                         \
    --set-file license=/path/to/new/license.txt
```

</div>
<div class="tab-pane fade" id="script" role="tabpanel" aria-labelledby="script-tab">

## Using YAML (with helm 3)

```bash
$ helm template stash-enterprise appscode/stash-enterprise  \
    --set-file license=/path/to/new/license.txt             \
    --show-only templates/license.yaml                      \
    --no-hooks | kubectl apply -f -
```

</div>
</div>

## Upgrading Between Community Edition and Enterprise Edition

Stash uses two different binaries for Community edition and Enterprise edition. So, it is not possible to upgrade between the Community edition and Enterprise edition without re-installation. However, it is possible to re-install Stash without losing the existing backup resources.

Follow the below instructions to re-install Stash:

- Uninstall the old version by following the respective uninstallation guide. Don't delete the CRDs.
- Install the new version by following the respective installation guide.

## Upgrading between patch versions

If you are upgrading Stash to a patch release, please reapply the [installation instructions](/docs/v2020.09.16/setup/README). That will upgrade the operator pod to the new version and fix any RBAC issues.

## Upgrading from 0.9.x to v2020.x.x

If you are upgrading from `0.9.x` which did not use license verification to new `v2020.x.x`, you have to first uninstall the old version. Then, you have to re-install the new version.

If you are upgrading from `0.9.x` to `v2020.x.x` Community edition, please note that following features are only available in Enterprise edition:

- **Auto-Backup:** Auto-backup is now an enterprise feature. You won't be able to setup any new backup using auto-backup. However, your existing auto-backup resources should keep functioning.
- **Batch Backup:** Batch backup and restore is also now an enterprise feature. You won't be able to create any new backup using batch-backup. However, your existing backup should continue to work and you would be able to restore the data that were backed up using BatchBackup.
- **Local Backend:** Local backend now is an enterprise feature. If you are using any Kubernetes volume (i.e. NFS, PVC, HostPath, etc.) as backend, you won't be able to create any new backup using those backends. However, your existing backup that uses sidecar model should keep functioning. You have to use the Enterprise edition to restore from the backed up data. If you are interested in purchasing Enterprise license, please contact us via sales@appscode.com for further discussion. You can also set up a meeting via our [calendly link](https://calendly.com/appscode/30min).

If you are using any Stash addons, you might need to update the `Task` name in your `BackupConfiguration` to comply with the new naming scheme of the `Function` and `Task`.
