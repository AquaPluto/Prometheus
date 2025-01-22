# Prometheus Components Compose

本示例中的每个目录，均是可独立运行的docker compose配置示例，它们分别用于编排同的应用，具体的组件请参考各目录的名称。
## 部署服务

### 启动Consul和Consul Exporter

```bash 
cd 01-prometheus-components-compose/consul-and-exporter/
docker-compose up -d
```

### 启动Prometheus Server

```bash
cd 01-prometheus-components-compose/prometheus-server/
docker-compose up -d
```

> 提示：可以直接使用示例中的examples.yml示例配置文件替换默认的prometheus.yml配置文件，从而直接监控各目标组件；

### 启动Node Exporter

```bash
cd node-exporter/
docker-compose up -d
```

### 启动MySQL Server和mysqld_exporter

```bash
cd mysqld-and-exporter/
docker-compose up -d
```

而后需要接入mysql server，创建用于监控的用户并完成授权

```bash
docker-compose exec mysqld /bin/sh
mysql
```

而后执行如下mysql命令

```
mysql> CREATE USER 'exporter'@'192.168.%.%' IDENTIFIED BY 'exporter';
mysql> GRANT PROCESS, REPLICATION CLIENT ON *.* TO 'exporter'@'192.168.%.%';
mysql> GRANT SELECT ON performance_schema.* TO 'exporter'@'192.168.%.%';
```

> 提示：本示例中，mysqld和mysqld-exporter容器运行于一个自定义的网络192.168.0.0/16之中。

### 启动Nginx和nginx_exporter

```bash
cd nginx-and-exporter/
docker-compose up -d
```

### 启动tomcat

```bash
cd tomcat-and-metrics/
docker-compose build
docker-compose up -d
```

#### 启动Blackbox Exporter

...

#### 启用AlertManager

...