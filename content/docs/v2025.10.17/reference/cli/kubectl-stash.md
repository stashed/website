---
title: Kubectl-Stash
menu:
  docs_v2025.10.17:
    identifier: kubectl-stash
    name: Kubectl-Stash
    parent: reference-cli
    weight: 0
menu_name: docs_v2025.10.17
section_menu_id: reference
url: /docs/v2025.10.17/reference/cli/
aliases:
- /docs/v2025.10.17/reference/cli/kubectl-stash/
info:
  cli: v0.42.0
  community: v0.42.0
  elasticsearch:
  - 5.6.4-v38
  - 6.2.4-v38
  - 6.3.0-v38
  - 6.4.0-v38
  - 6.5.3-v38
  - 6.8.0-v38
  - 7.14.0-v24
  - 7.2.0-v38
  - 7.3.2-v38
  - 8.2.0-v21
  enterprise: v0.42.0
  etcd:
  - 3.5.0-v25
  installer: v2025.10.17
  kubedump:
  - 0.2.0-v6
  mariadb:
  - 10.6.23-v1
  mongodb:
  - 3.4.17-v39
  - 3.4.22-v39
  - 3.6.13-v39
  - 3.6.8-v39
  - 4.0.11-v39
  - 4.0.3-v39
  - 4.0.5-v39
  - 4.1.13-v39
  - 4.1.4-v39
  - 4.1.7-v39
  - 4.2.3-v39
  - 4.4.6-v30
  - 5.0.15-v12
  - 5.0.3-v27
  - 6.0.5-v15
  mysql:
  - 5.7.25-v39
  - 8.0.14-v38
  - 8.0.21-v32
  - 8.0.3-v38
  nats:
  - 2.6.1-v26
  - 2.8.2-v21
  percona-xtradb:
  - 5.7-v33
  postgres:
  - 10.14-v37
  - 11.9-v37
  - 12.4-v37
  - 13.1-v34
  - 14.0-v26
  - 15.1-v18
  - 16.1-v7
  - 17.2-v5
  - 9.6.19-v37
  redis:
  - 5.0.13-v26
  - 6.2.5-v26
  - 7.0.5-v19
  ui-server: v0.23.0
  vault:
  - 1.10.3-v18
  version: v2025.10.17
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

* [kubectl-stash check](/docs/v2025.10.17/reference/cli/kubectl-stash_check)	 - Check the repository for errors
* [kubectl-stash clone](/docs/v2025.10.17/reference/cli/kubectl-stash_clone)	 - Clone Kubernetes resources
* [kubectl-stash completion](/docs/v2025.10.17/reference/cli/kubectl-stash_completion)	 - Generate completion script
* [kubectl-stash cp](/docs/v2025.10.17/reference/cli/kubectl-stash_cp)	 - Copy stash resources from one namespace to another namespace
* [kubectl-stash create](/docs/v2025.10.17/reference/cli/kubectl-stash_create)	 - create stash resources
* [kubectl-stash debug](/docs/v2025.10.17/reference/cli/kubectl-stash_debug)	 - Debug common Stash issues
* [kubectl-stash delete](/docs/v2025.10.17/reference/cli/kubectl-stash_delete)	 - Delete stash resources
* [kubectl-stash download](/docs/v2025.10.17/reference/cli/kubectl-stash_download)	 - Download snapshots
* [kubectl-stash gen](/docs/v2025.10.17/reference/cli/kubectl-stash_gen)	 - generate stash resources
* [kubectl-stash key](/docs/v2025.10.17/reference/cli/kubectl-stash_key)	 - manages restic keys (passwords) for accessing the repository
* [kubectl-stash migrate](/docs/v2025.10.17/reference/cli/kubectl-stash_migrate)	 - Migrate restic repository to v2
* [kubectl-stash pause](/docs/v2025.10.17/reference/cli/kubectl-stash_pause)	 - Pause Stash backup temporarily
* [kubectl-stash prune](/docs/v2025.10.17/reference/cli/kubectl-stash_prune)	 - Prune restic repository
* [kubectl-stash rebuild-index](/docs/v2025.10.17/reference/cli/kubectl-stash_rebuild-index)	 - Build a new index
* [kubectl-stash resume](/docs/v2025.10.17/reference/cli/kubectl-stash_resume)	 - Resume Stash backup
* [kubectl-stash trigger](/docs/v2025.10.17/reference/cli/kubectl-stash_trigger)	 - Trigger a backup
* [kubectl-stash unlock](/docs/v2025.10.17/reference/cli/kubectl-stash_unlock)	 - Unlock restic repository
* [kubectl-stash version](/docs/v2025.10.17/reference/cli/kubectl-stash_version)	 - Prints binary version number.

