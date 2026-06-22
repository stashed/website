---
title: Ciphertext Verification Failed| Stash
description: Troubleshooting "ciphertext verification failed" issue
menu:
  docs_v2025.2.10:
    identifier: troubleshooting-ciphertext-verification-failed
    name: Ciphertext verification failed
    parent: troubleshooting
    weight: 40
product_name: stash
menu_name: docs_v2025.2.10
section_menu_id: guides
info:
  cli: v0.39.0
  community: v0.39.0
  elasticsearch:
  - 5.6.4-v35
  - 6.2.4-v35
  - 6.3.0-v35
  - 6.4.0-v35
  - 6.5.3-v35
  - 6.8.0-v35
  - 7.14.0-v21
  - 7.2.0-v35
  - 7.3.2-v35
  - 8.2.0-v18
  enterprise: v0.39.0
  etcd:
  - 3.5.0-v22
  installer: v2025.2.10
  kubedump:
  - 0.2.0-v3
  mariadb:
  - 10.5.8-v29
  mongodb:
  - 3.4.17-v36
  - 3.4.22-v36
  - 3.6.13-v36
  - 3.6.8-v36
  - 4.0.11-v36
  - 4.0.3-v36
  - 4.0.5-v36
  - 4.1.13-v36
  - 4.1.4-v36
  - 4.1.7-v36
  - 4.2.3-v36
  - 4.4.6-v27
  - 5.0.15-v9
  - 5.0.3-v24
  - 6.0.5-v12
  mysql:
  - 5.7.25-v36
  - 8.0.14-v35
  - 8.0.21-v29
  - 8.0.3-v35
  nats:
  - 2.6.1-v23
  - 2.8.2-v18
  percona-xtradb:
  - 5.7-v30
  postgres:
  - 10.14-v34
  - 11.9-v34
  - 12.4-v34
  - 13.1-v31
  - 14.0-v23
  - 15.1-v15
  - 16.1-v4
  - 17.2-v2
  - 9.6.19-v34
  redis:
  - 5.0.13-v23
  - 6.2.5-v23
  - 7.0.5-v16
  ui-server: v0.20.0
  vault:
  - 1.10.3-v15
  version: v2025.2.10
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
