# Ansible Sample

Ansible練習用のサンプルリポジトリ  
さくらVPSのCentOS7に特化したものを目指してる→Dockerのcentos7コンテナでしか確認（UsePAMに問題あり）→さくらVPSで確認しました（UsePAM問題なし）  
もし利用するならば、hostsを環境に合わせて修正してください  
デフォルトのアドレスはDockerの最初に立てたコンテナです  

## Usage

テストモジュール実行

```
# yum -y update
# yum -y install ansible
# yum --enablerepo=epel -y install sshpass python-passlib
# git clone https://github.com/fujiwarakoubou/ansible.git
# ansible-playbook -i hosts test.yml
```

ディレクトリ構成

```
group_vars      変数たち  
└ all
roles           サブモジュールたち
├ common       
├ debug        
├ firewalld    
├ reboot       
└ sshkeygen    
hosts           インベントリファイル（編集して下さい）
site.yml        サーバー初期設定モジュール
test.yml        テスト実行モジュール
```

## Relation

[Best Practices — Ansible Documentation](http://docs.ansible.com/ansible/playbooks_best_practices.html)  
[ansible-examples/lamp_simple at master · ansible/ansible-examples](https://github.com/ansible/ansible-examples/tree/master/lamp_simple)  
[実践！Ansibleベストプラクティス（前編） - さくらのナレッジ](http://knowledge.sakura.ad.jp/tech/3084/)  
[自分なりにあちこちからかき集めて作ったセキュアなCentOS7サーバ - Qiita](http://qiita.com/KurokoSin/items/51e79657f1f2104cf607)  

## Licence

[MIT](https://github.com/fujiwarakoubou/readme/blob/master/MIT)

## Author

[fujiwarakoubou](https://github.com/fujiwarakoubou)


## Other

DockerでAnsibleのテスト環境を構築しようとしてるときのメモ  

* イメージのダウンロード

```
docker pull fujiwarakoubou/centos7-systemd-sshd-firewalld
docker pull fujiwarakoubou/centos7-systemd-sshd-ansible
```

* 環境構築させたいサーバー

```
docker run --name centos7 -d --privileged fujiwarakoubou/centos7-systemd-sshd-firewalld
docker exec -it centos7 /bin/bash
passwd
firewall-cmd --permanent --zone=public --change-interface=eth0
firewall-cmd --reload
```

* Ansible実行サーバー

```
docker run --name ansible -d --privileged fujiwarakoubou/centos7-systemd-sshd-ansible
docker exec -it ansible /bin/bash
git clone https://github.com/fujiwarakoubou/ansible.git
cd ansible
ansible-playbook -i hosts site.yml -k
```

* コンテナ情報の表示  

```
docker inspect (コンテナ名またはコンテナID)
```


* Ansible実行サーバーから秘密鍵を取り出す  

```
docker cp ansible:/root/ansible/roles/common/files/id_ed25519 id_ed25519
docker cp ansible:/root/ansible/roles/common/files/id_ed25519.pub id_ed25519.pub
```

