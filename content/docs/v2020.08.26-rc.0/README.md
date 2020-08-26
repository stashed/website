---
title: Welcome | Stash
description: Welcome to Stash
menu:
  docs_v2020.08.26-rc.0:
    identifier: readme-stash
    name: Readme
    parent: welcome
    weight: -1
product_name: stash
menu_name: docs_v2020.08.26-rc.0
section_menu_id: welcome
url: /docs/v2020.08.26-rc.0/welcome/
aliases:
- /docs/v2020.08.26-rc.0/
- /docs/v2020.08.26-rc.0/README/
info:
  catalog: v2020.08.26-rc.0
  cli: v0.10.0-rc.0
  version: v2020.08.26-rc.0
---

# Stash

[Stash](https://stash.run) by AppsCode is a cloud native data backup and recovery solution for Kubernetes workloads. If you are running production workloads in Kubernetes, you might want to take backup of your disks, databases etc. Traditional tools are too complex to setup and maintain in a dynamic compute environment like Kubernetes. Stash is a Kubernetes operator that uses [restic](https://github.com/restic/restic) or Kubernetes CSI Driver VolumeSnapshotter functionality to address these issues. Using Stash, you can backup Kubernetes volumes mounted in workloads, stand-alone volumes and databases. User may even extend Stash via [addons](https://stash.run/docs/latest/guides/latest/addons/overview/) for any custom workload.

From here you can learn all about Stash's architecture and how to deploy and use Stash.

- [Concepts](/docs/v2020.08.26-rc.0/concepts/). Concepts explain some significant aspect of Stash. This is where you can learn about what Stash does and how it does it.

- [Setup](/docs/v2020.08.26-rc.0/setup/). Setup contains instructions for installing
  the Stash in various cloud providers.

- [Guides](/docs/v2020.08.26-rc.0/guides/latest/). Guides show you how to perform tasks with Stash.

- [Reference](/docs/v2020.08.26-rc.0/reference/). Detailed exhaustive lists of
command-line options, configuration options, API definitions, and procedures.

We're always looking for help improving our documentation, so please don't hesitate to [file an issue](https://github.com/stashed/project/issues/new) if you see some problem.

---

**Stash binaries collects anonymous usage statistics to help us learn how the software is being used and how we can improve it. To disable stats collection, run the operator with the flag** `--enable-analytics=false`.

---
