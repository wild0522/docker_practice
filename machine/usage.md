## �ϥ�

Docker Machine ����h�ث���X�ʡA�]�A�������B���a�D���M�����x���C

### ���a�D�����
�����T�O���a�D���i�H�q�L user �㸹�� key ���� ssh ��ؼХD���C

�ϥ� generic �������X�ʡA�Ыؤ@�x Docker �D���A�R�W�� test�C
```sh
$ docker-machine create -d generic --generic-ip-address=10.0.100.101 --generic-ssh-user=user test
```

�ЫإD�����\��A�i�H�q�L env �R�O��������ާ@��H���O�ؼХD���C
```sh
$ docker-machine env test
```

### ����X��
�q�L `-d` �ﶵ�i�H��ܤ�����X�������C
* amazonec2
* azure
* digitalocean
* exoscale
* generic
* google
* none
* openstack
* rackspace
* softlayer
* virtualbox
* vmwarevcloudair
* vmwarevsphere


### �ާ@�R�O
* `active`                �d�ݬ��D�� Docker �D��
* `config`                ��X�s�����t�m�H��
* `create`                �Ыؤ@�� Docker �D��
* `env`                   ��ܳs����Y�ӥD���ݭn�������ܶq
* `inspect`               ��X�D����h�H��
* `ip`                    ����D���a�}
* `kill`                  ����Y�ӥD��
* `ls`                    �C�X�Ҧ��޲z���D��
* `regenerate-certs`      ���Y�ӥD�����s�ͦ� TLS �{�ҫH��
* `restart`               ���ҥD��
* `rm`                    �R���Y�x�D��
* `ssh`                   SSH ��D���W����R�O
* `scp`                   �b�D�������ƻs���
* `start`                 �Ұʤ@�ӥD��
* `stop`                  ����@�ӥD��
* `upgrade`               ��s�D�� Docker �������̷s
* `url`                   ����D���� URL
* `help, h`               ��X���U�H��

�C�өR�O�A�S�a�����P���ѼơA�i�H�q�L
```sh
docker-machine <COMMAND> -h
```
�Ӭd�ݨ��骺�Ϊk�C
