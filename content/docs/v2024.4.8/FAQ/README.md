---
title: FAQ | Stash
menu:
  docs_v2024.4.8:
    identifier: faq-readme
    name: README
    parent: faq
    weight: -1
product_name: stash
menu_name: docs_v2024.4.8
section_menu_id: faq
url: /docs/v2024.4.8/faq/
aliases:
- /docs/v2024.4.8/faq/README/
info:
  cli: v0.34.0
  community: v0.34.0
  elasticsearch:
  - 5.6.4-v31
  - 6.2.4-v31
  - 6.3.0-v31
  - 6.4.0-v31
  - 6.5.3-v31
  - 6.8.0-v31
  - 7.14.0-v17
  - 7.2.0-v31
  - 7.3.2-v31
  - 8.2.0-v14
  enterprise: v0.34.0
  etcd:
  - 3.5.0-v18
  installer: v2024.4.8
  kubedump:
  - 0.1.0-v14
  mariadb:
  - 10.5.8-v25
  mongodb:
  - 3.4.17-v31
  - 3.4.22-v31
  - 3.6.13-v31
  - 3.6.8-v31
  - 4.0.11-v31
  - 4.0.3-v31
  - 4.0.5-v31
  - 4.1.13-v31
  - 4.1.4-v31
  - 4.1.7-v31
  - 4.2.3-v31
  - 4.4.6-v22
  - 5.0.15-v4
  - 5.0.3-v19
  - 6.0.5-v7
  mysql:
  - 5.7.25-v31
  - 8.0.14-v31
  - 8.0.21-v25
  - 8.0.3-v31
  nats:
  - 2.6.1-v19
  - 2.8.2-v14
  percona-xtradb:
  - 5.7-v26
  postgres:
  - 10.14-v30
  - 11.9-v30
  - 12.4-v30
  - 13.1-v27
  - 14.0-v19
  - 15.1-v11
  - "16.1"
  - 9.6.19-v30
  redis:
  - 5.0.13-v19
  - 6.2.5-v19
  - 7.0.5-v12
  ui-server: v0.15.0
  vault:
  - 1.10.3-v11
  version: v2024.4.8
---

# Frequently Asked Questions

## How to temporarily pause a backup?

### Pause Backup

Run the following commands to pasue a backup temporarily,

```bash
# pause backup by patching BackupConfiguration
❯ kubectl patch backupconfiguration -n <namespace> <backupconfiguration name> --type="merge" --patch='{"spec": {"paused": true}}'

# pause backup using Stash `kubectl` plugin 
❯ kubectl stash pause backup -n demo --backupconfig=<backupconfiguration name>
```

### Resume Backup

Similarly you can also resume a paused backup. Run the following commands to resume a backup,

```bash
# resume backup by patching BackupConfiguration
kubectl patch backupconfiguration -n <namespace> <backupconfiguration name> --type="merge" --patch='{"spec": {"paused": false}}'

# resume backup using Stash `kubectl` plugin
❯ kubectl stash resume backup -n demo --backupconfig=<backupconfiguration name>
```

## When `retentionPolicy` is applied?

`retentionPolicy` specifies the policy to follow for cleaning old snapshots. Stash removes any snapshot from backend that falls outside the scope of the policy. When a `BackupSession` is completed, Stash checks for outdated snapshots according to the `retentionPolicy` and remove them. If you use the policy `keep-last-5`, Stash will remove any snapshot that is older than the most recent 5 snapshots. 

## Do I need to delete the init containers after recovery?

You don't need to delete the init containers after recovery.  If your workload restarts with the `stash-init` init-container for any reason, the init-container will skip running restore process if there is no pending RestoreSession for this workload. If you delete the RestoreSession, Stash will remove the `init-container` automatically. Beware that it will cause your workload to restart.

## I am experiencing problem X. How do I fix this?

Please check our troubleshooting guide [here](/docs/v2024.4.8/guides/troubleshooting/how-to-troubleshoot/).

## Need More Help?

To speak with us, please leave a message on [our website](https://appscode.com/contact/). To receive product announcements, follow us on [Twitter](https://twitter.com/KubeStash).
