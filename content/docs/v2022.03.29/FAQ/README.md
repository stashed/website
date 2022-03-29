---
title: FAQ | Stash
menu:
  docs_v2022.03.29:
    identifier: faq-readme
    name: README
    parent: faq
    weight: -1
product_name: stash
menu_name: docs_v2022.03.29
section_menu_id: faq
url: /docs/v2022.03.29/faq/
aliases:
- /docs/v2022.03.29/faq/README/
info:
  cli: v0.19.0
  community: v0.19.0
  elasticsearch:
  - 5.6.4-v16
  - 6.2.4-v16
  - 6.3.0-v16
  - 6.4.0-v16
  - 6.5.3-v16
  - 6.8.0-v16
  - 7.14.0-v2
  - 7.2.0-v16
  - 7.3.2-v16
  enterprise: v0.19.0
  etcd:
  - 3.5.0-v3
  installer: v2022.03.29
  mariadb:
  - 10.5.8-v9
  mongodb:
  - 3.4.17-v15
  - 3.4.22-v15
  - 3.6.13-v15
  - 3.6.8-v15
  - 4.0.11-v15
  - 4.0.3-v15
  - 4.0.5-v15
  - 4.1.13-v15
  - 4.1.4-v15
  - 4.1.7-v15
  - 4.2.3-v15
  - 4.4.6-v6
  - 5.0.3-v3
  mysql:
  - 5.7.25-v16
  - 8.0.14-v16
  - 8.0.21-v10
  - 8.0.3-v16
  nats:
  - 2.6.1-v3
  percona-xtradb:
  - 5.7-v11
  postgres:
  - 10.14-v14
  - 11.9-v14
  - 12.4-v14
  - 13.1-v11
  - 14.0-v3
  - 9.6.19-v14
  redis:
  - 5.0.13-v4
  - 6.2.5-v4
  ui-server: v0.2.0
  version: v2022.03.29
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

## Need More Help?

To speak with us, please leave a message on [our website](https://appscode.com/contact/).

To join public discussions with the Stash community, join us in the [AppsCode Slack team](https://appscode.slack.com/messages/C8NCX6N23/details/) channel `#stash`. To sign up, use our [Slack inviter](https://slack.appscode.com/).
