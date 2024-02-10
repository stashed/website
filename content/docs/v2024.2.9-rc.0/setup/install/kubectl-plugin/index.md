---
title: Install Stash kubectl Plugin
description: Installation guide for Stash kubectl Plugin
menu:
  docs_v2024.2.9-rc.0:
    identifier: install-stash-kubectl-plugin
    name: Stash kubectl Plugin
    parent: installation-guide
    weight: 30
product_name: stash
menu_name: docs_v2024.2.9-rc.0
section_menu_id: setup
info:
  cli: v0.33.0-rc.0
  community: v0.33.0-rc.0
  elasticsearch:
  - 5.6.4-v29
  - 6.2.4-v29
  - 6.3.0-v29
  - 6.4.0-v29
  - 6.5.3-v29
  - 6.8.0-v29
  - 7.14.0-v15
  - 7.2.0-v29
  - 7.3.2-v29
  - 8.2.0-v12
  enterprise: v0.33.0-rc.0
  etcd:
  - 3.5.0-v16
  installer: v2024.2.9-rc.0
  kubedump:
  - 0.1.0-v12
  mariadb:
  - 10.5.8-v23
  mongodb:
  - 3.4.17-v29
  - 3.4.22-v29
  - 3.6.13-v29
  - 3.6.8-v29
  - 4.0.11-v29
  - 4.0.3-v29
  - 4.0.5-v29
  - 4.1.13-v29
  - 4.1.4-v29
  - 4.1.7-v29
  - 4.2.3-v29
  - 4.4.6-v20
  - 5.0.15-v2
  - 5.0.3-v17
  - 6.0.5-v5
  mysql:
  - 5.7.25-v29
  - 8.0.14-v29
  - 8.0.21-v23
  - 8.0.3-v29
  nats:
  - 2.6.1-v17
  - 2.8.2-v12
  percona-xtradb:
  - 5.7-v24
  postgres:
  - 10.14-v28
  - 11.9-v28
  - 12.4-v28
  - 13.1-v25
  - 14.0-v17
  - 15.1-v9
  - 9.6.19-v28
  redis:
  - 5.0.13-v17
  - 6.2.5-v17
  - 7.0.5-v10
  ui-server: v0.14.0-rc.0
  vault:
  - 1.10.3-v9
  version: v2024.2.9-rc.0
---

# Install Stash kubectl Plugin

Stash provides a `kubectl` plugin to interact with Stash resources.

## Install using Krew

Stash `kubectl` plugin can be installed using Krew. [Krew](https://krew.sigs.k8s.io/) is the plugin manager for kubectl command-line tool. To install follow the steps below:

- Install `krew` following the steps [here](https://krew.sigs.k8s.io/docs/user-guide/setup/install/).

- If you have already installed `krew`, please upgrade `krew` to version v0.4.0 or later so that you can use [custom plugin indexes](https://krew.sigs.k8s.io/docs/user-guide/custom-indexes/).

```bash
kubectl krew upgrade
kubectl krew version
```

- Add [AppsCode's kubectl plugin index](https://github.com/appscode/krew-index). If you have already added the index, update the index.

```bash
kubectl krew index add appscode https://github.com/appscode/krew-index.git
kubectl krew index list
kubectl krew update
```

- Install Stash `kubectl` plugin following the commands below:

```bash
kubectl krew install appscode/stash
kubectl stash version
```

- If Stash `kubectl` plugin is already installed, run the following command to upgrade the plugin:

```bash
kubectl krew upgrade
kubectl stash version
```

## Install using pre-built binary

You can download the pre-build binaries from [stashed/cli](https://github.com/stashed/cli/releases) releases and put it into one of your installation directory denoted by `$PATH` variable.

Here is a simple Linux command to install the latest 64-bit Linux binary directly into your `/usr/local/bin` directory:

```bash
# Linux amd 64-bit
curl -o kubectl-stash.tar.gz -fsSL https://github.com/stashed/cli/releases/download/{{< param "info.cli" >}}/kubectl-stash-linux-amd64.tar.gz \
  && tar zxvf kubectl-stash.tar.gz \
  && chmod +x kubectl-stash-linux-amd64 \
  && sudo mv kubectl-stash-linux-amd64 /usr/local/bin/kubectl-stash \
  && rm kubectl-stash.tar.gz LICENSE.md

# Mac OSX 64-bit
curl -o kubectl-stash.tar.gz -fsSL https://github.com/stashed/cli/releases/download/{{< param "info.cli" >}}/kubectl-stash-darwin-amd64.tar.gz \
  && tar zxvf kubectl-stash.tar.gz \
  && chmod +x kubectl-stash-darwin-amd64 \
  && sudo mv kubectl-stash-darwin-amd64 /usr/local/bin/kubectl-stash \
  && rm kubectl-stash.tar.gz LICENSE.md
```

If you prefer to install kubectl Stash cli from source code, make sure that your go development environment has been setup properly. Then, just run:

```bash
go get github.com/stashed/cli/...
```

>Please note that this will install Stash cli from master branch which might include breaking and/or undocumented changes.
