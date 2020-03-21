# initialize-kubeadm-ansible

CentOS7で`kubeadm init`を実行するために必要なmaster/workerノードの準備と構築を行うplaybook

- firewalld: 有効(cni0をtrusted)
- CNIはFlannel使用前提(firewalld設定)
- firewalldを無効化する場合はinventoryに設定

参考: [[Kubernetes] kubeadmを使ってCentOSへk8sクラスタをデプロイしてみた](https://zaki-hmkc.hatenablog.com/entry/2020/03/19/191534)
