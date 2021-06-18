---
title: Download
menu:
  docs_v2021.06.23:
    identifier: kubectl-stash-download
    name: Download
    parent: reference-cli
menu_name: docs_v2021.06.23
section_menu_id: reference
info:
  cli: v0.14.1
  community: v0.14.1
  elasticsearch:
  - 5.6.4-v11
  - 6.2.4-v11
  - 6.3.0-v11
  - 6.4.0-v11
  - 6.5.3-v11
  - 6.8.0-v11
  - 7.2.0-v11
  - 7.3.2-v11
  enterprise: v0.14.1
  installer: v2021.06.23
  mariadb:
  - 10.5.8-v4
  mongodb:
  - 3.4.17-v10
  - 3.4.22-v10
  - 3.6.13-v10
  - 3.6.8-v10
  - 4.0.11-v10
  - 4.0.3-v10
  - 4.0.5-v10
  - 4.1.13-v10
  - 4.1.4-v10
  - 4.1.7-v10
  - 4.2.3-v10
  - 4.4.6-v1
  mysql:
  - 5.7.25-v11
  - 8.0.14-v11
  - 8.0.21-v5
  - 8.0.3-v11
  percona-xtradb:
  - 5.7-v6
  postgres:
  - 10.14-v9
  - 11.9-v9
  - 12.4-v9
  - 13.1-v6
  - 9.6.19-v9
  version: v2021.06.23
---

## kubectl-stash download

Download snapshots

### Synopsis

Download contents of snapshots from Repository

```
kubectl-stash download [flags]
```

### Options

```
      --destination string       Destination path where snapshot will be restored.
      --docker-registry string   Docker image registry for restic cli (default "restic")
  -h, --help                     help for download
      --host string              Name of the source host machine (default "host-0")
      --image-tag string         Restic docker image tag (default "0.9.5")
      --paths strings            List of directories to be restored
      --snapshots strings        List of snapshots to be restored
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
      --enable-analytics                 Send analytical events to Google Analytics (default true)
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

* [kubectl-stash](/docs/v2021.06.23/reference/cli/kubectl-stash)	 - kubectl plugin for Stash by AppsCode

