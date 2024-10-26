# Setting Up a Minikube Cluster with Helm, Prometheus, Grafana, and Thanos

## Prerequisites
- 🖥️ Minikube installed
- 🛠️ kubectl installed
- 🎛️ Helm installed

## Steps

### 1. 🚀 Start Minikube
```bash
minikube start
minikube status
```

### 2. 🔍 Verify Helm Installation
Helm should already be installed as per the prerequisites. Verify the installation:
```bash
helm version
```


### Helm Version Output
```bash
@ulutasgo ➜ /workspaces/PrometheusAsDataSource (main) $ helm version
version.BuildInfo{Version:"v3.16.1", GitCommit:"5a5449dc42be07001fd5771d56429132984ab3ab", GitTreeState:"clean", GoVersion:"go1.22.7"}
@ulutasgo ➜ /workspaces/PrometheusAsDataSource (main) $ helm list
NAME            NAMESPACE       REVISION        UPDATED                                 STATUS          CHART                   APP VERSION       
grafana         default         1               2024-10-19 11:12:37.125003763 +0000 UTC deployed        grafana-8.5.8           11.2.2-security-01
prometheus      default         1               2024-10-19 11:12:16.266772312 +0000 UTC deployed        prometheus-25.27.0      v2.54.1
```


### 3. ➕ Add Helm Repositories
```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
echo "Helm repositories added successfully!"
```
Important always use a namespace never use default
kubectl create namespace monitoring

### 4. 📈 Install Prometheus
```bash
helm install prometheus prometheus-community/prometheus -n monitoring
```

### 5. 📊 Install Grafana
```bash
helm install grafana grafana/grafana -n monitoring
```

@ulutasgo ➜ /workspaces/PrometheusAsDataSource (main) $ kubectl get pods -A
NAMESPACE     NAME                                                 READY   STATUS    RESTARTS      AGE
kube-system   coredns-6f6b679f8f-f27b6                             1/1     Running   2 (42m ago)   6d22h
kube-system   etcd-minikube                                        1/1     Running   2 (42m ago)   6d22h
kube-system   kube-apiserver-minikube                              1/1     Running   2 (42m ago)   6d22h
kube-system   kube-controller-manager-minikube                     1/1     Running   2 (42m ago)   6d22h
kube-system   kube-proxy-njsgb                                     1/1     Running   2 (42m ago)   6d22h
kube-system   kube-scheduler-minikube                              1/1     Running   2 (42m ago)   6d22h
kube-system   storage-provisioner                                  1/1     Running   5 (42m ago)   6d22h
monitoring    grafana-6f9cddc4f6-hzt5s                             0/1     Running   0             8s
monitoring    prometheus-alertmanager-0                            1/1     Running   0             21s
monitoring    prometheus-kube-state-metrics-75b5bb4bf8-jqf4l       1/1     Running   0             21s
monitoring    prometheus-prometheus-node-exporter-nqf6j            1/1     Running   0             21s
monitoring    prometheus-prometheus-pushgateway-84557d6c79-nzp4t   1/1     Running   0             21s
monitoring    prometheus-server-644d686bc6-57999                   1/2     Running   0             21s


# Port forward Grafana

# initial Port forward setup 
```bash
kubectl port-forward svc/grafana 3000:80 -n monitoring
```
- **Local Environment:** Access Grafana at `http://localhost:3000`.
- Development ports > accessed over load balancer
- Development ports > accessed over the bridge

- **CodeSpaces Environment:** Access Grafana at `https://friendly-rotary-phone-7w5g6j49r6hwr4p-3000.app.github.dev`.
- Cloud spaces environment > https://fictional-space-fiesta-675v46v6q35vpw-3000.app.github.dev/connections/datasources/edit/ce1ci1ipttkw0e


### 6. 🔄 Port Forward Prometheus
```bash
kubectl port-forward svc/prometheus-server 9090:80 -n monitoring
```
port forward prometheus 

@ulutasgo ➜ /workspaces/PrometheusAsDataSource (main) $ kubectl port-forward svc/prometheus-server 9090:80 -n monitoring
Forwarding from 127.0.0.1:9090 -> 9090
Forwarding from [::1]:9090 -> 9090


- **Local Environment:** Access Prometheus at `http://localhost:9090`.

- **CodeSpaces Environment:** Access Prometheus at `https://friendly-rotary-phone-7w5g6j49r6hwr4p-9090.app.github.dev/graph?g0.expr=&g0.tab=1&g0.display_mode=lines&g0.show_exemplars=0&g0.range_input=1h`.

- Development ports > accessed over load balancer
- Development ports > accessed over the bridge

### 7. 🌐 Access Grafana
Get the Grafana admin password:
```bash
kubectl get secret --namespace monitoring grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```
 admin is the default username
 password : 4YXwXPesjqJGZSylqzGfJOIatvabGMfFgvKOvYrb

```
Access Grafana at `http://localhost:3000` and log in with `admin` and the retrieved password.

> Use the ports in the tabs in codespaces

### 8. 🌐 Access Prometheus
Forward the Prometheus port:
```bash
kubectl port-forward svc/prometheus-server 9090:80
```
- **Local Environment:** Access Prometheus at `http://localhost:9090`.
- **CodeSpaces Environment:** Access Prometheus at `https://friendly-rotary-phone-7w5g6j49r6hwr4p-9090.app.github.dev/graph?g0.expr=&g0.tab=1&g0.display_mode=lines&g0.show_exemplars=0&g0.range_input=1h`. Check ports with 9090.

### 9. ➕ Add Prometheus as a Data Source in Grafana
1. **Local:** Open Grafana in your browser at `http://localhost:3000`.
    **CodeSpaces:** Open Grafana at `https://friendly-rotary-phone-7w5g6j49r6hwr4p-3000.app.github.dev/?orgId=1`.
2. Log in with `admin` and the retrieved password.
3. Go to **Configuration** > **Data Sources**.
4. Click **Add data source**.
5. Select **Prometheus**.
>>> Not load balancer but clusterIP address for pod to pod networking access
6A. Set the URL to `https://friendly-rotary-phone-7w5g6j49r6hwr4p-9090.app.github.dev`.
6B. Set the URL to `http://clusterip:9090`.
7. Click **Save & Test** to verify the connection.

### 10. ✅ Verify Installations
Check the status of the pods:
```bash
kubectl get pods
kubectl get -n monitoring
```