# NUC 服务器


## 端口分配
> *端口分配原则: 老的端口不变，新的端口从8100开始分配*

| 服务名称   | 内部端口 | 外部端口 | 端口描述                             |
| ---------- | -------- | -------- | ------------------------------------ |
| ssh        | 22       | 30022    | ssh端口                              |
| nginx      | 80       | 30080    | ngix                                 |
| postgresql | 5432     | 35432    | postgresql数据库端口，暂时不对外开发 |
| jellyfin   | 8096     | 38096    | jellyfin端口                         |
| ss-local   | 1080     |          | 本地socks5代理                       |
| http代理   | 8118     |          | 本地http代理                         |
| plantuml   | 8089     | 38089    | plantuml server                      |
| gitlab     | 8100     |          | gitlab http端口                      |
| gitlab     | 8101     |          | gitlab ssh 端口  ｜                  |