## docker-compose 一键部署zabbix-proxy,zabbix-agent
基于官方文档做了精简

### zabbix-proxy配置文件挂载位置
./common/conf/zabbix-proxy/zabbix-proxy.conf

### 环境配置文件
mysql: ./common/env/mysql/env_mysql
MYSQL_DATABASE=zabbix_proxy
MYSQL_USER=zabbix
MYSQL_PASSWORD=zabbix.proxy
MYSQL_ROOT_PASSWORD=zabbix

zabbix-proxy: ./common/env/zabbix-proxy/env_proxy
DB_SERVER_HOST=zabbix-mysql
MYSQL_DATABASE=zabbix_proxy
MYSQL_USER=zabbix
MYSQL_PASSWORD=zabbix.proxy
ZBX_HOSTNAME=zbx-proxy
ZBX_SERVER_HOST=ZABBIX_SERVER_HOST_IP
ZBX_TIMEOUT=120
ZBX_CONFIGFREQUENCY=300
ZBX_DATASENDERFREQUENCY=3
ZBX_LOGTYPE=file
ZBX_LOGFILE=/tmp/zabbix_proxy.log

zabbix-agent: ./common/env/zabbix-agent/env_agent
ZBX_SERVER_HOST=192.168.70.180
ZBX_SERVER_PORT=10051
ZBX_HOSTNAME=docker-test
ZBX_LISTENPORT=10050
ZBX_LOGTYPE=file
ZBX_LOGFILE=/tmp/zabbix_agentd.log

使用前修改对应环境配置文件字段

### 修改zabbix-agent 日志记录方式
通过修改官方./etc/zabbix-agent/docker-entrypoint.sh文件内的日志记录方式,将agent的日志以文件方式记录在宿主机目录./logs/zabbix-agent/zabbix-agentd.log
docker-entrypoint.sh内字段内容:
    update_config_var $ZBX_AGENT_CONFIG "LogType" "${ZBX_LOGTYEP}"
    update_config_var $ZBX_AGENT_CONFIG "LogFile" "${ZBX_LOGFILE}"

如果不需要使用文件方式记录日志,将字段改为
    update_config_var $ZBX_AGENT_CONFIG "LogType" "console"
    update_config_var $ZBX_AGENT_CONFIG "LogFile"

则默认为console方式


## 使用方式
```
cd zabbix-proxy
docker-compose up -d
```



