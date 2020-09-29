#### Installation Prometheus and Grafana on Kubernetes Cluster

```
root@ubuntu16d-k8s1:~/helm-prometheus# helm version
version.BuildInfo{Version:"v3.3.4", GitCommit:"a61ce5633af99708171414353ed49547cf05013d", GitTreeState:"clean", GoVersion:"go1.14.9"}
root@ubuntu16d-k8s1:~/helm-prometheus#

root@ubuntu16d-k8s1:~/helm-prometheus# kubectl get pods -A
NAMESPACE     NAME                                     READY   STATUS    RESTARTS   AGE
kube-system   coredns-66bff467f8-h6zkp                 1/1     Running   6          6d1h
kube-system   coredns-66bff467f8-mffff                 1/1     Running   6          6d1h
kube-system   etcd-ubuntu16d-k8s1                      1/1     Running   8          6d1h
kube-system   kube-apiserver-ubuntu16d-k8s1            1/1     Running   10         6d1h
kube-system   kube-controller-manager-ubuntu16d-k8s1   1/1     Running   11         6d1h
kube-system   kube-flannel-ds-xlfc2                    1/1     Running   8          6d1h
kube-system   kube-proxy-f4j6t                         1/1     Running   6          6d1h
kube-system   kube-scheduler-ubuntu16d-k8s1            1/1     Running   8          6d1h
root@ubuntu16d-k8s1:~/helm-prometheus#

root@ubuntu16d-k8s1:~/helm-prometheus# helm repo add stable https://kubernetes-charts.storage.googleapis.com
"stable" has been added to your repositories
root@ubuntu16d-k8s1:~/helm-prometheus#

root@ubuntu16d-k8s1:~/helm-prometheus# helm repo update
Hang tight while we grab the latest from your chart repositories...
...Successfully got an update from the "stable" chart repository
Update Complete. ⎈Happy Helming!⎈
root@ubuntu16d-k8s1:~/helm-prometheus#

root@ubuntu16d-k8s1:~/helm-prometheus# helm repo list
NAME  	URL
stable	https://kubernetes-charts.storage.googleapis.com
root@ubuntu16d-k8s1:~/helm-prometheus#

root@ubuntu16d-k8s1:~/helm-prometheus# helm install --namespace monitoring prometheus-demo stable/prometheus-operator
WARNING: This chart is deprecated
manifest_sorter.go:192: info: skipping unknown hook: "crd-install"
manifest_sorter.go:192: info: skipping unknown hook: "crd-install"
manifest_sorter.go:192: info: skipping unknown hook: "crd-install"
manifest_sorter.go:192: info: skipping unknown hook: "crd-install"
manifest_sorter.go:192: info: skipping unknown hook: "crd-install"
manifest_sorter.go:192: info: skipping unknown hook: "crd-install"
NAME: prometheus-demo
LAST DEPLOYED: Tue Sep 29 18:28:42 2020
NAMESPACE: monitoring
STATUS: deployed
REVISION: 1
NOTES:
*******************
*** DEPRECATED ****
*******************
* stable/prometheus-operator chart is deprecated.
* Further development has moved to https://github.com/prometheus-community/helm-charts
* The chart has been renamed kube-prometheus-stack to more clearly reflect
* that it installs the `kube-prometheus` project stack, within which Prometheus
* Operator is only one component.

The Prometheus Operator has been installed. Check its status by running:
  kubectl --namespace monitoring get pods -l "release=prometheus-demo"

Visit https://github.com/coreos/prometheus-operator for instructions on how
to create & configure Alertmanager and Prometheus instances using the Operator.
root@ubuntu16d-k8s1:~/helm-prometheus#

root@ubuntu16d-k8s1:~/helm-prometheus# helm ls -n monitoring
NAME           	NAMESPACE 	REVISION	UPDATED                               	STATUS  	CHART                    	APP VERSION
prometheus-demo	monitoring	1       	2020-09-29 18:28:42.70546611 +0530 IST	deployed	prometheus-operator-9.3.2	0.38.1
root@ubuntu16d-k8s1:~/helm-prometheus#


root@ubuntu16d-k8s1:~/helm-prometheus# kubectl --namespace monitoring get pods -l "release=prometheus-demo"
NAME                                                   READY   STATUS    RESTARTS   AGE
prometheus-demo-prometheus-node-exporter-gddqg         1/1     Running   0          43m
prometheus-demo-prometheus-operator-7cb95bf668-4vtv2   2/2     Running   0          43m
root@ubuntu16d-k8s1:~/helm-prometheus#

root@ubuntu16d-k8s1:~/helm-prometheus# kubectl -n monitoring get pods
NAME                                                     READY   STATUS    RESTARTS   AGE
alertmanager-prometheus-demo-prometheus-alertmanager-0   2/2     Running   0          43m
prometheus-demo-grafana-7c789f67c-9fpg4                  2/2     Running   0          43m
prometheus-demo-kube-state-metrics-54fc59465b-8qrj6      1/1     Running   0          43m
prometheus-demo-prometheus-node-exporter-gddqg           1/1     Running   0          43m
prometheus-demo-prometheus-operator-7cb95bf668-4vtv2     2/2     Running   0          43m
prometheus-prometheus-demo-prometheus-prometheus-0       3/3     Running   0          43m
root@ubuntu16d-k8s1:~/helm-prometheus#

root@ubuntu16d-k8s1:~/helm-prometheus# kubectl get svc -A
NAMESPACE     NAME                                                 TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                        AGE
default       kubernetes                                           ClusterIP   10.96.0.1        <none>        443/TCP                        6d2h
kube-system   kube-dns                                             ClusterIP   10.96.0.10       <none>        53/UDP,53/TCP,9153/TCP         6d2h
kube-system   littering-squid-prometheus-kubelet                   ClusterIP   None             <none>        10250/TCP,10255/TCP,4194/TCP   19h
kube-system   prometheus-demo-prometheus-coredns                   ClusterIP   None             <none>        9153/TCP                       56m
kube-system   prometheus-demo-prometheus-kube-controller-manager   ClusterIP   None             <none>        10252/TCP                      56m
kube-system   prometheus-demo-prometheus-kube-etcd                 ClusterIP   None             <none>        2379/TCP                       56m
kube-system   prometheus-demo-prometheus-kube-proxy                ClusterIP   None             <none>        10249/TCP                      56m
kube-system   prometheus-demo-prometheus-kube-scheduler            ClusterIP   None             <none>        10251/TCP                      56m
kube-system   prometheus-demo-prometheus-kubelet                   ClusterIP   None             <none>        10250/TCP,10255/TCP,4194/TCP   55m
monitoring    alertmanager-operated                                ClusterIP   None             <none>        9093/TCP,9094/TCP,9094/UDP     55m
monitoring    prometheus-demo-grafana                              ClusterIP   10.101.243.18    <none>        80/TCP                         56m
monitoring    prometheus-demo-kube-state-metrics                   ClusterIP   10.103.189.66    <none>        8080/TCP                       56m
monitoring    prometheus-demo-prometheus-alertmanager              ClusterIP   10.109.81.133    <none>        9093/TCP                       56m
monitoring    prometheus-demo-prometheus-node-exporter             ClusterIP   10.96.174.34     <none>        9100/TCP                       56m
monitoring    prometheus-demo-prometheus-operator                  ClusterIP   10.100.227.18    <none>        8080/TCP,443/TCP               56m
monitoring    prometheus-demo-prometheus-prometheus                ClusterIP   10.100.195.241   <none>        9090/TCP                       56m
monitoring    prometheus-operated                                  ClusterIP   None             <none>        9090/TCP                       55m
root@ubuntu16d-k8s1:~/helm-prometheus#

root@ubuntu16d-k8s1:~/helm-prometheus# kubectl port-forward --address 0.0.0.0 -n monitoring prometheus-prometheus-demo-prometheus-prometheus-0 9090  >/dev/null 2>&1 &
[1] 27053
root@ubuntu16d-k8s1:~/helm-prometheus#

root@ubuntu16d-k8s1:~/helm-prometheus# kubectl port-forward --address 0.0.0.0 -n monitoring alertmanager-prometheus-demo-prometheus-alertmanager-0 9093  >/dev/null 2>&1 &
[2] 27867
root@ubuntu16d-k8s1:~/helm-prometheus#

root@ubuntu16d-k8s1:~/helm-prometheus# kubectl port-forward --address 0.0.0.0 -n monitoring prometheus-demo-grafana-7c789f67c-9fpg4 3000  >/dev/null 2>&1 &
[3] 847
root@ubuntu16d-k8s1:~/helm-prometheus#


```

Ref: https://rancher.com/blog/2020/custom-monitoring
