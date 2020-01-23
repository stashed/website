---
title: Welcome | Stash
description: Welcome to Stash
menu:
  docs_v0.9.0-rc.4:
    identifier: readme-stash
    name: Readme
    parent: welcome
    weight: -1
product_name: stash
menu_name: docs_v0.9.0-rc.4
section_menu_id: welcome
url: /docs/v0.9.0-rc.4/welcome/
aliases:
- /docs/v0.9.0-rc.4/
- /docs/v0.9.0-rc.4/README/
info:
  catalog: v0.2.0
  cli: v0.3.0
  version: v0.9.0-rc.4
---

# Stash
 Stash by AppsCode is a Kubernetes operator for [restic](https://restic.net). If you are running production workloads in Kubernetes, you might want to take backup of your disks. Using Stash, you can backup Kubernetes volumes mounted in following types of workloads:

- Deployment
- DaemonSet
- ReplicaSet
- ReplicationController
- StatefulSet

From here you can learn all about Stash's architecture and how to deploy and use Stash.

- [Concepts](/docs/v0.9.0-rc.4/concepts/). Concepts explain some significant aspect of Stash. This is where you can learn about what Stash does and how it does it.

- [Setup](/docs/v0.9.0-rc.4/setup/). Setup contains instructions for installing
  the Stash in various cloud providers.

- [Guides](/docs/v0.9.0-rc.4/guides/latest/). Guides show you how to perform tasks with Stash.

- [Reference](/docs/v0.9.0-rc.4/reference/). Detailed exhaustive lists of
command-line options, configuration options, API definitions, and procedures.

We're always looking for help improving our documentation, so please don't hesitate to [file an issue](https://github.com/stashed/stash/issues/new) if you see some problem. Or better yet, submit your own [contributions](/docs/v0.9.0-rc.4/CONTRIBUTING) to help
make our docs better.

---

**Stash binaries collects anonymous usage statistics to help us learn how the software is being used and how we can improve it. To disable stats collection, run the operator with the flag** `--enable-analytics=false`.

---
