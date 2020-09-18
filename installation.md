#### Below Process is to Install Helm on Existing kubernetes cluster

````
root@ubuntu16Desktop:~# kubectl cluster-info
Kubernetes master is running at https://192.168.11.50:6443
KubeDNS is running at https://192.168.11.50:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
root@ubuntu16Desktop:~#


root@ubuntu16Desktop:~# kubectl config get-contexts
CURRENT   NAME                          CLUSTER      AUTHINFO           NAMESPACE
*         kubernetes-admin@kubernetes   kubernetes   kubernetes-admin
root@ubuntu16Desktop:~#

root@ubuntu16Desktop:~#mkdir helm

root@ubuntu16Desktop:~#cd helm

root@ubuntu16Desktop:~/helm# curl https://raw.githubusercontent.com/kubernetes/helm/master/scripts/get > install-helm.sh
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  7160  100  7160    0     0   8113      0 --:--:-- --:--:-- --:--:--  8108
root@ubuntu16Desktop:~/helm#
root@ubuntu16Desktop:~/helm# ls
install-helm.sh
root@ubuntu16Desktop:~/helm#

root@ubuntu16Desktop:~/helm# chmod u+x install-helm.sh

root@ubuntu16Desktop:~/helm# ./install-helm.sh
Downloading https://get.helm.sh/helm-v2.16.11-linux-amd64.tar.gz
Preparing to install helm and tiller into /usr/local/bin
helm installed into /usr/local/bin/helm
tiller installed into /usr/local/bin/tiller
Run 'helm init' to configure helm.
root@ubuntu16Desktop:~/helm#
root@ubuntu16Desktop:~/helm#
root@ubuntu16Desktop:~/helm# kubectl -n kube-system create serviceaccount tiller
serviceaccount/tiller created
root@ubuntu16Desktop:~/helm#
root@ubuntu16Desktop:~/helm# kubectl create clusterrolebinding tiller --clusterrole cluster-admin --serviceaccount=kube-system:tiller
clusterrolebinding.rbac.authorization.k8s.io/tiller created
root@ubuntu16Desktop:~/helm#
root@ubuntu16Desktop:~/helm#
root@ubuntu16Desktop:~/helm# helm init --service-account tiller
Creating /root/.helm
Creating /root/.helm/repository
Creating /root/.helm/repository/cache
Creating /root/.helm/repository/local
Creating /root/.helm/plugins
Creating /root/.helm/starters
Creating /root/.helm/cache/archive
Creating /root/.helm/repository/repositories.yaml
Adding stable repo with URL: https://kubernetes-charts.storage.googleapis.com
Adding local repo with URL: http://127.0.0.1:8879/charts
$HELM_HOME has been configured at /root/.helm.

Tiller (the Helm server-side component) has been installed into your Kubernetes Cluster.

Please note: by default, Tiller is deployed with an insecure 'allow unauthenticated users' policy.
To prevent this, run `helm init` with the --tiller-tls-verify flag.
For more information on securing your installation see: https://v2.helm.sh/docs/securing_installation/
root@ubuntu16Desktop:~/helm#


root@ubuntu16Desktop:~/helm# kubectl get pods --namespace kube-system | grep tiller
tiller-deploy-569797587b-d8tgg            0/1     ContainerCreating   0          30s
root@ubuntu16Desktop:~/helm#

root@ubuntu16Desktop:~/helm# kubectl get pods --namespace kube-system | grep tiller
tiller-deploy-569797587b-d8tgg            0/1     ContainerCreating   0          38s
root@ubuntu16Desktop:~/helm#

root@ubuntu16Desktop:~/helm# kubectl get pods --namespace kube-system | grep tiller
tiller-deploy-569797587b-d8tgg            1/1     Running   0          104s
root@ubuntu16Desktop:~/helm#


root@ubuntu16Desktop:~# helm list
root@ubuntu16Desktop:~#


root@ubuntu16Desktop:~# helm install stable/kubernetes-dashboard --name dashboard-demo
WARNING: This chart is deprecated
NAME:   dashboard-demo
LAST DEPLOYED: Fri Sep 18 22:03:33 2020
NAMESPACE: default
STATUS: DEPLOYED

RESOURCES:
==> v1/Deployment
NAME                                 READY  UP-TO-DATE  AVAILABLE  AGE
dashboard-demo-kubernetes-dashboard  0/1    1           0          0s

==> v1/Pod(related)
NAME                                                  READY  STATUS             RESTARTS  AGE
dashboard-demo-kubernetes-dashboard-588cbd9ffc-5txsp  0/1    ContainerCreating  0         1s

==> v1/Role
NAME                                 CREATED AT
dashboard-demo-kubernetes-dashboard  2020-09-18T16:33:34Z

==> v1/RoleBinding
NAME                                 ROLE                                      AGE
dashboard-demo-kubernetes-dashboard  Role/dashboard-demo-kubernetes-dashboard  0s

==> v1/Secret
NAME                                 TYPE    DATA  AGE
dashboard-demo-kubernetes-dashboard  Opaque  0     0s

==> v1/Service
NAME                                 TYPE       CLUSTER-IP   EXTERNAL-IP  PORT(S)  AGE
dashboard-demo-kubernetes-dashboard  ClusterIP  10.99.5.170  <none>       443/TCP  0s

==> v1/ServiceAccount
NAME                                 SECRETS  AGE
dashboard-demo-kubernetes-dashboard  1        0s


NOTES:
*********************************************************************************
*** PLEASE BE PATIENT: kubernetes-dashboard may take a few minutes to install ***
*********************************************************************************

Get the Kubernetes Dashboard URL by running:
  export POD_NAME=$(kubectl get pods -n default -l "app=kubernetes-dashboard,release=dashboard-demo" -o jsonpath="{.items[0].metadata.name}")
  echo https://127.0.0.1:8443/
  kubectl -n default port-forward $POD_NAME 8443:8443

root@ubuntu16Desktop:~#


root@ubuntu16Desktop:~# helm list
NAME          	REVISION	UPDATED                 	STATUS  	CHART                      	APP VERSION	NAMESPACE
dashboard-demo	1       	Fri Sep 18 22:03:33 2020	DEPLOYED	kubernetes-dashboard-1.11.1	1.10.1     	default
root@ubuntu16Desktop:~#


root@ubuntu16Desktop:~# helm delete dashboard-demo
release "dashboard-demo" deleted
root@ubuntu16Desktop:~#



```
