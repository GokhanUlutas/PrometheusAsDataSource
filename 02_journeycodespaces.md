


## prometheus & grafana pods installed with Helm

@ulutasgo ➜ /workspaces/PrometheusAsDataSource (main) $ kubectl get pods
NAME                                                 READY   STATUS    RESTARTS   AGE
grafana-9d57ccbfd-jkpz5                              1/1     Running   0          17m
prometheus-alertmanager-0                            1/1     Running   0          18m
prometheus-kube-state-metrics-75b5bb4bf8-5dp4z       1/1     Running   0          18m
prometheus-prometheus-node-exporter-ljjkr            1/1     Running   0          18m
prometheus-prometheus-pushgateway-84557d6c79-fkqdk   1/1     Running   0          18m
prometheus-server-644d686bc6-rncdx                   2/2     Running   0          18m