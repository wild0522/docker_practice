## 安裝
安裝swarm的最簡單的方式是使用Docker官方的swarm鏡像
> $ sudo docker pull swarm 

可以使用下面的命令來查看swarm是否成功安裝。
 > $ sudo docker run --rm swarm -v
 
 輸出下面的形式則表示成功安裝(具體輸出根據swarm的版本變化)
> swarm version 0.2.0 (48fd993)
