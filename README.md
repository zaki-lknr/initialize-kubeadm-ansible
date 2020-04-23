# initialize-kubeadm-ansible

CentOS7で`kubeadm init`を実行するために必要なmaster/workerノードの準備と構築を行うplaybook

- firewalld: 有効(cni0をtrusted)
- CNIはFlannel使用前提(firewalld設定)
- firewalldを無効化する場合はinventoryに設定

参考: [[Kubernetes] kubeadmを使ってCentOSへk8sクラスタをデプロイしてみた](https://zaki-hmkc.hatenablog.com/entry/2020/03/19/191534)

# usage

playbookは以下の通り

| playbook                            | 機能                                      |
| ----------------------------------- | --------------------------------------- |
| playbooks/playbook.yaml             | 以下全て実行                                  |
| playbooks/ssh_keys.yaml             | ssh鍵作成・公開鍵配布                            |
| playbooks/preparing_hosts.yaml      | ホストの事前設定・kubeadm/kubectl/kubeletのインストール |
| playbooks/exec_kubeadm_init.yaml    | `kubeadm init`をmasterノード上で実行            |
| playbooks/configure_kubeconfig.yaml | 一般ユーザの`.kube/config`の設定                 |
| playbooks/deploy_cni.yaml           | CNIのインストール(現在はFlannel限定)                |
| playbooks/add_worker_node.yaml      | workerノードの追加(`kubeadm join`)            |

OSインストール後にこのplaybookを実行し、その後に

- `kubeadm init ...`を実行
- cluster設定(`~/.kube/config`の作成)
- pod networkの設定(`kubectl apply -f kube-flannel.yml`)
- workerの追加(`kubeadm join ...`)

を実行する。
