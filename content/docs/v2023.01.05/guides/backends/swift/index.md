---
title: Swift Backend | Stash
description: Configure Stash to use OpenStack Swift as Backend.
menu:
  docs_v2023.01.05:
    identifier: backend-swift
    name: OpenStack Swift
    parent: backend
    weight: 60
product_name: stash
menu_name: docs_v2023.01.05
section_menu_id: guides
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

# OpenStack Swift

Stash supports [OpenStack Swift](https://docs.openstack.org/swift/latest/) as a backend. This tutorial will show you how to use this backend.

In order to use OpenStack Swift as backend, you have to create a `Secret` and a `Repository` object pointing to the desired Swift container.

>If the Swift container does not exist yet, Stash will automatically create it during the first backup.

#### Create Storage Secret

Stash supports Swift's Keystone v1, v2, v3 authentication as well as token-based authentication.

**Keystone v1 authentication:**

For keystone v1 authentication, following secret keys are needed:

| Key                      | Description                                                |
|--------------------------|------------------------------------------------------------|
| `RESTIC_PASSWORD`        | Password used that will be used to encrypt the backup snapshots.|
| `ST_AUTH`                | URL of the Keystone server.                             |
| `ST_USER`                | Username.                             |
| `ST_KEY`                 | Password.                             |

**Keystone v2 authentication:**

For keystone v2 authentication, following secret keys are needed:

| Key                      | Description                                                |
|--------------------------|------------------------------------------------------------|
| `RESTIC_PASSWORD`        | Password used that will be used to encrypt the backup snapshots.|
| `OS_AUTH_URL`            | URL of the Keystone server.                             |
| `OS_REGION_NAME`         | Storage region name                              |
| `OS_USERNAME`            | Username                             |
| `OS_PASSWORD`            | Password                             |
| `OS_TENANT_ID`           | Tenant ID                             |
| `OS_TENANT_NAME`         | Tenant Name                             |

**Keystone v3 authentication:**

For keystone v3 authentication, following secret keys are needed:

| Key                      | Description                                                |
|--------------------------|------------------------------------------------------------|
| `RESTIC_PASSWORD`        | Password used that will be used to encrypt the backup snapshots.|
| `OS_AUTH_URL`            | URL of the Keystone server.                             |
| `OS_REGION_NAME`         | Storage region name                             |
| `OS_USERNAME`            | Username                             |
| `OS_PASSWORD`            | Password                             |
| `OS_USER_DOMAIN_NAME`    | User domain name                             |
| `OS_PROJECT_NAME`        | Project name                             |
| `OS_PROJECT_DOMAIN_NAME` | Project domain name                             |

For keystone v3 application credential authentication (application credential id):

| Key                      | Description                                                |
|--------------------------|------------------------------------------------------------|
| `RESTIC_PASSWORD`        | Password used that will be used to encrypt the backup snapshots.|
| `OS_AUTH_URL`            | URL of the Keystone server.                             |
| `OS_APPLICATION_CREDENTIAL_ID` | The ID of the application credential used for authentication. If not provided, the application credential must be identified by its name and its owning user.|
| `OS_APPLICATION_CREDENTIAL_SECRET` | The secret for authenticating the application credential. |

For keystone v3 application credential authentication (application credential name):

| Key                      | Description                                                |
|--------------------------|------------------------------------------------------------|
| `RESTIC_PASSWORD`        | Password used that will be used to encrypt the backup snapshots.|
| `OS_AUTH_URL`            | URL of the Keystone server.                             |
| `OS_USERNAME` | User name|
| `OS_USER_DOMAIN_NAME` | User domain name|
| `OS_APPLICATION_CREDENTIAL_NAME` | The name of the application credential used for authentication. If provided, must be accompanied by a user object. |
| `OS_APPLICATION_CREDENTIAL_SECRET` | The secret for authenticating the application credential. |

**Token-based authentication:**

For token-based authentication, following secret keys are needed:

| Key                      | Description                                                |
|--------------------------|------------------------------------------------------------|
| `RESTIC_PASSWORD`        | Password used that will be used to encrypt the backup snapshots.|
| `OS_STORAGE_URL`         | Storage URL                         |
| `OS_AUTH_TOKEN`          | Authentication token                         |

A sample storage secret creation for keystone v2 authentication is shown below,

```bash
$ echo -n 'changeit' > RESTIC_PASSWORD
$ echo -n '<your-auth-url>' > OS_AUTH_URL
$ echo -n '<your-tenant-id>' > OS_TENANT_ID
$ echo -n '<your-tenant-name>' > OS_TENANT_NAME
$ echo -n '<your-username>' > OS_USERNAME
$ echo -n '<your-password>' > OS_PASSWORD
$ echo -n '<your-region>' > OS_REGION_NAME
$ kubectl create secret generic swift-secret \
    --from-file=./RESTIC_PASSWORD \
    --from-file=./OS_AUTH_URL \
    --from-file=./OS_TENANT_ID \
    --from-file=./OS_TENANT_NAME \
    --from-file=./OS_USERNAME \
    --from-file=./OS_PASSWORD \
    --from-file=./OS_REGION_NAME
secret/swift-secret created
```

### Create Repository

Now, you have to create a `Repository` crd. You have to provide the storage secret that we have created earlier in `spec.backend.storageSecretName` field.

Following parameters are available for `Swift` backend.

|     Parameter     |                                  Description                                   |
| ----------------- | ------------------------------------------------------------------------------ |
| `swift.container` | `Required`. Name of Storage container                                          |
| `swift.prefix`    | `Optional`. Path prefix inside the container where backed up data will be stored. |

Below, the YAML of a sample `Repository` crd that uses a Swift container as a backend.

```yaml
apiVersion: stash.appscode.com/v1alpha1
kind: Repository
metadata:
  name: swift-repo
  namespace: demo
spec:
  backend:
    swift:
      container: stash-backup
      prefix: /demo/deployment/my-deploy
    storageSecretName: swift-secret
```

Create the `Repository` we have shown above using the following command,

```bash
$ kubectl apply -f https://github.com/stashed/docs/raw/{{< param "info.version" >}}/docs/guides/backends/swift/examples/swift.yaml
repository/swift-repo created
```

Now, we are ready to use this backend to backup our desired data using Stash.

## Next Steps

- Learn how to use Stash to backup workloads data from [here](/docs/v2023.01.05/guides/workloads/overview/).
- Learn how to use Stash to backup databases from [here](/docs/v2023.01.05/guides/addons/overview/).
- Learn how to use Stash to backup stand-alone PVC from [here](/docs/v2023.01.05/guides/volumes/overview/).
