# Docker�R�O�d��

##�򥻻y�k
    docker [OPTIONS] COMMAND [arg...]
�@��ӻ��ADocker �R�O�i�H�ΨӺ޲z daemon�A�Ϊ̳q�L CLI �R�O�޲z�蹳�M�e���C�i�H�q�L `man docker` �Ӭd�ݳo�ǩR�O�C


##�ﶵ
    -D=true|false
        �ϥ� debug �Ҧ��C�q�{�� false�C

    -H, --host=[unix:///var/run/docker.sock]: tcp://[host:port]�Ӹj�w�Ϊ� unix://[/path/to/socket] �ӨϥΡC
        �b daemon �Ҧ��U�j�w�� socket�A�q�L�@�өΦh�� tcp://host:port, unix:///path/to/socket, fd://* or fd://socketfd �ӫ��w�C

    --api-enable-cors=true|false
        �b���� API ���ҥ� CORS �Y�C�ʬ٬� false�C

    -b=""
        �N�e��������@�Ӥw�s�b�������W�C���w�� 'none' �ɫh�T�ήe���������C

    --bip=""
        ���ʺA�Ыت� docker0 �ĥε��w�� CIDR �a�}; �P -b �ﶵ�����C

    -d=true|false
        �ϥ� daemon �Ҧ��C�ʬ٬� false�C

    --dns=""
        �� Docker �ϥε��w�� DNS �A�Ⱦ��C

    -g=""
        ���w Docker �B��ɪ� root ���|�C�ʬ٬� /var/lib/docker�C

    --icc=true|false
        �ҥήe�����q�H�C�q�{�� true�C

    --ip=""
        �j�w�ݤf�ɭԪ��q�{ IP �a�}�C�ʬ٬� 0.0.0.0�C

    --iptables=true|false
        �T�� Docker �K�[ iptables �W�h�C�ʬ٬� true�C

    --mtu=VALUE
        ���w�e�������� mtu�C�ʬ٬� 1500�C

    -p=""
        ���w daemon �� PID �����|�C�ʬ٬� /var/run/docker.pid�C

    -s=""
        �j�� Docker �B��ɨϥε��w���s�x�X�ʡC

    -v=true|false
        ��X�����H���ðh�X�C�ʬ٭Ȭ� false�C

    --selinux-enabled=true|false
        �ҥ� SELinux ����C�ʬ٭Ȭ� false�CSELinux �ثe����� BTRFS �s�x�X�ʡC


##�R�O
Docker ���R�O�i�H�ĥ� `docker-CMD` �Ϊ� `docker CMD` ���覡����C��̤@�P�C

    docker-attach(1)
        �̪���@�ӥ��b�B�檺�e�����C

    docker-build(1)
        �q�@�� Dockerfile �Ыؤ@���蹳

    docker-commit(1)
        �q�@�Ӯe�����ק襤�Ыؤ@�ӷs���蹳

    docker-cp(1)
        �q�e�����ƻs����J�D�t�Τ�

    docker-diff(1)
        �ˬd�@�Ӯe�����t�Ϊ��ק�

    docker-events(1)
        �q�A�Ⱥ������ɪ��ƥ�

    docker-export(1)
        �ɥX�e�����e���@�� tar �]

    docker-history(1)
        ��ܤ@���蹳�����v

    docker-images(1)
        �C�X�s�b���蹳

    docker-import(1)
        �ɤJ�@�Ӥ��]�嫬�� tar �]�^���|�Υؿ��ӳЫؤ@���蹳

    docker-info(1)
        ��ܤ@�Ǭ������t�ΫH��

    docker-inspect(1)
        ��ܤ@�Ӯe�������h����H���C

    docker-kill(1)
        �����@�ӹB�椤���e�� (�]�A�i�{�M�Ҧ��귽)

    docker-load(1)
        �q�@�� tar �]���[���@���蹳

    docker-login(1)
        ���U�εn����@�� Docker ���ܮw�A�Ⱦ�

    docker-logout(1)
        �q Docker ���ܮw�A�Ⱦ��n�X

    docker-logs(1)
        ����e���� log �H��

    docker-pause(1)
        �Ȱ��@�Ӯe�������Ҧ��i�{

    docker-port(1)
        �d��@�� nat ��@�Өp�����f�����@�f

    docker-ps(1)
        �C�X�e��

    docker-pull(1)
        �q�@��Docker���ܮw�A�Ⱦ��U�Ԥ@���蹳�έܮw

    docker-push(1)
        �N�@���蹳�Ϊ̭ܮw���e��@�� Docker �����U�A�Ⱦ�

    docker-restart(1)
        ���Ҥ@�ӹB�椤���e��

    docker-rm(1)
        �R�����w���Y�z�Ӯe��

    docker-rmi(1)
        �R�����w���Y�z���蹳

    docker-run(1)
        �Ыؤ@�ӷs�e���A�æb�䤤�B�浹�w�R�O

    docker-save(1)
        �O�s�@���蹳�� tar �]���

    docker-search(1)
        �b Docker index ���j���@���蹳

    docker-start(1)
        �Ұʤ@�Ӯe��

    docker-stop(1)
        �פ�@�ӹB�椤���e��

    docker-tag(1)
        ���@���蹳������

    docker-top(1)
        �d�ݤ@�Ӯe���������b�B�檺�i�{�H��

    docker-unpause(1)
        �N�@�Ӯe�����Ҧ����i�{�q�Ȱ����A����_

    docker-version(1)
        ��X Docker �������H��

    docker-wait(1)
        ���몽��@�Ӯe���פ�A�M���X�����h�X��

##�@�i���`�� Docker ���R�O

![�R�O�g��](../_images/cmd_logic.png)
