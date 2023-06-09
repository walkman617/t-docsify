

## 使用 Apache InLong 把csv文件内容采集到消息队列（pulsar）中

本项目为基于Apache InLong的项目实践，使用了InLong的自主消费模式。

实践内容包括数据收集与数据传输：将三个数据集（csv文件）通过InLong分别发往数据队列pulsar的不同topic中，之后从pulsar中取出数据并以文件形式存储。



### 安装环境

```
Ubuntu 20.04.6 LTS
```


#### 安装 Docker 

1. 卸载旧版本Docker库

```
sudo apt-get remove docker docker-engine docker.io containerd runc
```

2. 获取软件最新源

```
sudo apt-get update
```

3. 安装 apt 依赖包

```
sudo apt-get -y install apt-transport-https ca-certificates curl software-properties-common
```

   ![](imgs/apt-dep.png)

4. 安装GPG证书

```
curl -fsSL https://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
```

   ![](imgs/apt-key.png)

5. 验证

```
sudo apt-key fingerprint 0EBFCD88
```

   ![](imgs/apt-key-v.png)

6. 设置稳定版仓库

```
sudo add-apt-repository "deb [arch=amd64] https://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable"
```

   ![](imgs/apt-res.png)



#### 安装 Docker-Engine 

1. 更新 apt 包索引

```
sudo apt-get update
```

2. 安装最新版本

```
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

   ![](imgs/engine.png)

3. 测试

```
sudo docker run hello-world
```

   ![](imgs/test.png)



#### 安装 Docker-Compose

1. 切换至root用户

```
sudo -i
```

2. 下载安装

```
curl -L https://get.daocloud.io/docker/compose/releases/download/v2.6.1/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
```

   ![](imgs/compose-i.png)

3. 授权

```
sudo chmod +x /usr/local/bin/docker-compose
```

4. 查看

```
docker-compose -v
```

   ![](imgs/compose-v.png)



#### 参考链接

`https://blog.csdn.net/u012590718/article/details/125632482`

`https://blog.csdn.net/u012590718/article/details/125702606`



### InLong 安装与部署

#### 操作步骤

1. 下载&解压

    `https://downloads.apache.org/inlong/1.6.0/apache-inlong-1.6.0-bin.tar.gz`

2. 启动 Docker 

   ```
   service docker start
   ```

4. 部署

   - 切换至root用户

   - 切换路径至apache-inlong-1.6.0/docker/docker-compose

   - 运行

   ```
   docker-compose up -d
   ```

   ![image-20230422213619795](imgs/image-20230422213619795.png)

5. 访问 InLong Dashboard

地址： http://localhost ，

并使用以下默认账号登录:

```
User: admin
Password: inlong
```


   ![](imgs/web-login.png)

   ![](imgs/web.png)



#### 参考链接

`https://inlong.apache.org/zh-CN/docs/introduction`




### InLong 使用

#### 新建集群标签

```
[集群管理]->[标签管理]->[新建]，指定集群标签名称和负责人

注：由于各个组件默认上报集群标签为default_cluster，请勿使用其它名称。
```

![image-20230422221238985](imgs/image-20230422221238985.png)



#### 注册 Pulsar 集群

```
[集群管理]->[集群管理]->[新建集群]，注册 Pulsar 集群
```

<img src="imgs/image-20230422221432881.png" alt="image-20230422221432881" style="zoom:50%;" />



#### 新建数据流组

1. 使用用户名/密码（admin/inlong）登录至 Inlong 数据集成主界面

   ![](imgs/Inlong0.png)


2. 点击新建数据流组，进入数据流组创建界面

```
分别输入数据流组ID，责任人。

选择 MQ(消息队列)类型，示例中我们选择Pulsar
```

   ![](imgs/Inlong1.png)


3. 点击下一步，进入数据流创建界面

   ![](imgs/Inlong2.png)


4. 点击新建数据流，创建数据流

