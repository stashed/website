---
title: Welcome | Stash
description: Welcome to Stash
menu:
  docs_v2020.10.21:
    identifier: readme-stash
    name: Readme
    parent: welcome
    weight: -1
product_name: stash
menu_name: docs_v2020.10.21
section_menu_id: welcome
url: /docs/v2020.10.21/welcome/
aliases:
- /docs/v2020.10.21/
- /docs/v2020.10.21/README/
info:
  catalog: v2020.10.21
  cli: v0.11.3
  community: v0.11.3
  elasticsearch:
  - 5.6.4-v3
  - 6.2.4-v3
  - 6.3.0-v3
  - 6.4.0-v3
  - 6.5.3-v3
  - 6.8.0-v3
  - 7.2.0-v3
  - 7.3.2-v3
  enterprise: v0.11.3
  installer: v0.11.3
  mongodb:
  - 3.4.17-v3
  - 3.4.22-v3
  - 3.6.13-v3
  - 3.6.8-v3
  - 4.0.11-v3
  - 4.0.3-v3
  - 4.0.5-v3
  - 4.1.13-v3
  - 4.1.4-v3
  - 4.1.7-v3
  - 4.2.3-v3
  mysql:
  - 5.7.25-v3
  - 8.0.14-v3
  - 8.0.3-v3
  percona-xtradb:
  - 5.7-v3
  postgres:
  - 10.14.0-v1
  - 11.9.0-v1
  - 12.4.0-v1
  - 9.6.19-v1
  version: v2020.10.21
---

# Stash

[Stash](https://stash.run) by AppsCode is a cloud native data backup and recovery solution for Kubernetes workloads. If you are running production workloads in Kubernetes, you might want to take backup of your disks, databases etc. Traditional tools are too complex to set up and maintain in a dynamic compute environment like Kubernetes. Stash is a Kubernetes operator that uses [restic](https://github.com/restic/restic) or Kubernetes CSI Driver VolumeSnapshotter functionality to address these issues. Using Stash, you can backup Kubernetes volumes mounted in workloads, stand-alone volumes and databases. Users may even extend Stash via [addons](https://stash.run/docs/latest/guides/latest/addons/overview/) for any custom workload.

From here you can learn all about Stash's architecture and how to deploy and use Stash.

- [Concepts](/docs/v2020.10.21/concepts/). Concepts explain some significant aspect of Stash. This is where you can learn about what Stash does and how it does it.

- [Setup](/docs/v2020.10.21/setup/). Setup contains instructions for installing
  the Stash in various cloud providers.

- [Guides](/docs/v2020.10.21/guides/latest/). Guides show you how to perform tasks with Stash.

- [Reference](/docs/v2020.10.21/reference/). Detailed exhaustive lists of
command-line options, configuration options, API definitions, and procedures.

We're always looking for help improving our documentation, so please don't hesitate to [file an issue](https://github.com/stashed/project/issues/new) if you see some problem.

---

**Stash binaries collects anonymous usage statistics to help us learn how the software is being used and how we can improve it. To disable stats collection, run the operator with the flag** `--enable-analytics=false`.

---
