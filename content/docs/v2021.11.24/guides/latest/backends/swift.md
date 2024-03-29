---
title: Swift Backend | Stash
description: Configure Stash to use OpenStack Swift as Backend.
menu:
  docs_v2021.11.24:
    identifier: backend-swift
    name: OpenStack Swift
    parent: backend
    weight: 60
product_name: stash
menu_name: docs_v2021.11.24
section_menu_id: guides
info:
  cli: v0.17.0
  community: v0.17.0
  elasticsearch:
  - 5.6.4-v14
  - 6.2.4-v14
  - 6.3.0-v14
  - 6.4.0-v14
  - 6.5.3-v14
  - 6.8.0-v14
  - 7.14.0
  - 7.2.0-v14
  - 7.3.2-v14
  enterprise: v0.17.0
  etcd:
  - 3.5.0-v1
  installer: v2021.11.24
  mariadb:
  - 10.5.8-v7
  mongodb:
  - 3.4.17-v13
  - 3.4.22-v13
  - 3.6.13-v13
  - 3.6.8-v13
  - 4.0.11-v13
  - 4.0.3-v13
  - 4.0.5-v13
  - 4.1.13-v13
  - 4.1.4-v13
  - 4.1.7-v13
  - 4.2.3-v13
  - 4.4.6-v4
  - 5.0.3-v1
  mysql:
  - 5.7.25-v14
  - 8.0.14-v14
  - 8.0.21-v8
  - 8.0.3-v14
  nats:
  - 2.6.1-v1
  percona-xtradb:
  - 5.7-v9
  postgres:
  - 10.14-v12
  - 11.9-v12
  - 12.4-v12
  - 13.1-v9
  - 14.0-v1
  - 9.6.19-v12
  redis:
  - 5.0.13-v2
  - 6.2.5-v2
  version: v2021.11.24
---

# OpenStack Swift

Stash supports [OpenStack Swift](https://www.swiftstack.com/product/open-source/openstack-swift) as a backend. This tutorial will show you how to use this backend.

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
$ kubectl apply -f https://github.com/stashed/docs/raw/{{< param "info.version" >}}/docs/examples/guides/latest/backends/swift.yaml
repository/swift-repo created
```

Now, we are ready to use this backend to backup our desired data using Stash.

## Next Steps

- Learn how to use Stash to backup workloads data from [here](/docs/v2021.11.24/guides/latest/workloads/overview).
- Learn how to use Stash to backup databases from [here](/docs/v2021.11.24/guides/latest/addons/overview).
- Learn how to use Stash to backup stand-alone PVC from [here](/docs/v2021.11.24/guides/latest/volumes/overview).
