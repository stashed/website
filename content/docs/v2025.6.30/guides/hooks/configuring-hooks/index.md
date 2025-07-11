---
title: Hooks Examples | Stash
menu:
  docs_v2025.6.30:
    identifier: configuring-hooks
    name: Configuring Hooks
    parent: hooks
    weight: 40
product_name: stash
menu_name: docs_v2025.6.30
section_menu_id: guides
info:
  cli: v0.40.0
  community: v0.40.0
  elasticsearch:
  - 5.6.4-v36
  - 6.2.4-v36
  - 6.3.0-v36
  - 6.4.0-v36
  - 6.5.3-v36
  - 6.8.0-v36
  - 7.14.0-v22
  - 7.2.0-v36
  - 7.3.2-v36
  - 8.2.0-v19
  enterprise: v0.40.0
  etcd:
  - 3.5.0-v23
  installer: v2025.6.30
  kubedump:
  - 0.2.0-v4
  mariadb:
  - 10.5.8-v30
  mongodb:
  - 3.4.17-v37
  - 3.4.22-v37
  - 3.6.13-v37
  - 3.6.8-v37
  - 4.0.11-v37
  - 4.0.3-v37
  - 4.0.5-v37
  - 4.1.13-v37
  - 4.1.4-v37
  - 4.1.7-v37
  - 4.2.3-v37
  - 4.4.6-v28
  - 5.0.15-v10
  - 5.0.3-v25
  - 6.0.5-v13
  mysql:
  - 5.7.25-v37
  - 8.0.14-v36
  - 8.0.21-v30
  - 8.0.3-v36
  nats:
  - 2.6.1-v24
  - 2.8.2-v19
  percona-xtradb:
  - 5.7-v31
  postgres:
  - 10.14-v35
  - 11.9-v35
  - 12.4-v35
  - 13.1-v32
  - 14.0-v24
  - 15.1-v16
  - 16.1-v5
  - 17.2-v3
  - 9.6.19-v35
  redis:
  - 5.0.13-v24
  - 6.2.5-v24
  - 7.0.5-v17
  ui-server: v0.21.0
  vault:
  - 1.10.3-v16
  version: v2025.6.30
---

# Configuring Different Types of Hooks

In this guide, we are going to discuss how to configure different types of hooks. Here, we will give some examples of different configurations.

## HTTPGet

`httpGet` hook allows sending an HTTP GET request to an HTTP server before and after the backup/restore process. The following configurable fields are available for `httpGet` hook.

| Field         | Field Type | Data Type             | Usage                                                                                                                                                                                                                                                                                    |
| ------------- | ---------- | --------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `host`        | `Optional` | `String`              | Specify the name of the host where the GET request will be sent.  If you don't specify this field, Stash will use the hook executor Pod IP as host.                                                                                                                                      |
| `port`        | `Required` | `Integer` or `String` | Specify the port where the HTTP server is listening. You can specify a numeric port or the name of the port in case of the HTTP server is running in the same pod where the hook will be executed. If you specify the port name, you must specify the `containerName` field of the hook. |
| `path`        | `Required` | `String`              | Specify the path to access on the HTTP server.                                                                                                                                                                                                                                           |
| `scheme`      | `Required` | `String`              | Specify the scheme to use to connect with the host.                                                                                                                                                                                                                                      |
| `httpHeaders` | `Optional` | Array of Objects      | Specify a list of custom headers to set in the request.                                                                                                                                                                                                                                  |

**Examples:**

- Hook with `host` and `port` are specified:

```yaml
preBackup:
  httpGet:
    host: my-host.demo.svc
    port: 8080
    path: "/"
    scheme: HTTP
```

- Hook to extract `host` and `port` from the hook executor pod:

```yaml
preBackup:
  httpGet:
    port: my-port
    path: "/"
    scheme: HTTP
  containerName: my-container
```

- Hook for sending custom header in the HTTP request:

```yaml
preBackup:
  httpGet:
    host: my-host.demo.svc
    port: 8080
    path: "/"
    scheme: HTTP
    httpHeaders:
    - name: "User-Agent"
      value: "stash/0.9.x"
```

## HTTPPost

`httpPost` hook allows sending an HTTP POST request to an HTTP server before and after the backup/restore process. The following configurable fields are available for `httpPost` hook.

