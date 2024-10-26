##

@ulutasgo ➜ /workspaces/PrometheusAsDataSource (main) $ kubectl get pods -n monitoring
NAME                                                 READY   STATUS    RESTARTS   AGE
grafana-6f9cddc4f6-hzt5s                             1/1     Running   0          8m3s
prometheus-alertmanager-0                            1/1     Running   0          8m16s
prometheus-kube-state-metrics-75b5bb4bf8-jqf4l       1/1     Running   0          8m16s
prometheus-prometheus-node-exporter-nqf6j            1/1     Running   0          8m16s
prometheus-prometheus-pushgateway-84557d6c79-nzp4t   1/1     Running   0          8m16s
prometheus-server-644d686bc6-57999                   2/2     Running   0          8m16s