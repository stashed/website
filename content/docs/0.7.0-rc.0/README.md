---
title: Weclome | Stash
description: Welcome to Stash
menu:
  docs_0.7.0-rc.0:
    identifier: readme-stash
    name: Readme
    parent: welcome
    weight: -1
product_name: stash
menu_name: docs_0.7.0-rc.0
section_menu_id: welcome
url: /docs/0.7.0-rc.0/welcome/
aliases:
- /docs/0.7.0-rc.0/
- /docs/0.7.0-rc.0/README/
info:
  version: 0.7.0-rc.0
---

# Stash
 Stash by AppsCode is a Kubernetes operator for [restic](https://restic.net). If you are running production workloads in Kubernetes, you might want to take backup of your disks. Using Stash, you can backup Kubernetes volumes mounted in following types of workloads:

- Deployment
- DaemonSet
- ReplicaSet
- ReplicationController
- StatefulSet

From here you can learn all about Stash's architecture and how to deploy and use Stash.

- [Concepts](/docs/0.7.0-rc.0/concepts/). Concepts explain some significant aspect of Stash. This is where you can learn about what Stash does and how it does it.

- [Setup](/docs/0.7.0-rc.0/setup/). Setup contains instructions for installing
  the Stash in various cloud providers.

- [Guides](/docs/0.7.0-rc.0/guides/). Guides show you how to perform tasks with Stash.

- [Reference](/docs/0.7.0-rc.0/reference/). Detailed exhaustive lists of
command-line options, configuration options, API definitions, and procedures.

We're always looking for help improving our documentation, so please don't hesitate to [file an issue](https://github.com/appscode/stash/issues/new) if you see some problem. Or better yet, submit your own [contributions](/docs/0.7.0-rc.0/CONTRIBUTING) to help
make our docs better.

---

**Stash binaries collects anonymous usage statistics to help us learn how the software is being used and how we can improve it. To disable stats collection, run the operator with the flag** `--analytics=false`.

---
