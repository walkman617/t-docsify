## 使用 Apache InLong 创建 MySQL -> Kafka -> ClickHouse 数据链路


### 资源准备
- 下载最近发布的 Apache InLong 1.7.0 安装包

下载地址：https://downloads.apache.org/inlong/1.7.0/apache-inlong-1.7.0-bin.tar.gz

- 准备 Docker、Docker Compose 环境

### 环境部署

- 安装 ClickHouse

```
docker run -d --rm --net=host --name clickhouse -e CLICKHOUSE_USER=admin -e CLICKHOUSE_PASSWORD=inlong -e CLICKHOUSE_DEFAULT_ACCESS_MANAGEMENT=1 clickhouse/clickhouse-server:22.8
```

- 安装 Kafka

```
# 准备 Kafka 需要的 zookeeper
docker run -d --name zookeeper -p 2181:2181 -t wurstmeister/zookeeper
# 安装 Kafka 
docker run -d --name kafka -p 9092:9092 -e KAFKA_BROKER_ID=0 -e KAFKA_ZOOKEEPER_CONNECT=zookeeper:2181 --link zookeeper -e KAFKA_ADVERTISED_LISTENERS=PLAINTEXT://kafka:9092 -e KAFKA_LISTENERS=PLAINTEXT://0.0.0.0:9092 -t wurstmeister/kafka
```

- 安装 Apache InLong

解压 apache-inlong-1.7.0-bin.tar.gz，
并修改 docker-compose.yml 中的MQ_TYPE=kafka。

```
# 进入安装目录
cd docker/docker-compose
# 修改 MQ_TYPE 类型
sed -i 's/MQ_TYPE=pulsar/MQ_TYPE=kafka/g' docker-compose.yml
# 启动
docker-compose up -d
```

### 集群初始化

容器启动成功后，访问 InLong Dashboard 地址 http://localhost，并使用以下默认账号登录：

```
User: admin
Password: inlong
```

- 创建集群标签

页面点击 【集群管理】->【标签管理】->【新建】，指定集群标签名称和负责人：

![case4](./res/b1.jpeg)

注：default_cluster 是各个组件默认上报集群标签，请勿使用其它名称

- 注册 Kafka 集群

页面点击 【集群管理】->【新建集群】，注册 Kafka 集群：

![case4](./res/b2.jpeg)

- 注册 Clickhouse 数据节点

页面点击 【数据节点】→【新建】，新增 ClickHouse 数据节点：

![case4](./res/b3.jpeg)

### 任务创建
- 新建数据流组

页面点击【数据集成】→【新建数据流组】，填写 ID，选择 MQ 类型为 Kafka 即可：

![case4](./res/b4.jpeg)

- 新建数据流

点击【下一步】→【新建数据流】，配置数据流 ID 及测试源字段信息：

![case4](./res/b5.jpeg)

- 新建数据源

点击 【新建数据源】→【MySQL】，配置数据源名称、地址、用户密码以及库表信息等

![case4](./res/b6.jpeg)

- 新建数据目标

点击 【新建数据目标】→【ClickHouse】，配置名称、库表、已创建的 ck 数据节点以及 Schema 映射信息等，完成后点击 【提交审批】

![case4](./res/b7.jpeg)

- 审批数据流

页面点击【审批管理】->【我的审批】->【详情】->【选择集群标签】->【通过】

![case4](./res/b8.jpeg)

返回 【数据集成】页面，等待任务配置成功

![case4](./res/b9.jpeg)

### 测试数据
- 发送数据

![case4](./res/b10.jpeg)

累计对 MySQL 添加 1001 条数据

- 数据验证

查看审计页面发送数据

![case4](./res/b11.jpeg)

然后进入 ClickHouse 容器，查看库表数据：

![case4](./res/b12.jpeg)

### 常见问题
- 任务配置失败

一般是 MQ 或者 Flink 集群配置错误导致，可以在页面查看错误信息，或者进入 Manager 容器查看详细日志。