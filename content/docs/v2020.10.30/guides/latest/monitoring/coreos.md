---
title: CoreOS Prometheus Operator | Stash
description: Monitor Stash using Prometheus operator
menu:
  docs_v2020.10.30:
    identifier: monitoring-coreos-operator
    name: Prometheus Operator
    parent: monitoring
    weight: 30
product_name: stash
menu_name: docs_v2020.10.30
section_menu_id: guides
info:
  catalog: v2020.10.30
  cli: v0.11.5
  community: v0.11.5
  elasticsearch:
  - 5.6.4-v3
  - 6.2.4-v3
  - 6.3.0-v3
  - 6.4.0-v3
  - 6.5.3-v3
  - 6.8.0-v3
  - 7.2.0-v3
  - 7.3.2-v3
  enterprise: v0.11.5
  installer: v0.11.5
  mongodb:
  - 3.4.17-v3
  - 3.4.22-v3
  - 3.6.13-v3
  - 3.6.8-v3
  - 4.0.11-v3
  - 4.0.3-v3
  - 4.0.5-v3
  - 4.1.13-v3
  - 4.1.4-v3
  - 4.1.7-v3
  - 4.2.3-v3
  mysql:
  - 5.7.25-v3
  - 8.0.14-v3
  - 8.0.3-v3
  percona-xtradb:
  - 5.7-v3
  postgres:
  - 10.14.0-v1
  - 11.9.0-v1
  - 12.4.0-v1
  - 9.6.19-v1
  version: v2020.10.30
---

# Monitoring Using CoreOS Prometheus Operator

