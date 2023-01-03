---
title: FAQ | Stash
menu:
  docs_v2023.01.05:
    identifier: faq-readme
    name: README
    parent: faq
    weight: -1
product_name: stash
menu_name: docs_v2023.01.05
section_menu_id: faq
url: /docs/v2023.01.05/faq/
aliases:
- /docs/v2023.01.05/faq/README/
info:
  cli: v0.25.0
  community: v0.25.0
  elasticsearch:
  - 5.6.4-v21
  - 6.2.4-v21
  - 6.3.0-v21
  - 6.4.0-v21
  - 6.5.3-v21
  - 6.8.0-v21
  - 7.14.0-v7
  - 7.2.0-v21
  - 7.3.2-v21
  - 8.2.0-v4
  enterprise: v0.25.0
  etcd:
  - 3.5.0-v8
  installer: v2023.01.05
  kubedump:
  - 0.1.0-v4
  mariadb:
  - 10.5.8-v14
  mongodb:
  - 3.4.17-v21
  - 3.4.22-v21
  - 3.6.13-v21
  - 3.6.8-v21
  - 4.0.11-v21
  - 4.0.3-v21
  - 4.0.5-v21
  - 4.1.13-v21
  - 4.1.4-v21
  - 4.1.7-v21
  - 4.2.3-v21
  - 4.4.6-v12
  - 5.0.3-v9
  mysql:
  - 5.7.25-v21
  - 8.0.14-v21
  - 8.0.21-v15
  - 8.0.3-v21
  nats:
  - 2.6.1-v9
  - 2.8.2-v4
  percona-xtradb:
  - 5.7-v16
  postgres:
  - 10.14-v20
  - 11.9-v20
  - 12.4-v20
  - 13.1-v17
  - 14.0-v9
  - 15.1-v1
  - 9.6.19-v20
  redis:
  - 5.0.13-v9
  - 6.2.5-v9
  - 7.0.5-v2
  ui-server: v0.7.0
  vault:
  - 1.10.3-v1
  version: v2023.01.05
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

Please check our troubleshooting guide [here](/docs/v2023.01.05/guides/troubleshooting/how-to-troubleshoot/).

## Need More Help?

To speak with us, please leave a message on [our website](https://appscode.com/contact/). To receive product announcements, follow us on [Twitter](https://twitter.com/KubeStash).
