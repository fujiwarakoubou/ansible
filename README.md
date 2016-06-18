# Ansible Sample

Ansible練習用のサンプルリポジトリ  
さくらVPSのCentOS7に特化したものを目指してる  


## Usage

テストモジュール実行

```
# yum -y update
# yum -y install ansible
# yum --enablerepo=epel -y install sshpass 
# git clone https://github.com/fujiwarakoubou/ansible.git
# cd ansible/sakuravps/centos7
# ansible-playbook -i hosts test.yml
```

site.ymlの利用について、いくつか編集しないと使えません。

1. hosts を初期設定したいサーバーにして下さい
2. group_vars/all を追加するwheelユーザーにして下さい
3. `ansible-playbook -i hosts site.yml -k`

ディレクトリ構成

```
sakuravps ─ centos7  ┬ group_vars 変数たち
                     │
                     ├ roles      サブモジュールたち
                     │
                     ├ hosts      インベントリファイル（編集して下さい）
                     │
                     ├ site.yml   サーバー初期設定モジュール
                     │
                     └ test.yml   テスト実行モジュール
```

## Relation

[Ansible Best Practices](http://docs.ansible.com/ansible/playbooks_best_practices.html)  
[ansible-examples/lanmp_simple](https://github.com/ansible/ansible-examples/tree/master/lamp_simple)  
[実践！Ansibleベストプラクティス（前編）](http://knowledge.sakura.ad.jp/tech/3084/)  

## Licence

[MIT](https://github.com/fujiwarakoubou/readme/blob/master/MIT)

## Author

[fujiwarakoubou](https://github.com/fujiwarakoubou)


## Other

DockerでAnsibleのテスト環境を構築しようとしてるときのメモ  

* 環境構築させたいサーバー（sshを受信できるようにしておく）  

```
docker pull ansible/centos7-ansible
docker run --privileged --name sshd_server -d ansible/centos7-ansible /sbin/init
docker exec -it sshd_server /bin/bash
yum -y install openssh-server
systemctl start sshd.service
systemctl status sshd
```

* Ansible実行サーバー  (sshpassが必要）

```
docker run --privileged --name ansible_client -d ansible/centos7-ansible /sbin/init
docker exec -it ansible_client /bin/bash
yum --enablerepo=epel -y install sshpass 
cd ~
git clone https://github.com/fujiwarakoubou/ansible.git
cd ansible/sakuravps/centos7
vi hosts
ansible-playbook -i hosts site.yml -k
```

* Ansible実行サーバーから秘密鍵を取り出す  

```
docker cp ansible_client:~/roles/common/files/id_rsa
```