| Field         | Field Type | Data Type             | Usage                                                                                                                                                                                                                                                                             |
| ------------- | ---------- | --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `host`        | `Optional` | `String`              | Specify the name of the host where the POST request will be sent. If you don't specify this field, Stash will use the hook executor Pod IP as host.                                                                                                                               |
| `port`        | `Required` | `Integer` or `String` | Specify the port where the HTTP server is listening. You can specify a numeric port or the name of the port if your HTTP server is running in the same pod where the hook will be executed. If you specify the port name, you must specify the `containerName` field of the hook. |
| `path`        | `Required` | `String`              | Specify the path to access on the HTTP server.                                                                                                                                                                                                                                    |
| `scheme`      | `Required` | `String`              | Specify the scheme to use to connect with the host.                                                                                                                                                                                                                               |
| `httpHeaders` | `Optional` | Array of Objects      | Specify a list of custom headers to set in the request.                                                                                                                                                                                                                           |
| `body`        | `Optional` | `String`              | Specify the data to send in the request body.                                                                                                                                                                                                                                     |
| `form`        | `Optional` | Array of Objects      | Specify the data to send as URL encoded form with the request.                                                                                                                                                                                                                    |

**Examples:**

- Send JSON data in the request body:

```yaml
preBackup:
  httpPost:
    host: my-service.mynamespace.svc
    path: /demo
    port: 8080
    scheme: HTTP
    httpHeaders:
    - name: Content-Type
      value: application/json
    body: '{
            "name": "john doe",
            "age": "28"
           }'
```

- Send URL encoded form in the request:

```yaml
preBackup:
  httpPost:
    host: my-service.mynamespace.svc
    path: /demo
    port: 8080
    scheme: HTTP
    form:
    - key: "name"
      values: ["john doe"]
    - key: "age"
      values: ["28"]
```

## TCPSocket

`tcpSocket` hook allows running a check against a TCP port to verify whether it is open or not. The following configurable fields are available for `tcpSocket` hook.

| Field  | Field Type | Data Type             | Usage                                                                                                                                                                                                                                                |
| ------ | ---------- | --------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `host` | `Optional` | `String`              | Specify the name of the host where the server is running. If you don't specify this field, Stash will use the hook executor Pod IP as host.                                                                                                          |
| `port` | `Required` | `Integer` or `String` | Specify the port to check. You can specify a numeric port or the name of the port when your server is running in the same pod where the hook will be executed. If you specify the port name, you must specify the `containerName` field of the hook. |

**Examples:**

- Hook with `host` and `port` field specified.

```yaml
preBackup:
  tcpSocket:
    host: my-host.demo.svc
    port: 8080
```

- Hook to extract `host` and `port` from the hook executor pod.

```yaml
preBackup:
  tcpSocket:
    port: my-port
  containerName: my-container
```

## Exec

`exec` hook allows running commands inside a container before or after the backup/restore process. The following configurable fields are available for `exec` hook.

| Field     | Field Type | Data Type        | Usage                                  |
| --------- | ---------- | ---------------- | -------------------------------------- |
| `command` | `Required` | Array of Strings | Specify a list of commands to execute. |

> If you don't specify `containerName` for `exec` hook, the command will be executed into the 0th container of the respective pod.

**Examples:**

- Hook to remove old corrupted data before restore:

```yaml
preRestore:
  exec:
    command: ["/bin/sh","-c","rm -rf /corrupted/data/directory"]
  containerName: my-container
```

- Hook to make a MySQL database read-only before backup:

```yaml
preBackup:
  exec:
    command:
      - /bin/sh
      - -c
      - mysql -u root --password=$MYSQL_ROOT_PASSWORD -e "SET GLOBAL super_read_only = ON;"
  containerName: mysql
```

- Hook to make a MySQL database writable after backup:

```yaml
postBackup:
  exec:
    command:
      - /bin/sh
      - -c
      - mysql -u root --password=$MYSQL_ROOT_PASSWORD -e "SET GLOBAL super_read_only = OFF;"
  containerName: mysql
```

- Hook to remove corrupted MySQL database before restoring:

```yaml
preRestore:
  exec:
    command:
      - /bin/sh
      - -c
      - mysql -u root --password=$MYSQL_ROOT_PASSWORD -e "DROP DATABASE companyRecord;"
  containerName: mysql
```

- Hook to apply some migration after restoring a MySQL database:

```yaml
postRestore:
  exec:
    command:
      - /bin/sh
      - -c
      - mysql -u root --password=$MYSQL_ROOT_PASSWORD -e "RENAME TABLE companyRecord.employee TO companyRecord.salaryRecord;"
  containerName: mysql
```

If the above hooks do not cover your use cases or you have any questions regarding the hooks, please feel free to open an issue [here](https://github.com/stashed/stash/issues).
