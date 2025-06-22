# linkerd-monitoring

linkerd-monitoring is an umbrella Helm providing a comprehensive solution for
deploying a best-practices observability stack powered by Linkerd. It supports
the optional installation of three components: kube-prometheus-stack, kube-prometheus-stack-linkerd and linkerd-dashboard.

## Installation

To install the linkerd-monitoring chart, ensure you have already [installed the Linkerd control plane](https://linkerd.io/2/tasks/install-helm/). Once the control plane is set up, you can install linkerd-monitoring using the following command:

```
helm install -n monitoring --create-namespace monitoring oci://ghcr.io/alpeb/linkerd-monitoring
```

## Components

### kube-prometheus-stack

The kube-prometheus-stack Helm chart, maintained by the Prometheus community,
provides a robust observability stack based on Prometheus, tailored for
Kubernetes environments.

The chart's values are accessible under the `kube-prometheus-stack` entry.

To enable this component set `kube-prometheus-stack: true`.

From the chart's description page:

> Installs core components of the [kube-prometheus](https://github.com/prometheus-operator/kube-prometheus) stack, a collection of Kubernetes manifests, [Grafana](https://grafana.com/) dashboards, and [Prometheus rules](https://prometheus.io/docs/prometheus/latest/configuration/recording_rules/) combined with documentation and scripts to provide easy to operate end-to-end Kubernetes cluster monitoring with [Prometheus](https://prometheus.io/) using the [Prometheus Operator](https://github.com/prometheus-operator/prometheus-operator).

Besides installing the Prometheus Operator, the chart also installs as dependencies:

- [kube-state-metrics](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-state-metrics): Exports Kubernetes resource metrics.
- [prometheus-node-exporter](https://github.com/prometheus-community/helm-charts/tree/main/charts/prometheus-node-exporter): Exportes hardware and OS metrics.
- [Grafana](https://github.com/grafana/helm-charts/tree/main/charts/grafana):
  Provides visualization for metrics.

Refer to each project's documentation for detailed configuration options.

### kube-prometheus-stack-linkerd

This component enhances kube-prometheus-stack by adding Linkerd-specific
configurations, including `PodMonitor` and `PrometheusRule` CRD instances for
Prometheus scraping and alerting rules, as well as Grafana dashboard definitions
via ConfigMaps.

The chart's values are accessible under the `kube-prometheus-stack-linkerd` entry.

To enable this component, set `kube-prometheus-stack-linkerd.enabled: true`.

### linkerd-dashboard

The Linkerd dashboard provides a user-friendly interface for monitoring and
managing your Linkerd service mesh within a cluster. By default, it's configured
to source its data from the Prometheus instance configured in the
kube-prometheus-stack chart above.

The chart's values are accessible under the `linkerd-dashboard` entry.

To enable this component set `linkerd-dashboard.enabled: true`.

Configuration options are available under the `linkerd-dashboard` values in the
Helm chart. For details, visit the chart's page on
[ArtifactHub](https://artifacthub.io/packages/helm/linkerd-dashboard/linkerd-dashboard)
or refer to the [Buoyant
documentation](https://docs.buoyant.io/linkerd-dashboard/getting-started/).

## Accessing the Dashboards

## Grafana

By default Grafana exposes a service that we can port-forward into:

```
kubectl -n monitoring port-forward svc/monitoring-grafana 8080:80
```

## linkerd-dashboard

Linkerd-dashboard doesn't expose a service, but you can port-forward directly
into the workload:

```
kubectl -n monitoring port-forward deploy/linkerd-dashboard 8080
```
