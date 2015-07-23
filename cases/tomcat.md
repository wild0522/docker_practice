## �Ы� tomcat/weblogic ���s
### �w�� tomcat �蹳
�ǳƦn�ݭn�� jdk�Btomcat ���n���� home �ؿ��U���A�Ұʤ@�Ӯe��
```
docker run -t -i -v /home:/opt/data  --name mk_tomcat ubuntu /bin/bash
```
�o���R�O�������a home �ؿ���e���� /opt/data �ؿ��A�e�����ؿ��Y���s�b�A�h�|�۰ʳЫءC���U�ӴN�O tomcat ���򥻰t�m�Ajdk �����ܶq�]�m�n����A�N tomcat �{�ǩ�� /opt/apache-tomcat �U��
�s�� /etc/supervisor/conf.d/supervisor.conf ���A�K�[ tomcat ��
```
[supervisord]
nodaemon=true

[program:tomcat]
command=/opt/apache-tomcat/bin/startup.sh

[program:sshd]
command=/usr/sbin/sshd -D
```

```
docker commit  ac6474aeb31d  tomcat
```

�s�� tomcat ��󧨡A�s�� Dockerfile�C
```
FROM mk_tomcat
EXPOSE  22 8080
CMD ["/usr/bin/supervisord"]
```
�ھ� Dockerfile �Ы��蹳�C
```
docker build tomcat tomcat
```
### �w�� weblogic �蹳

�B�J�M tomcat �򥻤@�P�A�o�̶K�@�U�t�m���
```
supervisor.conf
[supervisord]
nodaemon=true


[program:weblogic]
command=/opt/Middleware/user_projects/domains/base_domain/bin/startWebLogic.sh

[program:sshd]
command=/usr/sbin/sshd -D
dockerfile
FROM weblogic
EXPOSE  22 7001
CMD ["/usr/bin/supervisord"]
```

### tomcat/weblogic �蹳���ϥ�
#### �s�x���ϥ�
�b�Ұʪ��ɭԡA�ϥ� `-v` �Ѽ�

    -v, --volume=[]            Bind mount a volume (e.g. from the host: -v /host:/container, from docker: -v /container)

�N���a�ϽL�M�g��e�������A���b�D���M�e�������O����ܤƪ��A�ҥH�ڭ̧�s�{�ǡB�W�ǥN�X�u�ݭn��s���z�D�����ؿ��N�i�H�F

#### tomcat �M weblogic ���s����{
tomcat �u�n�}�Ҧh�Ӯe���Y�i
```
docker run -d -v -p 204:22 -p 7003:8080 -v /home/data:/opt/data --name tm1 tomcat /usr/bin/supervisord
docker run -d -v -p 205:22 -p 7004:8080 -v /home/data:/opt/data --name tm2 tomcat /usr/bin/supervisord
docker run -d -v -p 206:22 -p 7005:8080 -v /home/data:/opt/data --name tm3 tomcat /usr/bin/supervisord
```

�o�̻��@�U weblogic ���t�m�A�j�a���D weblogic ���@�Ӱ쪺�����C�p�G�n�ϥα`�W�� administrator +node ���覡���p�A�N�ݭn�b supervisord �����O�g�X administartor server �M node server ���Ұʸ}���A�o�˰����u�I�O�G
* �i�H�ϥ� weblogic �����s�A�P�B������
* ���p�@�Ӷ��s���ε{�ǡA�u�ݭn�w�ˤ@�����Ψ춰�s�W�Y�i

���I�O�G
* Docker �t�m�����F
* �S��k�۰��X�i���s���p��e�q�A�p�ݲK�[�`�I�A�ݭn�b administrator �W���Ыظ`�I�A�M��A�t�m�s���e�� supervisor �Ұʸ}���A�M��A�Ұʮe��

�t�~�ؤ�k�O�N�Ҧ����{�ǳ��w�˦b adminiserver �W���A�ݭn�X�i���ɭԡA�Ұʦh�Ӹ`�I�Y�i�A�����u�I�M���I�M�W�@�ؤ�k���ۤϡC�]��ĳ�ϥγo�ؤ覡�ӳ��p�}�o�M�������ҡ^
```
docker run -d -v -p 204:22 -p 7001:7001 -v /home/data:/opt/data --name node1 weblogic /usr/bin/supervisord
docker run -d -v -p 205:22 -p 7002:7001 -v /home/data:/opt/data --name node2 weblogic /usr/bin/supervisord
docker run -d -v -p 206:22 -p 7003:7001 -v /home/data:/opt/data --name node3 weblogic /usr/bin/supervisord
```

�o�˦b�e�ݨϥ� nginx �Ӱ��t�����ŴN�i�H�����t�m�F