CoreOS [prometheus-operator](https://github.com/coreos/prometheus-operator) provides simple and Kubernetes native way to deploy and configure Prometheus server. This tutorial will show you how to use Prometheus operator for monitoring Stash.

## Before You Begin

- At first, you need to have a Kubernetes cluster, and the kubectl command-line tool must be configured to communicate with your cluster. If you do not already have a cluster, you can create one by using [kind](https://kind.sigs.k8s.io/docs/user/quick-start/).

- To keep Prometheus resources isolated, we are going to use a separate namespace to deploy Prometheus operator and respective resources.

  ```bash
  $ kubectl create ns monitoring
  namespace/monitoring created
  ```

- We need a CoreOS prometheus-operator instance running. If you already don't have a running instance, deploy one following the docs from [here](https://github.com/appscode/third-party-tools/blob/master/monitoring/prometheus/coreos-operator/README.md).

## Enable Monitoring in Stash

Enable Prometheus monitoring using `prometheus.io/operator` agent while installing Stash. To know details about how to enable monitoring see [here](/docs/v2020.10.30/guides/latest/monitoring/overview#how-to-enable-monitoring).

Here, we are going to enable monitoring for both `backup`, `restore` and `operator` metrics using Helm 3.

```bash
$ helm install stash-operator appscode/stash --version {{< param "info.version" >}} \
  --namespace kube-system \
  --set monitoring.agent=prometheus.io/operator \
  --set monitoring.backup=true \
  --set monitoring.operator=true \
  --set monitoring.prometheus.namespace=monitoring \
  --set monitoring.serviceMonitor.labels.k8s-app=prometheus
```

This will create a `ServiceMonitor` crd with name `stash-servicemonitor` in monitoring namespace for monitoring endpoints of `stash-operator` service. This ServiceMonitor will have label `k8s-app: prometheus` provided by `--servicemonitor-label` flag. This label will be used by Prometheus crd to select this ServiceMonitor.

Let's check the ServiceMonitor crd using following command,

```yaml
$ kubectl get servicemonitor stash-servicemonitor -n monitoring -o yaml
apiVersion: monitoring.coreos.com/v1
kind: ServiceMonitor
metadata:
  annotations:
    kubectl.kubernetes.io/last-applied-configuration: |
      {"apiVersion":"monitoring.coreos.com/v1","kind":"ServiceMonitor","metadata":{"annotations":{},"labels":{"k8s-app":"prometheus"},"name":"stash-servicemonitor","namespace":"monitoring"},"spec":{"endpoints":[{"honorLabels":true,"port":"pushgateway"},{"bearerTokenFile":"/var/run/secrets/kubernetes.io/serviceaccount/token","port":"api","scheme":"https","tlsConfig":{"caFile":"/etc/prometheus/secrets/stash-apiserver-cert/tls.crt","serverName":"stash-operator.kube-system.svc"}}],"namespaceSelector":{"matchNames":["kube-system"]},"selector":{"matchLabels":{"app":"stash"}}}}
  creationTimestamp: 2018-11-21T09:35:37Z
  generation: 1
  labels:
    k8s-app: prometheus
  name: stash-servicemonitor
  namespace: monitoring
  resourceVersion: "6126"
  selfLink: /apis/monitoring.coreos.com/v1/namespaces/monitoring/servicemonitors/stash-servicemonitor
  uid: cd6cca14-ed70-11e8-8838-0800272dd258
spec:
  endpoints:
  - honorLabels: true
    port: pushgateway
  - bearerTokenFile: /var/run/secrets/kubernetes.io/serviceaccount/token
    port: api
    scheme: https
    tlsConfig:
      caFile: /etc/prometheus/secrets/stash-apiserver-cert/tls.crt
      serverName: stash-operator.kube-system.svc
  namespaceSelector:
    matchNames:
    - kube-system
  selector:
    matchLabels:
      app: stash
```

Here, we have two endpoints at `spec.endpoints` field. One is `pushgateway` that exports backup and recovery metrics and another is `api` which exports operator metrics.

Stash exports operator metrics via TLS secured `api` endpoint. So, Prometheus server need to provide certificate while scraping metrics from this endpoint. Stash has created a secret named `stash-apiserver-certs` with this certificate in `monitoring` namespace as we have specified that we are going to deploy Prometheus in that namespace through `--prometheus-namespace` flag. We have to specify this secret in Prometheus crd through `spec.secrets` field. Prometheus operator will mount this secret at `/etc/prometheus/secrets/stash-apiserver-cert` directory of respective Prometheus pod. So, we need to configure `tlsConfig` field to use that certificate. Here, `caFile` indicates the certificate to use and `serverName` is used to verify hostname. In our case, the certificate is valid for hostname `server` and `stash-operator.kube-system.svc`.

Let's check secret `stash-apiserver-cert` has been created in monitoring namespace.

```bash
$ kubectl get secret -n monitoring -l=app=stash
NAME                   TYPE                DATA   AGE
stash-apiserver-cert   kubernetes.io/tls   2      31m
```

Also note that, there is a `bearerTokenFile` field. This file is token for the serviceaccount that will be created while creating RBAC stuff for Prometheus crd. This is required for authorizing Prometheus to scrape Stash API server.

Now, we are ready to deploy Prometheus server.

## Deploy Prometheus Server

In order to deploy Prometheus server, we have to create `Prometheus` crd. Prometheus crd defines a desired Prometheus server setup. For more details about `Prometheus` crd, please visit [here](https://github.com/coreos/prometheus-operator/blob/master/Documentation/design.md#prometheus).

If you are using a RBAC enabled cluster, you have to give necessary permissions to Prometheus. Check the documentation to see required RBAC permission from [here](https://github.com/appscode/third-party-tools/blob/master/monitoring/prometheus/coreos-operator/README.md#deploy-prometheus-server).

**Create Prometheus:**

Below is the YAML of `Prometheus` crd that we are going to create for this tutorial,

```yaml
apiVersion: monitoring.coreos.com/v1
kind: Prometheus
metadata:
  name: prometheus
  namespace: monitoring
  labels:
    k8s-app: prometheus
spec:
  replicas: 1
  serviceAccountName: prometheus
  serviceMonitorSelector:
    matchLabels:
      k8s-app: prometheus
  secrets:
  - stash-apiserver-cert
  resources:
    requests:
      memory: 400Mi
```

Here, `spec.serviceMonitorSelector` is used to select the `ServiceMonitor` crd that is created by Stash. We have provided `stash-apiserver-cert` secret in `spec.secrets` field. This will be mounted in Prometheus pod.

Let's create the `Prometheus` object we have shown above,

```bash
$ kubectl apply -f https://github.com/stashed/docs/raw/{{< param "info.version" >}}/docs/examples/guides/latest/monitoring/coreos/prometheus.yaml
prometheus.monitoring.coreos.com/prometheus created
```

Prometheus operator watches for `Prometheus` crd. Once a `Prometheus` crd is created, Prometheus operator generates respective configuration and creates a StatefulSet to run Prometheus server.

Let's check StatefulSet has been created,

```bash
$ kubectl get statefulset -n monitoring
NAME                    DESIRED   CURRENT   AGE
prometheus-prometheus   1         1         4m
```

Check StatefulSet's pod is running,

```bash
$ kubectl get pod prometheus-prometheus-0 -n monitoring
NAME                      READY   STATUS    RESTARTS   AGE
prometheus-prometheus-0   2/2     Running   0          6m
```

Now, we are ready to access Prometheus dashboard.

### Verify Monitoring Metrics

Prometheus server is running on port `9090`. We are going to use [port forwarding](https://kubernetes.io/docs/tasks/access-application-cluster/port-forward-access-application-cluster/) to access Prometheus dashboard. Run following command on a separate terminal,

```bash
$ kubectl port-forward -n monitoring prometheus-prometheus-0 9090
Forwarding from 127.0.0.1:9090 -> 9090
Forwarding from [::1]:9090 -> 9090
```

Now, we can access the dashboard at `localhost:9090`. Open [http://localhost:9090](http://localhost:9090) in your browser. You should see `pushgateway` and `api` endpoints of `stash-operator` service as target.

<figure align="center">
  <img alt="Stash Monitoring Flow" src="/docs/v2020.10.30/images/guides/latest/monitoring/prom-coreos-target.png">
<figcaption align="center">Fig: Prometheus dashboard</figcaption>
</figure>

## Cleanup

To cleanup the Kubernetes resources created by this tutorial, run:

```bash
# cleanup Prometheus resources
kubectl delete -n monitoring prometheus prometheus
kubectl delete -n monitoring secret stash-apiserver-cert

# delete namespace
kubectl delete ns monitoring
```

To uninstall Stash follow this [guide](/docs/v2020.10.30/setup/README).

## Next Steps

- Learn how monitoring in Stash works from [here](/docs/v2020.10.30/guides/latest/monitoring/overview).
- Learn how to monitor Stash using builtin Prometheus from [here](/docs/v2020.10.30/guides/latest/monitoring/builtin).
