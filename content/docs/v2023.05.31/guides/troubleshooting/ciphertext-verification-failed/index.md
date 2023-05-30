---
title: Ciphertext Verification Failed| Stash
description: Troubleshooting "ciphertext verification failed" issue
menu:
  docs_v2023.05.31:
    identifier: troubleshooting-ciphertext-verification-failed
    name: Ciphertext verification failed
    parent: troubleshooting
    weight: 40
product_name: stash
menu_name: docs_v2023.05.31
section_menu_id: guides
info:
  cli: v0.30.0
  community: v0.30.0
  elasticsearch:
  - 5.6.4-v26
  - 6.2.4-v26
  - 6.3.0-v26
  - 6.4.0-v26
  - 6.5.3-v26
  - 6.8.0-v26
  - 7.14.0-v12
  - 7.2.0-v26
  - 7.3.2-v26
  - 8.2.0-v9
  enterprise: v0.30.0
  etcd:
  - 3.5.0-v13
  installer: v2023.05.31
  kubedump:
  - 0.1.0-v9
  mariadb:
  - 10.5.8-v19
  mongodb:
  - 3.4.17-v26
  - 3.4.22-v26
  - 3.6.13-v26
  - 3.6.8-v26
  - 4.0.11-v26
  - 4.0.3-v26
  - 4.0.5-v26
  - 4.1.13-v26
  - 4.1.4-v26
  - 4.1.7-v26
  - 4.2.3-v26
  - 4.4.6-v17
  - 5.0.3-v14
  - 6.0.5-v2
  mysql:
  - 5.7.25-v26
  - 8.0.14-v26
  - 8.0.21-v20
  - 8.0.3-v26
  nats:
  - 2.6.1-v14
  - 2.8.2-v9
  percona-xtradb:
  - 5.7-v21
  postgres:
  - 10.14-v25
  - 11.9-v25
  - 12.4-v25
  - 13.1-v22
  - 14.0-v14
  - 15.1-v6
  - 9.6.19-v25
  redis:
  - 5.0.13-v14
  - 6.2.5-v14
  - 7.0.5-v7
  ui-server: v0.12.0
  vault:
  - 1.10.3-v6
  version: v2023.05.31
---

# Troubleshooting "ciphertext verification failed" issue

Sometimes, the backup starts failing after a few days with an error indicating `ciphertext verification failed`. In this guide, we are going to explain what might cause the issue and how to solve it.

## Identifying the issue

The backup should run successfully for a few days. Suddenly, it will start failing. Any subsequent backups will fail too. If you describe the respective `BackupSession` or view the log from the respective backup sidecar/job, you should see the following error:

```bash
Fatal: ciphertext verification failed
```

## Possible reasons

This can happen if the backed-up data get corrupted for any of these reasons.

- Someone deleted some files/folders from the backend manually.
- The respective bucket has a policy configured to delete the old data automatically.

## Solution

At first, check if the bucket has any policy configured to delete the old data automatically. If this is the case, please remove that policy and depend only on the retention policy provided by Stash Repository to cleanup the old data.

For example, if you are using a GCS bucket, you can check for such policy in the `LIFECYCLE` tab.

<figure align="center">
  <img alt="Object deletion policy in GCS" src="images/gcs_lifecycle_policy.png">
<figcaption align="center">Fig: Object deletion policy in GCS</figcaption>
</figure>

Unfortunately, there is no known way to fix the corrupted repository. You have to delete all the corrupted data from the backend. Only then, the subsequent backups will succeed again.
