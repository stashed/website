---
title: REST Backend | Stash
description: Configure Stash to REST Server as Backend.
menu:
  docs_v2021.06.23:
    identifier: backend-rest
    name: REST Server
    parent: backend
    weight: 80
product_name: stash
menu_name: docs_v2021.06.23
section_menu_id: guides
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

- Learn how to use Stash to backup workloads data from [here](/docs/v2021.06.23/guides/latest/workloads/overview).
- Learn how to use Stash to backup databases from [here](/docs/v2021.06.23/guides/latest/addons/overview).
- Learn how to use Stash to backup stand-alone PVC from [here](/docs/v2021.06.23/guides/latest/volumes/overview).
