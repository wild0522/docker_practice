##環境變量參考

*注意: 現在已經不推薦使用環境變量鏈接服務。替代方案是使用鏈接名稱（預設就是被連接的服務名字）作為主機名來鏈接。詳情查看 [fig.yml章節](./yml_ref.md)。

Fig 使用 Docker 鏈接來暴露一個服務的容器給其它容器。每一個鏈接的容器會注入一組以容器名稱的大寫字母開頭得環境變量。

查看一個服務有那些有效的環境變量可以執行 `fig run SERVICE env`。

`name_PORT`

完整URL，例如： `DB_PORT=tcp://172.17.0.5:5432`

`name_PORT_num_protocol`

完整URL，例如： `DB_PORT_5432_TCP=tcp://172.17.0.5:5432`

`name_PORT_num_protocol_ADDR`

容器的IP地址，例如： `DB_PORT_5432_TCP_ADDR=172.17.0.5`

`name_PORT_num_protocol_PORT`

暴露連接阜號，例如： `DB_PORT_5432_TCP_PORT=5432`

`name_PORT_num_protocol_PROTO`

協議（tcp 或 udp），例如： `DB_PORT_5432_TCP_PROTO=tcp`

`name_NAME`

完整合格的容器名稱，例如： `DB_1_NAME=/myapp_web_1/myapp_db_1`
