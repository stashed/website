---
title: REST Backend | Stash
description: Configure Stash to REST Server as Backend.
menu:
  docs_v2021.10.11:
    identifier: backend-rest
    name: REST Server
    parent: backend
    weight: 80
product_name: stash
menu_name: docs_v2021.10.11
section_menu_id: guides
info:
  cli: v0.16.0
  community: v0.16.0
  elasticsearch:
  - 5.6.4-v13
  - 6.2.4-v13
  - 6.3.0-v13
  - 6.4.0-v13
  - 6.5.3-v13
  - 6.8.0-v13
  - 7.2.0-v13
  - 7.3.2-v13
  enterprise: v0.16.0
  etcd:
  - 3.5.0
  installer: v2021.10.11
  mariadb:
  - 10.5.8-v6
  mongodb:
  - 3.4.17-v12
  - 3.4.22-v12
  - 3.6.13-v12
  - 3.6.8-v12
  - 4.0.11-v12
  - 4.0.3-v12
  - 4.0.5-v12
  - 4.1.13-v12
  - 4.1.4-v12
  - 4.1.7-v12
  - 4.2.3-v12
  - 4.4.6-v3
  - 5.0.3
  mysql:
  - 5.7.25-v13
  - 8.0.14-v13
  - 8.0.21-v7
  - 8.0.3-v13
  nats:
  - 2.6.1
  percona-xtradb:
  - 5.7-v8
  postgres:
  - 10.14-v11
  - 11.9-v11
  - 12.4-v11
  - 13.1-v8
  - "14.0"
  - 9.6.19-v11
  redis:
  - 5.0.13-v1
  - 6.2.5-v1
  version: v2021.10.11
---

# REST Backend

Stash supports restic's [REST Server](https://github.com/restic/rest-server) as a backend. This tutorial will show you how to use this backend.

In order to use REST Server as backend, you have to create a `Secret` and a `Repository` object pointing to the desired REST Server address.

#### Create Storage Secret

To configure storage secret for this backend, following secret keys are needed:

|          Key           |    Type    |                                                                              Description                                                                              |
| ---------------------- | ---------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `RESTIC_PASSWORD`      | `Required` | Password that will be used to encrypt the backup snapshots.                                                                                                           |
| `REST_SERVER_USERNAME` | `Optional` | Username for basic authentication in the REST server.                                                                                                                 |
| `REST_SERVER_PASSWORD` | `Optional` | Password for basic authentication in the REST Server                                                                                                                  |
| `CA_CERT_DATA`         | `optional` | CA certificate used by storage backend. This can be used to pass the root certificate that has been used to sign the server certificate of a TLS secured REST Server. |

Create storage secret as below,

```bash
$ echo -n 'changeit' > RESTIC_PASSWORD
$ echo -n '<your-rest-server-username>' > REST_SERVER_USERNAME
$ echo -n '<your-rest-server-password>' > REST_SERVER_PASSWORD
$ kubectl create secret generic -n demo rest-secret \
    --from-file=./RESTIC_PASSWORD \
    --from-file=./REST_SERVER_USERNAME \
    --from-file=./REST_SERVER_PASSWORD
secret/rest-secret created
```

### Create Repository

Now, you have to create a `Repository` crd. You have to provide the storage secret that we have created earlier in `spec.backend.storageSecretName` field.

Following parameters are available for `rest` backend,

| Parameter  |    Type    |                                                  Description                                                  |
| ---------- | ---------- | ------------------------------------------------------------------------------------------------------------- |
| `rest.url` | `Required` | URL of the REST Server along with an optional path inside the server where backed up snapshot will be stored. |

Below, the YAML of a sample `Repository` crd that uses a REST Server as a backend.

```yaml
apiVersion: stash.appscode.com/v1alpha1
kind: Repository
metadata:
  name: rest-repo
  namespace: demo
spec:
  backend:
    rest:
      url: http://rest-server.demo.svc:8000/stash-backup-demo
    storageSecretName: rest-secret
```

Create the `Repository` we have shown above using the following command,

```bash
$ kubectl apply -f https://github.com/stashed/docs/raw/{{< param "info.version" >}}/docs/examples/guides/latest/backends/rest.yaml
repository/rest-repo created
```

Now, we are ready to use this backend to backup our desired data using Stash.

## Next Steps

- Learn how to use Stash to backup workloads data from [here](/docs/v2021.10.11/guides/latest/workloads/overview).
- Learn how to use Stash to backup databases from [here](/docs/v2021.10.11/guides/latest/addons/overview).
- Learn how to use Stash to backup stand-alone PVC from [here](/docs/v2021.10.11/guides/latest/volumes/overview).
