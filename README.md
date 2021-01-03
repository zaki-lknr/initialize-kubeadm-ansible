# initialize-kubeadm-ansible

CentOS7で`kubeadm init`を実行するために必要なmaster/workerノードの準備と構築を行うplaybook

- firewalld: 有効
- CNIはCalicoまたはFlannel使用前提(firewalld設定は自動)
- firewalldを無効化する場合はinventoryに設定

参考: [[Kubernetes] kubeadmを使ってCentOSへk8sクラスタをデプロイしてみた](https://zaki-hmkc.hatenablog.com/entry/2020/03/19/191534)

# usage

playbookは以下の通り

| playbook                            | 機能                                      |
| ----------------------------------- | --------------------------------------- |
| playbooks/playbook.yaml             | 以下全て実行                                  |
| playbooks/auth_settings.yaml        | sudoers/ssh鍵設定                            |
| playbooks/preparing_hosts.yaml      | ホストの事前設定・CRI、kubeadm/kubectl/kubeletのインストール |
| playbooks/exec_kubeadm_init.yaml    | `kubeadm init`をmasterノード上で実行            |
| playbooks/deploy_cni.yaml           | CNIのインストール(現在はCalico or Flannel指定可)                |
| playbooks/add_master_node.yaml      | masterノードの追加(`kubeadm join`)            |
| playbooks/add_worker_node.yaml      | workerノードの追加(`kubeadm join`)            |

OSインストール後にこの`playbooks/playbook.yaml`を実行すればk8sクラスタがデプロイされる。

```
$ ansible-playbook -i inventory.ini playbooks/playbook.yaml -kK
```

パスワード無しで`sudo`する設定や、ssh鍵設定前の場合は、`-K`および`-k`を付与して実行する。
