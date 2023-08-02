---
title: Stash Grafana Dashboard | Stash
description: Using Stash Grafana Dashboard
menu:
  docs_v2023.01.05:
    identifier: monitoring-grafana-dashboard
    name: Grafana Dashboard
    parent: monitoring
    weight: 30
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

{{< notice type="warning" message="This is an Enterprise-only feature. You must be **Stash Enterprise** customer to use pre-built Stash Grafana dashboard." >}}

# Stash Grafana Dashboard

Grafana provides an elegant graphical user interface to visualize data. You can create a beautiful dashboard easily with a meaningful representation of your Prometheus metrics.

We provide a pre-built Grafana dashboard to our **Stash Enterprise** users. In this guide, we are going to show you how you can import this dashboard from your Grafana UI.

>Some basic metrics are also available for Stash Community Edition. You can create your dashboard using those metrics.

## Before You Begin

- At first, you need to setup a Prometheus monitoring stack in your cluster. Please, follow the [Prometheus Operator](/docs/v2023.01.05/guides/monitoring/prom-operator/) guide to setup your monitoring stack if you haven't done already.
- Then, install Stash Enterprise edition with monitoring enabled. Please, follow [this guide](/docs/v2023.01.05/guides/monitoring/prom-operator/#enable-monitoring-in-stash) if you haven't done already.
- You must have `stash_dashboard.json` file. Please, contact us to get the dashboard JSON file.

## Install Panopticon

Stash Grafana dashboard depends on our another product called [Panopticon](https://blog.byte.builders/post/introducing-panopticon/). It is a Kubernetes state metric exporter similar to [kube-state-metrics](https://github.com/kubernetes/kube-state-metrics) but generic for all Kubernetes resources including CRDs. In this section, we are going to show the installation procedure for Panopticon.

**Issue License:**

Like other AppsCode products, [Panopticon](https://blog.byte.builders/post/introducing-panopticon/) also need a license to run. You can grab a 30 days trial license for Panopticon from [here](https://license-issuer.appscode.com/?p=panopticon-enterprise).

>**If you already have an enterprise license for KubeDB or Stash, you do not need to issue a new license for Panopticon. Your existing KubeDB or Stash license will work with Panopticon.**

**Install Panopticon:**

Now, install Panopticon using the following commands:

```bash
$ helm repo add appscode https://charts.appscode.com/stable/
$ helm repo update

$ helm install panopticon appscode/panopticon -n kubeops \
    --create-namespace \
    --set monitoring.enabled=true \
    --set monitoring.agent=prometheus.io/operator \
    --set monitoring.serviceMonitor.labels.release=prometheus-stack \
    --set-file license=/path/to/license-file.txt
```

Make sure to use the appropriate label in `monitoring.serviceMonitor.labels` field according to your setup. This label is used by the Prometheus server to select the desired ServiceMonitor.

## Import Stash Garafana Dashboard

At first, let's port-forward the respective service for the Grafana dashboard so that we can access it through our browser locally.

```bash
❯ kubectl port-forward -n monitoring service/prometheus-stack-grafana 3000:80
Forwarding from 127.0.0.1:3000 -> 3000
Forwarding from [::1]:3000 -> 3000
```

Now, go to [http://localhost:3000](http://localhost:3000/) in your browser and login to your Grafana UI. If you followed the [Prometheus Operator](/docs/v2023.01.05/guides/monitoring/prom-operator/) guide to deploy your Prometheus stack, then the default username and password should be `admin`, and `prom-operator` respectively.

Then, on the Grafana UI, click the `+` icon from the left sidebar and then click on `Import` button as below,

<figure align="center">
  <img alt="Import Stash Grafana Dashboard: Step 1" src="/docs/v2023.01.05/guides/monitoring/grafana/images/import_dashboard_1.png">
<figcaption align="center">Fig: Import Stash Grafana Dashboard (Step 1)</figcaption>
</figure>

Then, on the import UI, you can either upload the `stash_dashboard.json` file by clicking the `Upload JSON file` button or you can paste the content of the JSON file in the text area labeled as `Import via panel json`.

<figure align="center">
  <img alt="Import Stash Grafana Dashboard: Step 2" src="/docs/v2023.01.05/guides/monitoring/grafana/images/import_dashboard_2.png">
<figcaption align="center">Fig: Import Stash Grafana Dashboard (Step 2)</figcaption>
</figure>

Then, on the next step click the `Import` button as below.

<figure align="center">
  <img alt="Import Stash Grafana Dashboard: Step 3" src="/docs/v2023.01.05/guides/monitoring/grafana/images/import_dashboard_3.png">
<figcaption align="center">Fig: Import Stash Grafana Dashboard (Step 3)</figcaption>
</figure>

Once, you have successfully imported the dashboard, you should see the Stash dashboard similar to this.

<figure align="center">
  <img alt="Stash Grafana Dashboard" src="/docs/v2023.01.05/guides/monitoring/grafana/images/stash_grafana_dashboard.png">
<figcaption align="center">Fig: Stash Grafana Dashboard</figcaption>
</figure>

>If your cluster does not have any backup configured, you may see the dashboard panels are empty. Nothing to worry about here. Just, run some backups and your dashboard should be populated automatically.

## Dashboard Tour

The following video gives a tour of the different components of the Stash Grafana dashboard.

{{< youtube b5r9PDwl--U >}}