```
分别输入数据流ID，选择数据格式，数据编码，点击确定，完成数据流的创建。

注：源数据字段-字段名随意填写即可。
```

   ![](imgs/Inlong3.png)


5. 点击新建数据源，创建数据来源

```
分别输入数据源名称，选择类型、集群名称、数据源IP，输入文件路径，完成数据来源的创建。

数据源IP：与集群管理中agent节点的ip保持一致。

文件路径：在agent容器中的 /data/collect-data 目录下存放。


例如：本实例中选择了某个机器学习问题所需的三个数据集（csv文件）作为数据源。

打开链接：https://github.com/wang273257881/inlong-pulsar-demo.git

拉至本地后解压data.zip文件，可以看到本实验用到的三个csv文件。

```

![](imgs/0422-1.png)


6. 重复第4、5步，完成三个数据流及数据来源的创建，最后点击提交审批，完成数据流组的创建过程

   ![](imgs/Inlong5.png)



#### 审批新建的数据流组

1. 点击进入审批管理界面，可以看到我们刚刚创建的数据流组目前的状态处于待审批

   ![](imgs/Inlong6.png)


2. 点击详情，选择集群标签和数据上报方式（发往MQ），点击通过

   ![](imgs/Inlong7.png)


3. 回到数据集成的界面，可以看到我们刚刚创建的数据流组配置成功

   ![](imgs/Inlong8.png)



#### 新建数据订阅

1. 点击进入数据订阅界面

   ![](imgs/Inlong9.png)


2. 点击新建订阅，随后选择消费组名称，订阅负责人，所属的数据流组ID，Topic名称，点击提交审批

   ```
   注：Topic名称即数据流名称。
   ```
   
   ![](imgs/Inlong10.png)


3. 重复第二步，完成三个数据订阅的创建，如图可以看到我们刚刚创建的三个数据订阅目前的状态处于待审批

   ![](imgs/Inlong11.png)


4. 点击进入审批管理界面,对刚刚创建的三个数据订阅分别审批通过，随后回到数据订阅界面

   ```
   注：每个订阅需两次审批。
   ```
   
   ![](imgs/Inlong12.png)



### Pulsar 使用

#### 安装 JDK（以JDK8为例）

```
sudo apt install openjdk-8-jdk
```



#### 安装 Python 依赖

打开链接：https://github.com/wang273257881/inlong-pulsar-demo.git

拉至本地后运行：

```
cd inlong-pulsar-demo
pip install -r requirements.txt
```

#### Pulsar 消费

1. 查看topic信息

   ```
   [数据订阅]->[选择一消费组]->[详情]，记录Topic名称与消费组名称。

   注：在新建的三个数据订阅中选择其一即可。
   ```

   ![pulsar3](imgs/pulsar3.png)

2. 查看并修改 inlong-pulsar-demo/consumer.py

   ```
   将__main__中的函数参数改为第1步中记录的相应Topic名称与消费组名称。
   
   注：若与本示例保持一致，则无需修改。
   ```

   ![image-20230523184129384](imgs/image-20230523184129384.png)
   
3. 运行 inlong-pulsar-demo/consumer.py![code1](imgs/code1.png)

4. 将三个csv文件拷贝到 collect-data 路径下

   ```
   例：cp /path/to/inlong-pulsar-demo/data/train_internet.csv /path/to/apache-inlong-1.6.0/docker/docker-compose/collect-data/train_internet.csv
   ```
   
5. 观察运行状态，直至消息不再跳动

   ![code3](imgs/code3.png)

6. 停止运行，此时路径下应有相应文件，且在InLong中的相应数据流组的审计对账界面可以看到相关的数据传输曲线。

   ![image-20230601213525549](imgs/image-20230601213525549.png)

   ![image-20230531192012250](imgs/image-20230531192012250.png)

7. 按照不同数据流组对应的数据订阅、文件，修改参数并重复第2-6步，共3次。
