# Ansible Sample

Ansible���K�p�̃T���v�����|�W�g��  
������VPS��CentOS7�ɓ����������̂�ڎw���Ă�  


## Usage

�e�X�g���W���[�����s

```
# yum -y update
# yum -y install ansible
# yum --enablerepo=epel -y install sshpass 
# git clone https://github.com/fujiwarakoubou/ansible.git
# cd ansible/sakuravps/centos7
# ansible-playbook -i hosts test.yml
```

site.yml�̗��p�ɂ��āA�������ҏW���Ȃ��Ǝg���܂���B

1. hosts �������ݒ肵�����T�[�o�[�ɂ��ĉ�����
2. group_vars/all ��ǉ�����wheel���[�U�[�ɂ��ĉ�����
3. `ansible-playbook -i hosts site.yml -k`

�f�B���N�g���\��

```
sakuravps �� centos7  �� group_vars �ϐ�����
                     ��
                     �� roles      �T�u���W���[������
                     ��
                     �� hosts      �C���x���g���t�@�C���i�ҏW���ĉ������j
                     ��
                     �� site.yml   �T�[�o�[�����ݒ胂�W���[��
                     ��
                     �� test.yml   �e�X�g���s���W���[��
```

## Relation

[Ansible Best Practices](http://docs.ansible.com/ansible/playbooks_best_practices.html)  
[ansible-examples/lanmp_simple](https://github.com/ansible/ansible-examples/tree/master/lamp_simple)  
[���H�IAnsible�x�X�g�v���N�e�B�X�i�O�ҁj](http://knowledge.sakura.ad.jp/tech/3084/)  

## Licence

[MIT](https://github.com/fujiwarakoubou/readme/blob/master/MIT)

## Author

[fujiwarakoubou](https://github.com/fujiwarakoubou)


## Other

Docker��Ansible�̃e�X�g�����\�z���悤�Ƃ��Ă�Ƃ��̃���  

* ���\�z���������T�[�o�[�issh����M�ł���悤�ɂ��Ă����j  

```
docker pull ansible/centos7-ansible
docker run --privileged --name sshd_server -d ansible/centos7-ansible /sbin/init
docker exec -it sshd_server /bin/bash
yum -y install openssh-server
systemctl start sshd.service
systemctl status sshd
```

* Ansible���s�T�[�o�[  (sshpass���K�v�j

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

* Ansible���s�T�[�o�[����閧�������o��  

```
docker cp ansible_client:~/roles/common/files/id_rsa
```

