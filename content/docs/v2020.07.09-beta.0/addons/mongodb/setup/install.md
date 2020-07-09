---
title: Install MongoDB Addon | Stash
description: An guide on how to install MongoDB addon for Stash
menu:
  docs_v2020.07.09-beta.0:
    identifier: stash-mongodb-install
    name: Install
    parent: stash-mongodb-setup
    weight: 10
product_name: stash
menu_name: docs_v2020.07.09-beta.0
section_menu_id: stash-addons
info:
  catalog: v2020.07.09-beta.0
  cli: v0.10.0-beta.1
  version: v2020.07.09-beta.0
---

# Install MongoDB Addon for Stash

Stash uses `Function-Task` model to backup databases. This `Function-Task` model enables Stash to extend its capability via addons. In order to backup MongoDB databases, you have to install MongoDB addon (`stash-mongodb`) for Stash. This addon creates necessary `Function` and `Task` definitions to backup/restore MongoDB database.

You can install the addon either as a helm chart or you can create only the YAMLs of the respective resources.

<ul class="nav nav-tabs" id="installerTab" role="tablist">
  <li class="nav-item">
    <a class="nav-link active" id="helm3-tab" data-toggle="tab" href="#helm3" role="tab" aria-controls="helm3" aria-selected="true">Helm 3 (Recommended)</a>
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

Run the following script to install `stash-mongodb` addon as a Helm chart using Helm 3.

```console
curl -fsSL https://github.com/stashed/catalog/raw/{{< param "info.catalog" >}}/deploy/helm3.sh | bash -s -- --catalog=stash-mongodb
```

</div>
<div class="tab-pane fade" id="helm2" role="tabpanel" aria-labelledby="helm2-tab">

## Using Helm 2

Run the following script to install `stash-mongodb` addon as a Helm chart using Helm 2.

```console
curl -fsSL https://github.com/stashed/catalog/raw/{{< param "info.catalog" >}}/deploy/helm2.sh | bash -s -- --catalog=stash-mongodb
```

</div>
<div class="tab-pane fade" id="script" role="tabpanel" aria-labelledby="script-tab">

## Using YAML

Run the following script to install `stash-mongodb` addon as Kubernetes YAMLs.

```console
curl -fsSL https://github.com/stashed/catalog/raw/{{< param "info.catalog" >}}/deploy/script.sh | bash -s -- --catalog=stash-mongodb
```

>The above script uses Helm 3 for rendering the charts to generate the YAMLs.

</div>
</div>

## Verify Installation

After installation is completed, this addon will create `mongodb-backup-*` and `mongodb-restore-*` Functions and Tasks for all supported MongoDB versions. To verify, run the following command:

```console
$ kubectl get functions.stash.appscode.com
NAME                    AGE
mongodb-backup-4.1      20s
mongodb-backup-4.0      20s
mongodb-backup-3.6      19s
mongodb-backup-3.4      20s
mongodb-restore-4.1     20s
mongodb-restore-4.0     20s
mongodb-restore-3.6     19s
mongodb-restore-3.4     20s
pvc-backup              7h6m
pvc-restore             7h6m
update-status           7h6m
```

Also, verify that the `Task` have been created.

```console
$ kubectl get tasks.stash.appscode.com
NAME                    AGE
mongodb-backup-4.1      2m7s
mongodb-backup-4.0      2m7s
mongodb-backup-3.6      2m6s
mongodb-backup-3.4      2m7s
mongodb-restore-4.1     2m7s
mongodb-restore-4.0     2m7s
mongodb-restore-3.6     2m6s
mongodb-restore-3.4     2m7s
pvc-backup              7h7m
pvc-restore             7h7m
```

Now, Stash is ready to backup MongoDB databases.

## Customizing Installation

In order to install `Function` and `Task` only for a specific MongoDB version, use `--version` flag to specify the desired database version.

```console
curl -fsSL https://github.com/stashed/catalog/raw/{{< param "info.catalog" >}}/deploy/helm3.sh | bash -s -- --catalog=stash-mongodb --version=3.6
```

The flowing flags are available for customizing MongoDB addon installation:

| Flag                | Usage                                                                                                                                                                                                                                                                                           |
| ------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `--version`         | Specify a specific version of a specific addon to install. Use it along with `--catalog` flag.                                                                                                                                                                                                  |
| `--docker-registry` | Specify the docker registry to use to pull respective addon images. Default Value: `stashed`.                                                                                                                                                                                                   |
| `--image`           | Specify the name of the docker image to use for respective addons.                                                                                                                                                                                                                              |
| `--image-tag`       | Specify the tag of the docker image to use for respective addon.                                                                                                                                                                                                                                |
| `--mg-backup-args`  | Specify optional arguments to pass to `mongodump` command during backup. These arguments apply to all MongoDB instances in this cluster. To set arguments for a specific MongoDB database instance, set `mgArgs` parameter in `spec.task.params` field of the respective `BackupConfiguration`. |
| `--mg-restore-args` | Specify optional arguments to pass to `mongorestore` command during restore. These arguments apply to all MongoDB instances in this cluster. To set arguments for a specific MongoDB database instance, set `mgArgs` parameter in `spec.task.params` field of the respective `RestoreSession`.  |