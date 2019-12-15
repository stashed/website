---
title: Concepts | Stash
menu:
  docs_0.7.0-rc.4:
    identifier: concepts-readme
    name: Readme
    parent: concepts
    weight: -1
product_name: stash
menu_name: docs_0.7.0-rc.4
section_menu_id: concepts
url: /docs/0.7.0-rc.4/concepts/
aliases:
- /docs/0.7.0-rc.4/concepts/README/
info:
  version: 0.7.0-rc.4
---

# Concepts

Concepts help you learn about the different parts of the Stash and the abstractions it uses.

- What is Stash?
  - [Overview](/docs/0.7.0-rc.4/concepts/what-is-stash/overview). Provides a conceptual introduction to Stash, including the problems it solves and its high-level architecture.
- Custom Resource Definitions
  - [Restic](/docs/0.7.0-rc.4/concepts/crds/restic). Introduces the concept of `Restic` for configuring [restic](https://restic.net) in a Kubernetes native way.
  - [Recovery](/docs/0.7.0-rc.4/concepts/crds/recovery). Introduces the concept of `Recovery` to restore a backup taken using Stash.
  - [Repository](/docs/0.7.0-rc.4/concepts/crds/repository) Introduce concept of `Repository` that represents restic repository in a Kubernetes native way.
  - [Snapshot](/docs/0.7.0-rc.4/concepts/crds/snapshot) Introduce concept of `Snapshot` that represents backed up snapshots in a Kubernetes native way.