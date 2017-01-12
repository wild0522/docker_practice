##環境變數參考

*注意: 現在已經不推薦使用環境變數連結服務。替代專案是使用連結名稱（ 預設就是被連線的服務名字）作為主電腦名稱來連結。詳情 檢視 [fig.yml章節](./yml_ref.md)。

Fig 使用 Docker 連結來暴露一個服務的容器給其它容器。每一個連結的容器會注入一組以容器名稱的大寫字母開頭得環境變數。

 檢視一個服務有那些有效的環境變數可以執行 `fig run SERVICE env`。

`name_PORT`

完整URL，例如： `DB_PORT=tcp://172.17.0.5:5432`

`name_PORT_num_protocol`

完整URL，例如： `DB_PORT_5432_TCP=tcp://172.17.0.5:5432`

`name_PORT_num_protocol_ADDR`

容器的IP位址，例如： `DB_PORT_5432_TCP_ADDR=172.17.0.5`

`name_PORT_num_protocol_PORT`

暴露連接埠號碼，例如： `DB_PORT_5432_TCP_PORT=5432`

`name_PORT_num_protocol_PROTO`

協定（tcp 或 udp），例如： `DB_PORT_5432_TCP_PROTO=tcp`

`name_NAME`

完整合格的容器名稱，例如： `DB_1_NAME=/myapp_web_1/myapp_db_1`
