---
title: Upgrade | Stash
description: Stash Upgrade
menu:
  docs_v2024.9.30:
    identifier: upgrade-stash
    name: Upgrade
    parent: setup
    weight: 30
product_name: stash
menu_name: docs_v2024.9.30
section_menu_id: setup
info:
  cli: v0.36.0
  community: v0.36.0
  elasticsearch:
  - 5.6.4-v32
  - 6.2.4-v32
  - 6.3.0-v32
  - 6.4.0-v32
  - 6.5.3-v32
  - 6.8.0-v32
  - 7.14.0-v18
  - 7.2.0-v32
  - 7.3.2-v32
  - 8.2.0-v15
  enterprise: v0.36.0
  etcd:
  - 3.5.0-v19
  installer: v2024.9.30
  kubedump:
  - 0.1.0-v15
  mariadb:
  - 10.5.8-v26
  mongodb:
  - 3.4.17-v33
  - 3.4.22-v33
  - 3.6.13-v33
  - 3.6.8-v33
  - 4.0.11-v33
  - 4.0.3-v33
  - 4.0.5-v33
  - 4.1.13-v33
  - 4.1.4-v33
  - 4.1.7-v33
  - 4.2.3-v33
  - 4.4.6-v24
  - 5.0.15-v6
  - 5.0.3-v21
  - 6.0.5-v9
  mysql:
  - 5.7.25-v32
  - 8.0.14-v32
  - 8.0.21-v26
  - 8.0.3-v32
  nats:
  - 2.6.1-v20
  - 2.8.2-v15
  perconaxtradb:
  - 5.7-v27
  postgres:
  - 10.14-v31
  - 11.9-v31
  - 12.4-v31
  - 13.1-v28
  - 14.0-v20
  - 15.1-v12
  - 16.1-v1
  - 9.6.19-v31
  redis:
  - 5.0.13-v20
  - 6.2.5-v20
  - 7.0.5-v13
  ui-server: v0.17.0
  vault:
  - 1.10.3-v12
  version: v2024.9.30
---

# Upgrading Stash

This guide will show you how to upgrade various Stash components. Here, we are going to show how to upgrade from an old Stash version to the new version, and how to update the license, etc.

## Upgrading Stash from `v2021.xx.xx` to `{{< param "info.version" >}}`

In order to upgrade from Stash `v2021.xx.xx` to `{{< param "info.version" >}}`, please follow the following steps.

#### 1. Update Stash catalog CRDs

