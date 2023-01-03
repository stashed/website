---
title: Create Backupconfig
menu:
  docs_v2023.01.05:
    identifier: kubectl-stash-create-backupconfig
    name: Create Backupconfig
    parent: reference-cli
menu_name: docs_v2023.01.05
section_menu_id: reference
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

## kubectl-stash create backupconfig

Create a new BackupConfiguration

### Synopsis

Create a new BackupConfiguration to backup a target

```
kubectl-stash create backupconfig [flags]
```

### Examples

```
  # Create a new BackupConfiguration
  # stash create backupconfig --namespace=<namespace> gcs-repo [Flag]
  # For Restic driver
  stash create backupconfig ss-backup --namespace=demo --repo-name=gcs-repo --schedule="*/4 * * * *" --target-apiversion=apps/v1 --target-kind=StatefulSet --target-name=stash-demo --paths=/source/data --volume-mounts=source-data:/source/data --keep-last=5 --prune=true
  # For VolumeSnapshotter driver
  stash create backupconfig statefulset-volume-snapshot --namespace=demo --driver=VolumeSnapshotter --schedule="*/4 * * * *" --target-apiversion=apps/v1 --target-kind=StatefulSet --target-name=stash-demo --replica=1 --volumesnpashotclass=default-snapshot-class --keep-last=5 --prune=true
```

### Options

```
      --driver string                Driver indicates the mechanism used to backup (i.e. VolumeSnapshotter, Restic)
      --dry-run                      Specify whether to test retention policy without deleting actual data
  -h, --help                         help for backupconfig
      --keep-daily int               Specify value for retention strategy
      --keep-hourly int              Specify value for retention strategy
      --keep-last int                Specify value for retention strategy
      --keep-monthly int             Specify value for retention strategy
      --keep-weekly int              Specify value for retention strategy
      --keep-yearly int              Specify value for retention strategy
      --paths strings                List of paths to backup
      --prune                        Specify whether to prune old snapshot data
      --replica int32                Replica specifies the number of replicas whose data should be backed up
      --repo-name string             Name of the Repository
      --repo-namespace string        Namespace of the Repository
      --schedule string              Schedule of the Backup
      --target-apiversion string     API-Version of the target resource
      --target-kind string           Kind of the target resource
      --target-name string           Name of the target resource
      --task string                  Name of the Task
      --volume-mounts strings        List of volumes and their mountPaths
      --volumesnpashotclass string   Name of the VolumeSnapshotClass
```

### Options inherited from parent commands

```
      --as string                      Username to impersonate for the operation. User could be a regular user or a service account in a namespace.
      --as-group stringArray           Group to impersonate for the operation, this flag can be repeated to specify multiple groups.
      --as-uid string                  UID to impersonate for the operation.
      --cache-dir string               Default cache directory (default "/home/runner/.kube/cache")
      --certificate-authority string   Path to a cert file for the certificate authority
      --client-certificate string      Path to a client certificate file for TLS
      --client-key string              Path to a client key file for TLS
      --cluster string                 The name of the kubeconfig cluster to use
      --context string                 The name of the kubeconfig context to use
      --insecure-skip-tls-verify       If true, the server's certificate will not be checked for validity. This will make your HTTPS connections insecure
      --kubeconfig string              Path to the kubeconfig file to use for CLI requests.
      --match-server-version           Require server version to match client version
  -n, --namespace string               If present, the namespace scope for this CLI request
      --request-timeout string         The length of time to wait before giving up on a single server request. Non-zero values should contain a corresponding time unit (e.g. 1s, 2m, 3h). A value of zero means don't timeout requests. (default "0")
  -s, --server string                  The address and port of the Kubernetes API server
      --tls-server-name string         Server name to use for server certificate validation. If it is not provided, the hostname used to contact the server is used
      --token string                   Bearer token for authentication to the API server
      --user string                    The name of the kubeconfig user to use
```

### SEE ALSO

* [kubectl-stash create](/docs/v2023.01.05/reference/cli/kubectl-stash_create)	 - create stash resources

