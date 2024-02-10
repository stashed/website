---
title: Kubectl-Stash
menu:
  docs_v2024.2.9-rc.0:
    identifier: kubectl-stash
    name: Kubectl-Stash
    parent: reference-cli
    weight: 0
menu_name: docs_v2024.2.9-rc.0
section_menu_id: reference
url: /docs/v2024.2.9-rc.0/reference/cli/
aliases:
- /docs/v2024.2.9-rc.0/reference/cli/kubectl-stash/
info:
  cli: v0.33.0-rc.0
  community: v0.33.0-rc.0
  elasticsearch:
  - 5.6.4-v29
  - 6.2.4-v29
  - 6.3.0-v29
  - 6.4.0-v29
  - 6.5.3-v29
  - 6.8.0-v29
  - 7.14.0-v15
  - 7.2.0-v29
  - 7.3.2-v29
  - 8.2.0-v12
  enterprise: v0.33.0-rc.0
  etcd:
  - 3.5.0-v16
  installer: v2024.2.9-rc.0
  kubedump:
  - 0.1.0-v12
  mariadb:
  - 10.5.8-v23
  mongodb:
  - 3.4.17-v29
  - 3.4.22-v29
  - 3.6.13-v29
  - 3.6.8-v29
  - 4.0.11-v29
  - 4.0.3-v29
  - 4.0.5-v29
  - 4.1.13-v29
  - 4.1.4-v29
  - 4.1.7-v29
  - 4.2.3-v29
  - 4.4.6-v20
  - 5.0.15-v2
  - 5.0.3-v17
  - 6.0.5-v5
  mysql:
  - 5.7.25-v29
  - 8.0.14-v29
  - 8.0.21-v23
  - 8.0.3-v29
  nats:
  - 2.6.1-v17
  - 2.8.2-v12
  percona-xtradb:
  - 5.7-v24
  postgres:
  - 10.14-v28
  - 11.9-v28
  - 12.4-v28
  - 13.1-v25
  - 14.0-v17
  - 15.1-v9
  - 9.6.19-v28
  redis:
  - 5.0.13-v17
  - 6.2.5-v17
  - 7.0.5-v10
  ui-server: v0.14.0-rc.0
  vault:
  - 1.10.3-v9
  version: v2024.2.9-rc.0
---

## kubectl-stash

kubectl plugin for Stash by AppsCode

### Synopsis

kubectl plugin for Stash by AppsCode. For more information, visit here: https://appscode.com/products/stash

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

* [kubectl-stash clone](/docs/v2024.2.9-rc.0/reference/cli/kubectl-stash_clone)	 - Clone Kubernetes resources
* [kubectl-stash completion](/docs/v2024.2.9-rc.0/reference/cli/kubectl-stash_completion)	 - Generate completion script
* [kubectl-stash cp](/docs/v2024.2.9-rc.0/reference/cli/kubectl-stash_cp)	 - Copy stash resources from one namespace to another namespace
* [kubectl-stash create](/docs/v2024.2.9-rc.0/reference/cli/kubectl-stash_create)	 - create stash resources
* [kubectl-stash debug](/docs/v2024.2.9-rc.0/reference/cli/kubectl-stash_debug)	 - Debug common Stash issues
* [kubectl-stash delete](/docs/v2024.2.9-rc.0/reference/cli/kubectl-stash_delete)	 - Delete stash resources
* [kubectl-stash download](/docs/v2024.2.9-rc.0/reference/cli/kubectl-stash_download)	 - Download snapshots
* [kubectl-stash gen](/docs/v2024.2.9-rc.0/reference/cli/kubectl-stash_gen)	 - generate stash resources
* [kubectl-stash key](/docs/v2024.2.9-rc.0/reference/cli/kubectl-stash_key)	 - manages restic keys (passwords) for accessing the repository
* [kubectl-stash pause](/docs/v2024.2.9-rc.0/reference/cli/kubectl-stash_pause)	 - Pause Stash backup temporarily
* [kubectl-stash resume](/docs/v2024.2.9-rc.0/reference/cli/kubectl-stash_resume)	 - Resume Stash backup
* [kubectl-stash trigger](/docs/v2024.2.9-rc.0/reference/cli/kubectl-stash_trigger)	 - Trigger a backup
* [kubectl-stash unlock](/docs/v2024.2.9-rc.0/reference/cli/kubectl-stash_unlock)	 - Unlock restic repository
* [kubectl-stash version](/docs/v2024.2.9-rc.0/reference/cli/kubectl-stash_version)	 - Prints binary version number.

