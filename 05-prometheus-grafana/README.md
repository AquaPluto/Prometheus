# 多种Exporter监控示例

- consul-exporter
- mysqld-exporter
- nginx-exporter
- tomcat

### 各关键服务端口

容器环境本地网络：172.31.0.0/16

- prometheus: 9090/tcp
- consul： 8500/tcp
- tomcat: 8080/tcp
- grafana: 3000/tcp

## mysqld-exporter使用说明

需要在mysqld上运行如下命令，为mysqld-exporter创建具有采集监控数据权限的用户账号

```
  mysql> CREATE USER 'exporter'@'mysqld-exporter' IDENTIFIED BY 'exporter';
  mysql> GRANT PROCESS, REPLICATION CLIENT ON *.* TO 'exporter'@'mysqld-exporter';
  mysql> GRANT SELECT ON performance_schema.* TO 'exporter'@'mysqld-exporter';
```

本示例中，mysqld和mysqld-exporter均运行于172.31.0.0/16网络中，因此，也可按如下方式完成用户创建及授权

```
  mysql> CREATE USER 'exporter'@'172.31.%.%' IDENTIFIED BY 'exporter';
  mysql> GRANT PROCESS, REPLICATION CLIENT ON *.* TO 'exporter'@'172.31.%.%';
  mysql> GRANT SELECT ON performance_schema.* TO 'exporter'@'172.31.%.%';
```

注意：在docker-compose.yml文件中，mysqld-exporter接入mysqld的用户密和密码默认为“exporter”，因此，或需修改，请同时修改该文件中的配置；

## Consul Exporter有用的查询示例

**服务是否正常运行**

```
min(consul_catalog_service_node_healthy) by (service_name)
```

值为 1 表示服务的所有节点都通过。值为 0 表示服务至少有一个节点未正常。

**处于宕机状态的服务节点**

```
sum by (node, service_name)(consul_catalog_service_node_healthy == 0)
```

**获取处于critical状态的服务**

```
consul_health_service_status{status="critical"} == 1
```

您可以查询以下运行状况检查状态：“maintenance”、“critical”、“warning”或“passing”
