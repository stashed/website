---
title: Install Stash kubectl Plugin
description: Installation guide for Stash kubectl Plugin
menu:
  docs_v2023.08.14:
    identifier: install-stash-kubectl-plugin
    name: Stash kubectl Plugin
    parent: installation-guide
    weight: 30
product_name: stash
menu_name: docs_v2023.08.14
section_menu_id: setup
info:
  cli: v0.31.0
  community: v0.31.0
  elasticsearch:
  - 5.6.4-v27
  - 6.2.4-v27
  - 6.3.0-v27
  - 6.4.0-v27
  - 6.5.3-v27
  - 6.8.0-v27
  - 7.14.0-v13
  - 7.2.0-v27
  - 7.3.2-v27
  - 8.2.0-v10
  enterprise: v0.31.0
  etcd:
  - 3.5.0-v14
  installer: v2023.08.14
  kubedump:
  - 0.1.0-v10
  mariadb:
  - 10.5.8-v20
  mongodb:
  - 3.4.17-v27
  - 3.4.22-v27
  - 3.6.13-v27
  - 3.6.8-v27
  - 4.0.11-v27
  - 4.0.3-v27
  - 4.0.5-v27
  - 4.1.13-v27
  - 4.1.4-v27
  - 4.1.7-v27
  - 4.2.3-v27
  - 4.4.6-v18
  - 5.0.15
  - 5.0.3-v15
  - 6.0.5-v3
  mysql:
  - 5.7.25-v27
  - 8.0.14-v27
  - 8.0.21-v21
  - 8.0.3-v27
  nats:
  - 2.6.1-v15
  - 2.8.2-v10
  percona-xtradb:
  - 5.7-v22
  postgres:
  - 10.14-v26
  - 11.9-v26
  - 12.4-v26
  - 13.1-v23
  - 14.0-v15
  - 15.1-v7
  - 9.6.19-v26
  redis:
  - 5.0.13-v15
  - 6.2.5-v15
  - 7.0.5-v8
  ui-server: v0.13.0
  vault:
  - 1.10.3-v7
  version: v2023.08.14
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
