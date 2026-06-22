---
title: FAQ | Stash
menu:
  docs_v2025.7.31:
    identifier: faq-readme
    name: README
    parent: faq
    weight: -1
product_name: stash
menu_name: docs_v2025.7.31
section_menu_id: faq
url: /docs/v2025.7.31/faq/
aliases:
- /docs/v2025.7.31/faq/README/
info:
  cli: v0.41.0
  community: v0.41.0
  elasticsearch:
  - 5.6.4-v37
  - 6.2.4-v37
  - 6.3.0-v37
  - 6.4.0-v37
  - 6.5.3-v37
  - 6.8.0-v37
  - 7.14.0-v23
  - 7.2.0-v37
  - 7.3.2-v37
  - 8.2.0-v20
  enterprise: v0.41.0
  etcd:
  - 3.5.0-v24
  installer: v2025.7.31
  kubedump:
  - 0.2.0-v5
  mariadb:
  - 10.5.8-v31
  mongodb:
  - 3.4.17-v38
  - 3.4.22-v38
  - 3.6.13-v38
  - 3.6.8-v38
  - 4.0.11-v38
  - 4.0.3-v38
  - 4.0.5-v38
  - 4.1.13-v38
  - 4.1.4-v38
  - 4.1.7-v38
  - 4.2.3-v38
  - 4.4.6-v29
  - 5.0.15-v11
  - 5.0.3-v26
  - 6.0.5-v14
  mysql:
  - 5.7.25-v38
  - 8.0.14-v37
  - 8.0.21-v31
  - 8.0.3-v37
  nats:
  - 2.6.1-v25
  - 2.8.2-v20
  percona-xtradb:
  - 5.7-v32
  postgres:
  - 10.14-v36
  - 11.9-v36
  - 12.4-v36
  - 13.1-v33
  - 14.0-v25
  - 15.1-v17
  - 16.1-v6
  - 17.2-v4
  - 9.6.19-v36
  redis:
  - 5.0.13-v25
  - 6.2.5-v25
  - 7.0.5-v18
  ui-server: v0.22.0
  vault:
  - 1.10.3-v17
  version: v2025.7.31
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

Please check our troubleshooting guide [here](/docs/v2025.7.31/guides/troubleshooting/how-to-troubleshoot/).

## Need More Help?

To speak with us, please leave a message on [our website](https://appscode.com/contact/). To receive product announcements, follow us on [Twitter](https://twitter.com/KubeStash).
