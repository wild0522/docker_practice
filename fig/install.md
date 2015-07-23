##�w�� Fig

�����A�w�� 1.3 �Ϊ̧�s�� Docker �����C

�p�G�A���u�@���ҬO OS X �A�i�H�q�L�d�� [Mac �w�˫��n(�^��)](https://docs.docker.com/installation/mac/) �A�����w�� Docker �M boot2docker �C�@�� boot2docker �B���A����H�U���O�]�m�@�������ܶq�A���� Fig �N�i�H�M���椬�F�C

```
$(boot2docker shellinit)
```
**�p�G�Q�קK���ҫ᭫�s�]�m�A�i�H��W�����R�O�[��A�� ` ~/.bashrc` ���̡C*

���� `Ubuntu` �٦� `�䥦�����x` ���w�ˡA�i�H�ѷ� [Ubuntu �w�˫��n(����)](../install/ubuntu.md) �H�� [�x��w�ˤ�U(�^��)](https://docs.docker.com/installation/)�C


�U�@�B�A�w�� Fig �G

```
curl -L https://github.com/docker/fig/releases/download/1.0.1/fig-`uname -s`-`uname -m` > /usr/local/bin/fig; chmod +x /usr/local/bin/fig
```
**�p�G�A�� Docker �O�޲z�������w�ˡA�H�W�R�O�i��]�ݭn�ۦP�������C*

�ثe Fig ���o�檩���u��� OSX �M 64 �쪺 Linux �t�ΡC���]�����O�� Python �y���g���A�ҥH���䥦���x�W���Τ�A�i�H�q�L Python �w�˥]�ӧ����w�ˡ]������t�ΦP�˾A�Ρ^�C

```
$ sudo pip install -U fig
```
��o�̴N�w�g�����F�C ���� `fig --version` �A�T�{������`�B��C

