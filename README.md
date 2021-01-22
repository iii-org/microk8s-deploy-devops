# microk8s-deploy-devops

以下環境採用`10.20.0.68` 使用者`localadmin`

### 建立k8s群集(用非HA的版本)
* `sudo snap install microk8s --channel=1.18/stable --classic`
* `sudo usermod -a -G microk8s localadmin`
* `sudo chown -f -R localadmin ~/.kube`
選擇啟動所需的k8s功能(因為local儲存空間並非建議因此不啟用)
* `microk8s enable dns ingress rbac helm3 metrics-server`
#### (選擇性)安裝NFS Server(在本專案非必要, 部屬可採用其他公有雲或是私有雲的空間均兼容(storageclass), 可於rancher畫面上整合其他公私有雲空間)
* `sudo apt install nfs-kernel-server -y`
* `sudo mkdir /var/iiidevopsNFS`
* `sudo chmod -R 777 /var/iiidevopsNFS`
* 修改`/etc/exports`添加`/var/iiidevopsNFS *(no_root_squash,rw,sync,no_subtree_check)`
* `sudo systemctl restart nfs-kernel-server`
* `showmount -e 10.20.0.68`
* 

### 匯入套件清單
* cert-manager: `microk8s.helm3 repo add jetstack https://charts.jetstack.io`
* rancher: `microk8s.helm3 repo add rancher-stable https://releases.rancher.com/server-charts/stable`

#### 安裝cert-manager(全NameSpace)
* `microk8s.kubectl apply --validate=false -f https://github.com/jetstack/cert-manager/releases/download/v1.0.4/cert-manager.crds.yaml`
* `microk8s.kubectl create namespace cert-manager`
* `microk8s.helm3 install cert-manager jetstack/cert-manager --namespace cert-manager --version v1.0.4`

#### 安裝rancher準備
參考來源: [install-rancher-on-k8s](https://rancher.com/docs/rancher/v2.x/en/installation/install-rancher-on-k8s/)
* `microk8s.kubectl create namespace cattle-system`
* `microk8s.helm3 install rancher rancher-stable/rancher --namespace cattle-system --set hostname=rancher.10.20.0.68.xip.io --version 2.4.5`
#### 安裝Harbor、Redmine、Sonarqube、

#### 選擇關閉預設的HA功能(因為rancher無法兼容)
* `microk8s disable ha-cluster`
