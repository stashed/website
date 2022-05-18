---
title: Create Backupconfig
menu:
  docs_v2022.05.18:
    identifier: kubectl-stash-create-backupconfig
    name: Create Backupconfig
    parent: reference-cli
menu_name: docs_v2022.05.18
section_menu_id: reference
info:
  cli: v0.20.0
  community: v0.20.1
  elasticsearch:
  - 5.6.4-v17
  - 6.2.4-v17
  - 6.3.0-v17
  - 6.4.0-v17
  - 6.5.3-v17
  - 6.8.0-v17
  - 7.14.0-v3
  - 7.2.0-v17
  - 7.3.2-v17
  - 8.2.0
  enterprise: v0.20.1
  etcd:
  - 3.5.0-v4
  installer: v2022.05.18
  kubedump:
  - 0.1.0
  mariadb:
  - 10.5.8-v10
  mongodb:
  - 3.4.17-v16
  - 3.4.22-v16
  - 3.6.13-v16
  - 3.6.8-v16
  - 4.0.11-v16
  - 4.0.3-v16
  - 4.0.5-v16
  - 4.1.13-v16
  - 4.1.4-v16
  - 4.1.7-v16
  - 4.2.3-v16
  - 4.4.6-v7
  - 5.0.3-v4
  mysql:
  - 5.7.25-v17
  - 8.0.14-v17
  - 8.0.21-v11
  - 8.0.3-v17
  nats:
  - 2.6.1-v5
  - 2.8.2
  percona-xtradb:
  - 5.7-v12
  postgres:
  - 10.14-v15
  - 11.9-v15
  - 12.4-v15
  - 13.1-v12
  - 14.0-v4
  - 9.6.19-v15
  redis:
  - 5.0.13-v5
  - 6.2.5-v5
  ui-server: v0.2.0
  version: v2022.05.18
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
      --as string                        Username to impersonate for the operation
      --as-group stringArray             Group to impersonate for the operation, this flag can be repeated to specify multiple groups.
      --cache-dir string                 Default cache directory (default "/home/runner/.kube/cache")
      --certificate-authority string     Path to a cert file for the certificate authority
      --client-certificate string        Path to a client certificate file for TLS
      --client-key string                Path to a client key file for TLS
      --cluster string                   The name of the kubeconfig cluster to use
      --context string                   The name of the kubeconfig context to use
      --insecure-skip-tls-verify         If true, the server's certificate will not be checked for validity. This will make your HTTPS connections insecure
      --kubeconfig string                Path to the kubeconfig file to use for CLI requests.
      --match-server-version             Require server version to match client version
  -n, --namespace string                 If present, the namespace scope for this CLI request
      --request-timeout string           The length of time to wait before giving up on a single server request. Non-zero values should contain a corresponding time unit (e.g. 1s, 2m, 3h). A value of zero means don't timeout requests. (default "0")
  -s, --server string                    The address and port of the Kubernetes API server
      --tls-server-name string           Server name to use for server certificate validation. If it is not provided, the hostname used to contact the server is used
      --token string                     Bearer token for authentication to the API server
      --use-kubeapiserver-fqdn-for-aks   if true, uses kube-apiserver FQDN for AKS cluster to workaround https://github.com/Azure/AKS/issues/522 (default true)
      --user string                      The name of the kubeconfig user to use
```

### SEE ALSO

* [kubectl-stash create](/docs/v2022.05.18/reference/cli/kubectl-stash_create)	 - create stash resources

