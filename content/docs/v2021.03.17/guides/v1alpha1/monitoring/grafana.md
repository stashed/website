---
title: Use Grafana | Stash
description: Using Grafana dashboard to visualize Stash monitoring data
menu:
  docs_v2021.03.17:
    identifier: v1alpha1-monitoring-grafana
    name: Using Grafana
    parent: v1alpha1-monitoring
    weight: 40
product_name: stash
menu_name: docs_v2021.03.17
section_menu_id: guides
info:
  cli: v0.12.0
  community: v0.12.0
  elasticsearch:
  - 5.6.4-v7
  - 6.2.4-v7
  - 6.3.0-v7
  - 6.4.0-v7
  - 6.5.3-v7
  - 6.8.0-v7
  - 7.2.0-v7
  - 7.3.2-v7
  enterprise: v0.12.0
  installer: v0.12.0
  mariadb:
  - 10.5.8-v1
  mongodb:
  - 3.4.17-v6
  - 3.4.22-v6
  - 3.6.13-v6
  - 3.6.8-v6
  - 4.0.11-v6
  - 4.0.3-v6
  - 4.0.5-v6
  - 4.1.13-v6
  - 4.1.4-v6
  - 4.1.7-v6
  - 4.2.3-v6
  mysql:
  - 5.7.25-v7
  - 8.0.14-v7
  - 8.0.21-v1
  - 8.0.3-v7
  percona-xtradb:
  - 5.7-v2
  postgres:
  - 10.14-v5
  - 11.9-v5
  - 12.4-v5
  - 13.1-v2
  - 9.6.19-v5
  version: v2021.03.17
---

# Use Grafana Dashboard

Grafana provides an elegant graphical user interface to visualize data. You can create beautiful dashboard easily with a meaningful representation of your Prometheus metrics.

## Before You Begin

- At first, you need to have a Kubernetes cluster, and the kubectl command-line tool must be configured to communicate with your cluster. If you do not already have a cluster, you can create one by using [kind](https://kind.sigs.k8s.io/docs/user/quick-start/).

- You must have a Stash instant running with monitoring enabled. You can enable monitoring by following the guides for builtin [Prometheus](/docs/v2021.03.17/guides/v1alpha1/monitoring/builtin) scraper or [CoreOS Prometheus Operator](/docs/v2021.03.17/guides/v1alpha1/monitoring/coreos). For this tutorial, we have enabled Prometheus monitoring using Prometheus operator.

- If you already do not have a grafana instance running, deploy one following tutorial from [here](https://github.com/appscode/third-party-tools/blob/master/monitoring/grafana/README.md).

## Add Prometheus Data Source

We have to add our Prometheus server `prometheus-prometheus-0` as data source of grafana. We are going to use a `ClusterIP` service to connect Prometheus server with grafana. Let's create a service to select Prometheus server `prometheus-prometheus-0`,

```bash
$ kubectl apply -f https://github.com/stashed/docs/raw/{{< param "info.version" >}}/docs/examples/monitoring/coreos/prometheus-service.yaml
service/prometheus created
```

Below the YAML for the service we have created above,

```yaml
apiVersion: v1
kind: Service
metadata:
  name: prometheus
  namespace: monitoring
spec:
  type: ClusterIP
  ports:
  - name: web
    port: 9090
    protocol: TCP
    targetPort: 9090
  selector:
    app: prometheus
```

Now, follow these steps to add the Prometheus server as data source of Grafana UI.

1. From Grafana UI, go to `Configuration` option from sidebar and click on `Data Sources`.

    <p align="center">
      <img alt="Grafana: Data Sources"  src="/docs/v2021.03.17/images/v1alpha1/monitoring/grafana/grafana-data-source-1.png" style="padding: 10px;">
    </p>

2. Then, click on `Add data source`.

    <p align="center">
      <img alt="Grafana: Add data source"  src="/docs/v2021.03.17/images/v1alpha1/monitoring/grafana/grafana-data-source-2.png" style="padding: 10px;">
    </p>

3. Now, configure `Name`, `Type` and `URL` fields as specified below and keep rest of the configuration to their default value then click `Save&Test` button.
    - *Name: Stash* (you can give any name)
    - *Type: Prometheus*
    - *URL: http://prometheus.monitoring.svc:9090*
      (url format: http://{prometheus service name}.{namespace}.svc:{port})

    <p align="center">
      <img alt="Grafana: Configure data source"  src="/docs/v2021.03.17/images/v1alpha1/monitoring/grafana/grafana-data-source-3.png" style="padding: 10px;">
    </p>

Once you have added Prometheus data source successfully, you are ready to create a dashboard to visualize the metrics.

## Import Stash Dashboard

Stash has a preconfigured dashboard created by [Alexander Trost](https://github.com/galexrt). You can import the dashboard using dashboard id `4198` or you can download json configuration of the dashboard from [here](https://grafana.com/dashboards/4198).

Follow these steps to import the preconfigured stash dashboard,

1. From Grafana UI, go to `Create` option from sidebar and click on `import`.

    <p align="center">
        <img alt="Grafana: Import dashboard"  src="/docs/v2021.03.17/images/v1alpha1/monitoring/grafana/grafana-import-1.png" style="padding: 10px;">
    </p>

2. Then, insert the dashboard id `4198` in `Grafana.com Dashboard` field and press `Load` button. You can also upload `json` configuration file of the dashboard using `Upload .json File` button.

    <p align="center">
      <img alt="Grafana: Provide dashboard ID"  src="/docs/v2021.03.17/images/v1alpha1/monitoring/grafana/grafana-import-2.png" style="padding: 10px;">
    </p>

3. Now on `prometheus-infra` field, select the data source name that we have given to our Prometheus data source earlier. Then click on `Import` button.

    <p align="center">
        <img alt="Grafana: Select data source"  src="/docs/v2021.03.17/images/v1alpha1/monitoring/grafana/grafana-import-3.png" style="padding: 10px;">
    </p>

Once you have imported the dashboard successfully, you will be greeted with Stash dashboard.

<p align="center">
      <img alt="Grafana: Stash dashboard"  src="/docs/v2021.03.17/images/v1alpha1/monitoring/grafana/grafana-stash-dashboard.png" style="padding: 10px;">
</p>

## Cleanup

To cleanup the Kubernetes resources created by this tutorial, run:

```bash
kubectl delete -n monitoring service prometheus
```

## Next Steps

- Learn how monitoring in Stash works from [here](/docs/v2021.03.17/guides/v1alpha1/monitoring/overview).
- Learn how to monitor Stash using builtin Prometheus from [here](/docs/v2021.03.17/guides/v1alpha1/monitoring/builtin).
- Learn how to monitor Stash using CoreOS Prometheus Operator from [here](/docs/v2021.03.17/guides/v1alpha1/monitoring/coreos).
