## Mesos �t�m���ѪR

Mesos �� [�t�m��](http://mesos.apache.org/documentation/latest/configuration/) �i�H�q�L�ҰʮɭԶǻ��ѼƩΪ̰t�m�ؿ��U��󪺤覡���X�]���ˤ覡�A�@���A�M�^�C

�����T�������G�q�ζ��]master �M slave ������^�A�u�� master ������A�H�Υu�� slave ������C


### �q�ζ�
* `--ip=VALUE` ��ť�� IP �a�}
* `--firewall_rules=VALUE` endpoint ������W�h�A`VALUE` �i�H�O JSON �榡�Ϊ̦s�� JSON �榡�������|�C
* `--log_dir=VALUE` ��x�����|�A�q�{���s�x��x�쥻�a
* `--logbufsecs=VALUE`  buffer �h�֬���x�A�M��g�J���a
* `--logging_level=VALUE` ��x�O�����̧C�ŧO
* `--port=VALUE` ��ť���ݤf�Amaster �q�{�O 5050�Aslave �q�{�O 5051�C

### master �M�ݰt�m��
* `--quorum=VALUE` ���ƶ��A�ϥΰ�� replicated-Log �����U��ɡA�ƻs���Ӽ�
* `--work_dir=VALUE` ���ƶ��A���U����[�ƫH���s�x��m
* `--zk=VALUE` ���ƶ��Azookeepr �����f�a�}�A����h�Ӧa�}�A�����γr���j���A�i�H�������|
* `--acls=VALUE` ACL �W�h�ΩҦb���
* `--allocation_interval=VALUE` ���� allocation �����j�A�q�{�� 1sec
* `--allocator=VALUE` ���t����A�q�{�� HierarchicalDRF
* `--[no-]authenticate` �O�_���\�D�{�ҹL�� framework ���U
* `--[no-]authenticate_slaves` �O�_���\�D�{�ҹL�� slaves ���U
* `--authenticators=VALUE` �� framework �� salves �i��{�Үɪ���{����
* `--cluster=VALUE` ���s�O�W
* `--credentials=VALUE` �s�x�[�K����Ҫ���󪺸��|
* `--external_log_file=VALUE` �ĥΥ~������x���
* `--framework_sorter=VALUE` ���w framework �������귽���t����
* `--hooks=VALUE` master ���w�˪� hook �Ҷ�
* `--hostname=VALUE` master �`�I�ϥΪ��D���W�A���t�m�h�q�t�Τ����
* `--[no-]log_auto_initialize` �O�_�۰ʪ�l�Ƶ��U��ݭn�� replicated ��x
* `--modules=VALUE` �n�[�����Ҷ��A��������|�Ϊ� JSON
* `--offer_timeout=VALUE` offer �M�P���W��
* `--rate_limits=VALUE` framework ���t�v����A��p qps
* `--recovery_slave_removal_limit=VALUE` ������U���_��i�H�����ΰ�� slave �ƥءA�W�X�� master �|���ѡA�q�{�O 100%
* `--slave_removal_rate_limit=VALUE slave` �S���������d���ˬd�ɭԳQ�������t�v�W���A�Ҧp 1/10mins �N��C�Q�����̦h���@��
* `--registry=VALUE` ���U�����[�Ƶ����A�q�{�� `replicated_log`�A�٥i�H�� `in_memory`
* `--registry_fetch_timeout=VALUE` �X�ݵ��U���ѶW��
* `--registry_store_timeout=VALUE` �s�x���U���ѶW��
* `--[no-]registry_strict` �O�_���ӵ��U�����[�ƫH������ާ@�A�q�{�� false
* `--roles=VALUE` ���s�� framework �i�H���ݪ����t����
* `--[no-]root_submissions` root �O�_�i�H���� framework�A�q�{�� true
* `--slave_reregister_timeout=VALUE` �s�� lead master �`�I���|�X�ӫ�A�h�[�����Ҧ��� slave �ݭn���U�A�W�ɪ� salve �N�Q�����������A�q�{�� 10mins
* `--user_sorter=VALUE` �b�Τᤧ�����t�귽�������A�q�{�� drf
* `--webui_dir=VALUE` webui ��{�����ؿ��Ҧb�A�q�{�� `/usr/local/share/mesos/webui`
* `--weights=VALUE` �U�Ө��⪺�v��
* `--whitelist=VALUE` �����|�A�]�A�o�e offer �� slave �W��A�q�{�� None
* `--zk_session_timeout=VALUE` session �W�ɡA�q�{�� 10secs
* `--max_executors_per_slave=VALUE` �t�m�F `--with-network-isolator` �ɥi�ΡA����C�� slave �P�ɰ�����ȭӼ�

### slave �M�ݰt�m��
* `--master=VALUE` ���ƶ��Amaster �Ҧb�a�}�A�� zookeeper �a�}�A�Τ����|�A�i�H�O�C��
* `--attributes=VALUE` �����ݩ�
* `--authenticatee=VALUE` �� master �i��{�ҮɭԪ��{�Ҿ���
* `--[no-]cgroups_enable_cfs` �ĥ� CFS �i��a�e����ɭԹ� CPU �귽�i�歭��A�q�{�� false
* `--cgroups_hierarchy=VALUE` cgroups ���ؿ��ڦ�m�A�q�{�� `/sys/fs/cgroup`
* `--[no-]cgroups_limit_swap` ����s�M swap�A�q�{�� false�A�u����s
* `--cgroups_root=VALUE` �� cgroups ���W�١A�q�{�� mesos
* `--container_disk_watch_interval=VALUE` ���e���i��w�L�t�B�d�ߪ��ɶ����j
* `--containerizer_path=VALUE` �ĥΥ~���j������]`--isolation=external`�^�ɭԡA�~���e�������������|
* `--containerizers=VALUE` �i�Ϊ��e����{����A�]�A mesos�Bexternal�Bdocker
* `--credential=VALUE` �[�K����ҡA�Ϊ̩Ҧb�����|
* `--default_container_image=VALUE` �ĥΥ~���e������ɡA���ȯʬ٨ϥΪ��蹳
* `--default_container_info=VALUE` �e���H�����ʬ٭�
* `--default_role=VALUE` �귽�ʬ٤��t������
* `--disk_watch_interval=VALUE` �w�L�ϥα��p���g�����ˬd���j�A�q�{�� 1mins
* `--docker=VALUE` docker �����󪺸��|
* `--docker_remove_delay=VALUE` �R���e�����e�����ݮɶ��A�q�{�� 6hrs
* `--[no-]docker_kill_orphans` �M���t��e���A�q�{�� true
* `--docker_sock=VALUE` docker sock �a�}�A�q�{�� `/var/run/docker.sock`
* `--docker_mesos_image=VALUE` �B�� slave �� docker �蹳�A�p�G�Q�t�m�Adocker �|���w slave �B��b�@�� docker �e����
* `--docker_sandbox_directory=VALUE` sandbox �M�g��e���̪����Ӹ��|
* `--docker_stop_timeout=VALUE` �����ҫᵥ�ݦh�[���� kill �ާ@�A�q�{�� 0secs
* `--[no-]enforce_container_disk_quota` �O�_�ҥήe���t�B����A�q�{�� false
* `--executor_registration_timeout=VALUE` �������γ̦h�i�H���h�[�A���U�� slave�A�_�h����A�q�{�� 1mins
* `--executor_shutdown_grace_period=VALUE` �������ΰ����A���ݦh�[�A�q�{�� 5secs
* `--external_log_file=VALUE` �~����x���
* `--frameworks_home=VALUE` �������Ϋe�K�[���۹���|�A�q�{����
* `--gc_delay=VALUE` �h�[�M�z�@���������Υؿ��A�q�{�� 1weeks
* `--gc_disk_headroom=VALUE` �վ�p��̤j�������Υؿ��~�֪��w�L�d�Ŷq�A�q�{�� 0.1
* `--hadoop_home=VALUE` hadoop �w�˥ؿ��A�q�{���šA�|�۰ʬd�� HADOOP_HOME �Ϊ̱q�t�θ��|���d��
* `--hooks=VALUE` �w�˦b master ���� hook �Ҷ��C��
* `--hostname=VALUE` slave �`�I�ϥΪ��D���W
* `--isolation=VALUE` �j������A�Ҧp `posix/cpu,posix/mem`�]�q�{�^�Ϊ� `cgroups/cpu,cgroups/mem`
* `--launcher_dir=VALUE` mesos �i�����󪺸��|�A�q�{�� `/usr/local/lib/mesos`
* `--modules=VALUE` �n�[�����Ҷ��A��������|�Ϊ� JSON
* `--perf_duration=VALUE` perf �ļˮɪ��A�����p�� perf_interval�A�q�{�� 10secs
* `--perf_events=VALUE` perf �ļ˪��ƥ�
* `--perf_interval=VALUE` perf �ļ˪��ɶ����j
* `--recover=VALUE` �^�_��O�_���s�W�ª���������
* `--recovery_timeout=VALUE` slave ��_�ɪ��W�ɡA�Ӥ[�h�Ҧ��������������αN�ۦ�h�X�A�q�{�� 15mins
* `--registration_backoff_factor=VALUE` �� master �i����U�ɭԪ����ծɶ����j��k���]�l�A�q�{�� 1secs�A�ĥ��H�����ƺ�k�A�̪� 1mins
* `--resource_monitoring_interval=VALUE` �g���ʺʴ��������θ귽�ϥα��p�����j�A�q�{�� 1secs
* `--resources=VALUE` �C�� slave �i�Ϊ��귽
* `--slave_subsystems=VALUE` slave �B��b���� cgroup �l�t�Τ��A�]�A memory�Acpuacct ���A�ʬ٬���
* `--[no-]strict` �O�_�{���Ҧ����~�����i�����A�q�{�� true
* `--[no-]switch_user` �δ�����Ȫ��Τᨭ���ӹB��A�q�{�� true
* `--fetcher_cache_size=VALUE` fetcher �� cache �j�p�A�q�{�� 2 GB
* `--fetcher_cache_dir=VALUE` fetcher cache ���s��ؿ��A�q�{�� /tmp/mesos/fetch
* `--work_dir=VALUE` framework ���u�@�ؿ��A�q�{�� /tmp/mesos

�U�����ﶵ�ݭn�t�m `--with-network-isolator` �@�_�ϥ�
* `--ephemeral_ports_per_container=VALUE` ���t���@�Ӯe�����{�ɺݤf�A�q�{�� 1024
* `--eth0_name=VALUE` public ���������f�W�١A�p�G�����w�A�ھڥD�����Ѷi��q��
* `--lo_name=VALUE` loopback ���d�W��
* `--egress_rate_limit_per_container=VALUE` �C�Ӯe���� egress �y�q����t�v
* `--[no-]network_enable_socket_statistics` �O�_�Ķ��C�Ӯe���� socket �έp�H���A�q�{�� false
