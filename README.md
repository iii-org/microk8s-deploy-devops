# microk8s-deploy-devops

#### 建立k8s群集
* `sudo snap install microk8s --channel=1.19/stable --classic`
#### 選擇啟動所需的k8s功能
* `microk8s enable dns ingress rbac helm3 metrics-server storage`
#### 安裝rancher準備
參考來源: [install-rancher-on-k8s](https://rancher.com/docs/rancher/v2.x/en/installation/install-rancher-on-k8s/)
* `microk8s.helm3 repo add rancher-stable https://releases.rancher.com/server-charts/stable`
* `microk8s.kubectl create namespace cattle-system`
* `kubectl apply --validate=false -f https://github.com/jetstack/cert-manager/releases/download/v1.0.4/cert-manager.crds.yaml`
* `kubectl create namespace cert-manager`
* `helm repo add jetstack https://charts.jetstack.io`
* `helm repo update`
* `helm install cert-manager jetstack/cert-manager --namespace cert-manager --version v1.0.4`
* `microk8s.helm3 install rancher rancher-stable/rancher --namespace cattle-system --set hostname=rancher.10.20.0.67.xip.io --version 2.4.5`

