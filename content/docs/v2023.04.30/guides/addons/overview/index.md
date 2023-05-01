---
title: Addons | Stash
menu:
  docs_v2023.04.30:
    identifier: addons-overview
    name: Overview
    parent: addons
    weight: 10
product_name: stash
menu_name: docs_v2023.04.30
section_menu_id: guides
info:
  cli: v0.29.0
  community: v0.29.0
  elasticsearch:
  - 5.6.4-v25
  - 6.2.4-v25
  - 6.3.0-v25
  - 6.4.0-v25
  - 6.5.3-v25
  - 6.8.0-v25
  - 7.14.0-v11
  - 7.2.0-v25
  - 7.3.2-v25
  - 8.2.0-v8
  enterprise: v0.29.0
  etcd:
  - 3.5.0-v12
  installer: v2023.04.30
  kubedump:
  - 0.1.0-v8
  mariadb:
  - 10.5.8-v18
  mongodb:
  - 3.4.17-v25
  - 3.4.22-v25
  - 3.6.13-v25
  - 3.6.8-v25
  - 4.0.11-v25
  - 4.0.3-v25
  - 4.0.5-v25
  - 4.1.13-v25
  - 4.1.4-v25
  - 4.1.7-v25
  - 4.2.3-v25
  - 4.4.6-v16
  - 5.0.3-v13
  - 6.0.5-v1
  mysql:
  - 5.7.25-v25
  - 8.0.14-v25
  - 8.0.21-v19
  - 8.0.3-v25
  nats:
  - 2.6.1-v13
  - 2.8.2-v8
  percona-xtradb:
  - 5.7-v20
  postgres:
  - 10.14-v24
  - 11.9-v24
  - 12.4-v24
  - 13.1-v21
  - 14.0-v13
  - 15.1-v5
  - 9.6.19-v24
  redis:
  - 5.0.13-v13
  - 6.2.5-v13
  - 7.0.5-v6
  ui-server: v0.11.0
  vault:
  - 1.10.3-v5
  version: v2023.04.30
---

# Stash Addons

Stash 0.9.0+ supports extending its functionality through addons. This guide will give you an overview of what is an addon, how addons work and a list of Stash addons.

## What is an Addon

Stash 0.9.0+ uses two different models for backup/restore based on target types. One of them is **sidecar model** where Stash injects a `sidecar`/`init-container` into the targeted workload for backup/restore. Another is **job model** where backup or restore is done via an external job.

The job model is further divided into two categories. In the first category, the targeted resource is well known to Stash (example, a PVC). Hence, the Stash operator itself can create the required job to backup/restore the target. In the second category, Stash follows `Function-Task` model where the targeted resource is not known to Stash. In this case, the user creates some [Function](/docs/v2023.04.30/concepts/crds/function/) which resembles a step of backup/restore process and a [Task](/docs/v2023.04.30/concepts/crds/task/) which specifies the order of execution of these steps. Stash uses these `Function` and `Task` to generate the required job definition to backup/restore the target.

The `Function-Task` model enables Stash to backup/restore the resources that the operator itself is not aware of. Users can extend Stash by creating respective `Function`, `Task`, and docker images for respective `Function` to backup their desired resources.

A Stash addon is a collection of [Functions](/docs/v2023.04.30/concepts/crds/function/) and a [Task](/docs/v2023.04.30/concepts/crds/task/) to backup & restore a specific resource.

## How Addons Work

When a user installs a Stash addon, it creates some `Function` and `Task` definitions. Then, when he creates a `BackupConfiguration` or `RestoreSession` object to backup/restore his desired resource, Stash operator resolves the `Function` and `Task` to create a Job to backup/restore the target. The following diagram shows how addons works in Stash:

<figure align="center">
  <img alt="Stash Addon Overview" src="images/addon_overview.svg">
  <figcaption align="center">Fig: Stash Addon Overview</figcaption>
</figure>

A `Function` is fundamentally a container specification and `Task` specifies the execution order of the containers. Stash operator injects all the containers except last one resolved from the `Function` specified in `Task` as `init-container` into the job in the same order they appear in the `Task`. Then, it injects the last container as the main container of the Job.

## Available Addons

The following addons are available for [Stash Enterprise Edition](/docs/v2023.04.30/setup/install/enterprise/):

{{< catalogtable "elasticsearch" "mariadb" "mongodb" "mysql" "percona-xtradb" "postgres" >}}
