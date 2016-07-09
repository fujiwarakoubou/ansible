# Ansible Sample

Ansible練習用のサンプルリポジトリ  
さくらVPSのCentOS7に特化したものを目指してる（まだDockerのcentos7コンテナでしか確認できてないです）  
まだまだ未完成です  

## Usage

テストモジュール実行

```
# yum -y update
# yum -y install ansible
# yum --enablerepo=epel -y install sshpass python-passlib
# git clone https://github.com/fujiwarakoubou/ansible.git
# ansible-playbook -i hosts test.yml
```

site.ymlを使うときは、hostsを初期設定してください
デフォルトのアドレスはDockerのコンテナです  
  
UsePAMについては、yesにするとsshがつながらなく可能性が高いので、とりあえずnoにしています  
PAM認証を無効にするのはCentOS7からは推奨されていませんが、いろいろ解決策を試してみましたが上手くいきませんでした  

ディレクトリ構成

```
group_vars      変数たち  
└ all
roles           サブモジュールたち
├ common       
├ debug        
└ sshkeygen    
hosts           インベントリファイル（編集して下さい）
site.yml        サーバー初期設定モジュール
test.yml        テスト実行モジュール
```

## Relation

[Ansible Best Practices](http://docs.ansible.com/ansible/playbooks_best_practices.html)  
[ansible-examples/lanmp_simple](https://github.com/ansible/ansible-examples/tree/master/lamp_simple)  
[実践！Ansibleベストプラクティス（前編）](http://knowledge.sakura.ad.jp/tech/3084/)  
[自分なりにあちこちからかき集めて作ったセキュアなCentOS7サーバ - Qiita:](http://qiita.com/KurokoSin/items/51e79657f1f2104cf607)  

## Licence

[MIT](https://github.com/fujiwarakoubou/readme/blob/master/MIT)

## Author

[fujiwarakoubou](https://github.com/fujiwarakoubou)


## Other

DockerでAnsibleのテスト環境を構築しようとしてるときのメモ  

* イメージのダウンロード

```
docker pull fujiwarakoubou/centos7-systemd-sshd
docker pull fujiwarakoubou/centos7-systemd-sshd-ansible
```

* 環境構築させたいサーバー

```
docker run --name centos7 -d --privileged fujiwarakoubou/centos7-systemd-sshd-firewalld
docker exec  -it centos7 /bin/bash
passwd
firewall-cmd --permanent --zone=public --change-interface=eth0
```

* Ansible実行サーバー

```
docker run --name ansible  -d --privileged fujiwarakoubou/centos7-systemd-sshd-ansible
docker exec  -it ansible /bin/bash
git clone https://github.com/fujiwarakoubou/ansible.git
cd ansible
ansible-playbook -i hosts site.yml -k
```

* コンテナ情報の表示  

```
docker ps -a
docker inspect (コンテナ名またはコンテナID)
```


* Ansible実行サーバーから秘密鍵を取り出す  

```
docker cp ansible_client:~/roles/common/files/id_rsa
```

