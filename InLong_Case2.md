## 使用 Apache InLong 把csv文件内容采集到mysql中


### Apache Inlong 部署

​	本文主要使用dockers部署模式，将Apache InLong部署在服务器上。其中，使用docker compose部署Apache InLong的部署方式适用于开发和调试模式，不适用于生产环境当中。该种模式包含了标准架构所需要的所有组件，并使用Apache Pulsar 作为消息队列服务。

#### Apache Inlong 下载

​	在[下载 | Apache InLong](https://inlong.apache.org/zh-CN/download/)下载apache-inlong-[version]-bin.tar.gz安装包至linux服务器中，本文使用centos7.9版本下载1.6版本inlong。

#### 启动集群

```
cd docker/docker-compose
docker-compose up -d
```

​	集群成功启动之后，可以http://localhost页面看到如下页面：

![image-20230524144044500](./res/1.png)

```
User: admin
Password: inlong
```

​	输入账号密码即可进入页面。

![image-20230524144141795](./res/2.png)



### Apache Inlong 配置

#### 注册 pulsar 集群

​页面点击 [集群管理]->[集群管理]->[新建集群]，注册 Pulsar 集群：

![image-20230524144310302](./res/3.png)

​测试连接是否正常，正常进行下一步。集群标签、责任人、url等为默认参数，不必更改。

#### 配置 mysql 数据库

![image-20230524144508599](./res/4.png)

ip、用户名、密码为自己服务器的内容，根据服务器更改。

#### 配置数据流信息

![image-20230524144638934](./res/5.png)

​字段及类型根据实验内容进行修改。

#### 配置数据源 csv、log类型

![image-20230524144729660](./res/7.png)

数据源IP：与集群管理中agent节点的IP保持一致。

文件路径：在linux中的agent容器中/data/collect-data下存放。

#### 配置数据源 mysql

​本文使用MySQL作为数据源，配置如下：

![image-20230524145124806](./res/8.png)

ip、用户名、密码、表名白名单根据需要更改。

my_data.tags: my_data为数据库名

​							tags为数据库表名

#### 配置数据目的地址 MySQL

![image-20230524145350890](./res/9.png)

​数据节点即为2.2.2注册的数据库节点。

#### 数据采集

​完成以上配置之后，点击提交，经过负责人审查同意之后，便可以开始采集任务，直至数据采集完成。


### 使用示例

1. 根据上述内容完成inlong基本配置。

2. 本示例把csv文件内容采集到mysql中，文件包含userId，movieId，rating三个字段。

3. 数据流配置信息如下：

![image-20230524150015358](./res/6.png)

4. 数据源配置信息如下：

![image-20230524150036729](./res/7.png)

5. 接收端配置如下：

![image-20230524150100605](./res/9.png)

6. 完成提交后，采集端页面展示如下：

![image-20230524150126378](./res/10.png)