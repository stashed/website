---
title: Kubectl-Stash
menu:
  docs_v2025.1.9:
    identifier: kubectl-stash
    name: Kubectl-Stash
    parent: reference-cli
    weight: 0
menu_name: docs_v2025.1.9
section_menu_id: reference
url: /docs/v2025.1.9/reference/cli/
aliases:
- /docs/v2025.1.9/reference/cli/kubectl-stash/
info:
  cli: v0.38.0
  community: v0.38.0
  elasticsearch:
  - 5.6.4-v34
  - 6.2.4-v34
  - 6.3.0-v34
  - 6.4.0-v34
  - 6.5.3-v34
  - 6.8.0-v34
  - 7.14.0-v20
  - 7.2.0-v34
  - 7.3.2-v34
  - 8.2.0-v17
  enterprise: v0.38.0
  etcd:
  - 3.5.0-v21
  installer: v2025.1.9
  kubedump:
  - 0.2.0-v2
  mariadb:
  - 10.5.8-v28
  mongodb:
  - 3.4.17-v35
  - 3.4.22-v35
  - 3.6.13-v35
  - 3.6.8-v35
  - 4.0.11-v35
  - 4.0.3-v35
  - 4.0.5-v35
  - 4.1.13-v35
  - 4.1.4-v35
  - 4.1.7-v35
  - 4.2.3-v35
  - 4.4.6-v26
  - 5.0.15-v8
  - 5.0.3-v23
  - 6.0.5-v11
  mysql:
  - 5.7.25-v35
  - 8.0.14-v34
  - 8.0.21-v28
  - 8.0.3-v34
  nats:
  - 2.6.1-v22
  - 2.8.2-v17
  percona-xtradb:
  - 5.7-v29
  postgres:
  - 10.14-v33
  - 11.9-v33
  - 12.4-v33
  - 13.1-v30
  - 14.0-v22
  - 15.1-v14
  - 16.1-v3
  - 17.2-v1
  - 9.6.19-v33
  redis:
  - 5.0.13-v22
  - 6.2.5-v22
  - 7.0.5-v15
  ui-server: v0.19.0
  vault:
  - 1.10.3-v14
  version: v2025.1.9
---

## kubectl-stash

kubectl plugin for Stash by AppsCode

### Synopsis

kubectl plugin for Stash by AppsCode. For more information, visit here: https://stash.run

### Options

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
      --disable-compression            If true, opt-out of response compression for all requests to the server
  -h, --help                           help for kubectl-stash
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

* [kubectl-stash check](/docs/v2025.1.9/reference/cli/kubectl-stash_check)	 - Check the repository for errors
* [kubectl-stash clone](/docs/v2025.1.9/reference/cli/kubectl-stash_clone)	 - Clone Kubernetes resources
* [kubectl-stash completion](/docs/v2025.1.9/reference/cli/kubectl-stash_completion)	 - Generate completion script
* [kubectl-stash cp](/docs/v2025.1.9/reference/cli/kubectl-stash_cp)	 - Copy stash resources from one namespace to another namespace
* [kubectl-stash create](/docs/v2025.1.9/reference/cli/kubectl-stash_create)	 - create stash resources
* [kubectl-stash debug](/docs/v2025.1.9/reference/cli/kubectl-stash_debug)	 - Debug common Stash issues
* [kubectl-stash delete](/docs/v2025.1.9/reference/cli/kubectl-stash_delete)	 - Delete stash resources
* [kubectl-stash download](/docs/v2025.1.9/reference/cli/kubectl-stash_download)	 - Download snapshots
* [kubectl-stash gen](/docs/v2025.1.9/reference/cli/kubectl-stash_gen)	 - generate stash resources
* [kubectl-stash key](/docs/v2025.1.9/reference/cli/kubectl-stash_key)	 - manages restic keys (passwords) for accessing the repository
* [kubectl-stash pause](/docs/v2025.1.9/reference/cli/kubectl-stash_pause)	 - Pause Stash backup temporarily
* [kubectl-stash rebuild-index](/docs/v2025.1.9/reference/cli/kubectl-stash_rebuild-index)	 - Build a new index
* [kubectl-stash resume](/docs/v2025.1.9/reference/cli/kubectl-stash_resume)	 - Resume Stash backup
* [kubectl-stash trigger](/docs/v2025.1.9/reference/cli/kubectl-stash_trigger)	 - Trigger a backup
* [kubectl-stash unlock](/docs/v2025.1.9/reference/cli/kubectl-stash_unlock)	 - Unlock restic repository
* [kubectl-stash version](/docs/v2025.1.9/reference/cli/kubectl-stash_version)	 - Prints binary version number.

