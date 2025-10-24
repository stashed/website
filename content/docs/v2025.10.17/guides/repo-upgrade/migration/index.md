---
title: Upgrading Repository Format Version | Stash
menu:
  docs_v2025.10.17:
    identifier: how-to-upgrade
    name: How to upgrade?
    parent: repo-upgrade
    weight: 10
product_name: stash
menu_name: docs_v2025.10.17
section_menu_id: guides
info:
  cli: v0.42.0
  community: v0.42.0
  elasticsearch:
  - 5.6.4-v38
  - 6.2.4-v38
  - 6.3.0-v38
  - 6.4.0-v38
  - 6.5.3-v38
  - 6.8.0-v38
  - 7.14.0-v24
  - 7.2.0-v38
  - 7.3.2-v38
  - 8.2.0-v21
  enterprise: v0.42.0
  etcd:
  - 3.5.0-v25
  installer: v2025.10.17
  kubedump:
  - 0.2.0-v6
  mariadb:
  - 10.6.23-v1
  mongodb:
  - 3.4.17-v39
  - 3.4.22-v39
  - 3.6.13-v39
  - 3.6.8-v39
  - 4.0.11-v39
  - 4.0.3-v39
  - 4.0.5-v39
  - 4.1.13-v39
  - 4.1.4-v39
  - 4.1.7-v39
  - 4.2.3-v39
  - 4.4.6-v30
  - 5.0.15-v12
  - 5.0.3-v27
  - 6.0.5-v15
  mysql:
  - 5.7.25-v39
  - 8.0.14-v38
  - 8.0.21-v32
  - 8.0.3-v38
  nats:
  - 2.6.1-v26
  - 2.8.2-v21
  percona-xtradb:
  - 5.7-v33
  postgres:
  - 10.14-v37
  - 11.9-v37
  - 12.4-v37
  - 13.1-v34
  - 14.0-v26
  - 15.1-v18
  - 16.1-v7
  - 17.2-v5
  - 9.6.19-v37
  redis:
  - 5.0.13-v26
  - 6.2.5-v26
  - 7.0.5-v19
  ui-server: v0.23.0
  vault:
  - 1.10.3-v18
  version: v2025.10.17
---

# Upgrading Repository Format Version

Repositories created with older versions of `Stash` use an older repository format version, which needs to be upgraded to unlock new features. This upgrade process is optional but required if you want to take advantage of the latest enhancements. Keep in mind that upgrading the repository format will increase the up-to-date Stash version needed to access it. For example, repositories upgraded to format version 2 can only be read by `Stash v2025.1.9` or later.

> Upgrading repository format version will take some time depending on the repository size. Repository issues must be corrected before upgrading. It is recommended to contact with Stash team before upgrading repository.

## How to upgrade

Upgrading to repository version 2 involves the following steps:

1. **Pause the Corresponding Backup:** Before upgrading, pause the backup associated with the repository. You can do this using the [pause backup](/docs/v2025.10.17/guides/cli/kubectl-plugin/#pause-backup) command provided by the Stash kubectl plugin.
2. **Run the Migration Command:** Next, execute the [migrate](/docs/v2025.10.17/guides/cli/kubectl-plugin/#migrate-repository) command provided by the Stash kubectl plugin. This command will first check the repository’s integrity and then upgrade its format to version 2. Note that if any issues are found during the integrity check, they must be resolved before the migration can proceed.
3. **Run the Prune Command:** After a successful migration, use the [prune](/docs/v2025.10.17/guides/cli/kubectl-plugin/#prune) command provided by Stash kubectl plugin to compress the repository metadata. If you want to limit the amount of data rewritten in a single operation, use the `--max-repack-size` flag with the `prune` command.
4. **Resume the Corresponding Backup:** Now resume the backup associated with the repository. You can do this using the [resume backup](/docs/v2025.10.17/guides/cli/kubectl-plugin/#resume-backup) command provided by the Stash kubectl plugin.

Keep in mind that the contents of files already stored in the repository will not be rewritten during the upgrade. Only data from new backups will be compressed. Over time, more and more of the repository will be automatically compressed as new backups are added.