Helm [does not upgrade the CRDs](https://github.com/helm/helm/issues/6581) bundled in a Helm chart if the CRDs already exist. So, to upgrade the Stash catalog CRDs, please run the following commands below:

```bash
# Update catalog CRDs
$ kubectl apply -f https://github.com/stashed/installer/raw/{{< param "info.version" >}}/crds/stash-catalog-crds.yaml

# Update metrics CRDs
$ kubectl apply -f https://github.com/stashed/installer/raw/{{< param "info.version" >}}/charts/stash-metrics/crds/metrics.appscode.com_metricsconfigurations.yaml
```

#### 2. Upgrade Stash Operator

Now, upgrade the Stash helm chart using the following command. You can find the latest installation guide [here](/docs/v2024.9.30/setup/README).

```bash
$ helm upgrade stash oci://ghcr.io/appscode-charts/stash \
    --version {{< param "info.version" >}} \
    --namespace stash \
    --set features.enterprise=true \
    --set-file global.license=/path/to/the/license.txt \
    --wait --burst-limit=10000 --debug
```

#### 3. Post uprade cleanup

We have changed the name format for auto-backup resources to support dedicated backup or storage namespace. If you were using a dedicated storage namespace for your auto-backup through the `repoNamespace` field, you might see a new BackupConfiguration and Repository has been created with a new name format. Unfortunately, Stash can't remove the old BackupConfiguration and Repository before creating a new one due to a flaw in how we handled them previously. In this case, you can safely remove the old BackupConfiguration and Repository. Your backed-up data will be intact.

To cleanup the BackupConfigurations and the Repositories,

```bash
# Delete a particular backupconfiguration
$ kubectl delete backupconfiguration -n <namespace_name> <backupconfiguration_name>

# Delete a particular repository
$ kubectl delete repository -n <namespace_name> <repository_name>
```

## Upgrading Stash from `v0.11.x` and older to `v0.12.x`

In Stash `v0.11.x` and prior versions, Stash used separate charts for Stash community edition, Stash enterprise edition, and Stash Addons catalogs. In Stash `v0.12.x`, we have moved to a single combined chart for all the components for a better user experience. It also removes the burden of installing individual database addons manually.

In order to upgrade from Stash `v0.11.x` to `v0.12.x`, please follow the following steps.

#### 1. Pause Upcoming Backups

The entire upgrade process may take few minutes depending on your configurations. If you have any scheduled backup that can trigger during the upgrade process, you are advised to pause them before starting the upgrade process.

You can pause a backup by setting `spec.paused` field of a `BackupConfiguration` to `true` as below,

```bash
kubectl patch backupconfiguration -n <namespace> <BackupConfiguration name> --type="merge" --patch='{"spec": {"paused": true}}'
```

>If you don't pause the backup, then any backup that has been triggered during the upgrade process should start executing as soon as the upgrade is completed. So, if multiple backups of different targets are triggered during the upgrade process, they all will start executing simultaneously after the upgrade. If multiple backups of the same target are triggered during the upgrade process, only the first one will be executed and others will be skipped.

#### 2. Uninstall Stash Addons

Now, if you have installed Stash database addons, then uninstall them by following their uninstallation guide. All the database addons will be created automatically when you upgrade to the new version of the Stash.

Make sure to use the appropriate uninstallation guide for the addon version that you are currently using. You can use the dropdown on the left sidebar of the documentation site to navigate to the documentation for previous versions.

<figure align="center">
  <img alt="Naviage to old documentation" src="/docs/v2024.9.30/setup/upgrade/images/stash-navigate-old-version.png">
  <figcaption align="center">Fig: Naviage to old documentation</figcaption>
</figure>

>New documentation does not contain the installation/uninstallation guide for the Addons as they are now automatically installed/uninstalled along with the Stash operator.

#### 3. Uninstall Stash Operator

Now, uninstall the Stash operator by following the appropriate uninstallation guide of the Stash version that you are currently running.

>Make sure you are using the appropriate version of the uninstallation guide. The uninstallation guide for `v0.12.x` will not work for `v0.11.x` Use the dropdown at the sidebar of the documentation site to navigate to the appropriate version that you are currently running.

#### 5. Update CRDs

 When you uninstall the Stash Operator, it does not remove the old CRDs. Upgrade them to the latest version using the following commands:

```bash
# Update Stash Catalog CRDs
$ kubectl apply -f https://github.com/stashed/installer/raw/{{< param "info.installer" >}}/crds/stash-catalog-crds.yaml
```

#### 4. Reinstall new Stash Operator

Now, follow the latest installation guide to install the new version of the Stash operator. You can find the latest installation guide [here](/docs/v2024.9.30/setup/README).

#### 5. Update Task Name in BackupConfiguration

Stash `v0.12.x` has dropped the `v1`, `v2`, etc. suffix of `Task` name to make it easier to upgrade in future versions. In the future, when you upgrade the Stash operator, you will no longer need to update the `Task` name in the existing BackupConfigurations. However, for upgrading from `v0.11.x` to you `v0.12.x`, you have to update the `Task` name of your existing BackupConfigurations for one last time.

Please, remove the `-vX` suffix from the `spec.task.name` filed of any existing BackupConfiguration. Use the following command to edit the BackupConfiguration,

```bash
kubectl edit -n <namespace> <BackupConfiguration name>
```

For example, here is an example of `spec.task` section before and after the update.

```bash
# Before Update
  task:
    name: mysql-backup-8.0.27 # remove '-v1' part
```

```bash
# After update.
  task:
    name: mysql-backup-8.0.21
```

>If you are using KubeDB to manage your databases, you can remove the `spec.task` section entirely. KubeDB catalogs now include the respective addon information for each database version. Stash will read the addon information from there. You no longer have to worry about what addon to use with which database version.

#### 6. Resume Backup

Finally, you can resume the backups that you have paused before starting the upgrade process. You can resume backup by setting  `spec.paused` field of respective `BackupConfiguration` to `false` as below,

```bash
kubectl patch backupconfiguration -n <namespace> <BackupConfiguration name> --type="merge" --patch='{"spec": {"paused": false}}'
```

## Updating License

Stash support updating license without requiring any re-installation. Stash creates a Secret named `<helm release name>-license` with the license file. You just need to update the Secret. The changes will propagate automatically to the operator and it will use the updated license going forward.

Follow the below instructions to update the license:

- Get a new license and save it into a file.
- Then, run the following upgrade command based on your installation.

<ul class="nav nav-tabs" id="luTabs" role="tablist">
  <li class="nav-item">
    <a class="nav-link active" id="lu-helm3-tab" data-toggle="tab" href="#lu-helm3" role="tab" aria-controls="lu-helm3" aria-selected="true">Helm 3</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" id="lu-yaml-tab" data-toggle="tab" href="#lu-yaml" role="tab" aria-controls="lu-yaml" aria-selected="false">YAML</a>
  </li>
</ul>
<div class="tab-content" id="luTabContent">
  <div class="tab-pane fade show active" id="lu-helm3" role="tabpanel" aria-labelledby="lu-helm3">

#### Using Helm 3

```bash
# detect current version
helm ls -A | grep stash

# update license key keeping the current version
helm upgrade stash oci://ghcr.io/appscode-charts/stash \
  --version=<cur_version> \
  --namespace stash --create-namespace \
  --reuse-values \
  --set-file global.license=/path/to/new/license.txt \
  --wait --burst-limit=10000 --debug
```

</div>
<div class="tab-pane fade" id="lu-yaml" role="tabpanel" aria-labelledby="lu-yaml">

#### Using YAML (with helm 3)

```bash
# detect current version
helm ls -A | grep stash

# update license key keeping the current version
helm template stash oci://ghcr.io/appscode-charts/stash \
  --version=<cur_version> \
  --namespace stash --create-namespace \
  --set features.enterprise=true \
  --set global.skipCleaner=true \
  --show-only appscode/stash-enterprise/templates/license.yaml \
  --set-file global.license=/path/to/new/license.txt | kubectl apply -f -
```

</div>
</div>

## Upgrading from 0.9.x to v2020.x.x

If you are upgrading from `0.9.x` which did not use license verification to the new `v2020.x.x`, you have to first uninstall the old version. Then, you have to re-install the new version.

If you are upgrading from `0.9.x` to `v2020.x.x` Community edition, please note that the following features are only available in Enterprise edition:

- **Auto-Backup:** Auto-backup is now an enterprise feature. You won't be able to setup any new backup using auto-backup. However, your existing auto-backup resources should keep functioning.
- **Batch Backup:** Batch backup and restore is also now an enterprise feature. You won't be able to create any new backup using batch-backup. However, your existing backup should continue to work and you would be able to restore the data that were backed up using BatchBackup.
- **Local Backend:** Local backend now is an enterprise feature. If you are using any Kubernetes volume (i.e. NFS, PVC, HostPath, etc.) as backend, you won't be able to create any new backup using those backends. However, your existing backup that uses the sidecar model should keep functioning. You have to use the Enterprise edition to restore the backed-up data. If you are interested in purchasing an Enterprise license, please contact us via sales@appscode.com for further discussion. You can also set up a meeting via our [calendly link](https://calendly.com/appscode/intro).

If you are using any Stash addons, you might need to update the `Task` name in your `BackupConfiguration` to comply with the new naming scheme of the `Function` and `Task`.
