
# 准备工作——Docker

### 安装地址

- 前往docker安装地址https://docs.docker.com/engine/install/，选择对应的操作系统下载安装程序，安装

![](/Windows/docker_install.png)

### 可能存在的问题
-   在确认Hyper-V功能与CPU虚拟化均已开启的情况下，Docker启动时仍报错，信息如下


  `Check that hardware-assisted virtualization (either Intel VMX or AMD SVM) and Data Execution Prevention (sometimes labeled XD or Execute Disable or NX) are enabled in your BIOS.`

`Check your bootloader is configured to launch Hyper-V.`

`See https://docs.docker.com/desktop/windows/troubleshoot/#virtualization`

- 解决建议

  在确认Hyper-V功能与CPU虚拟化均已开启的情况下， 在管理员模式下的命令提示符中输入

  ```
  bcdedit /set hypervisorlaunchtype Auto
  ```

  并重启电脑 



# 准备工作——Flink

### 下载&解压
1. 下载压缩包
`https://www.apache.org/dyn/closer.lua/flink/flink-1.15.2/flink-1.15.2-bin-scala_2.12.tgz`
2. 前往下载路径解压
```
tar -xzf flink-1.15.2-bin-scala_2.12.tgz
```

### 创建bat文件

- 由于下载包中没有启动bat文件, 所以需要在安装文件夹的bin目录下创建对应bat文件(如何创
建bat文件有问题可自行搜索)，其中flink.bat用于执行task；

![](/Windows/flink.bat.jpg)

- start-cluster.bat用于启动批处理文件，文件具体内容参考上面链接。

![](/Windows/start-cluster.bat.png)

- 最后编辑conf/flink-conf.yaml文件，编辑具体内容参考上面链接。

![](/Windows/flink-conf.yaml.png)
- 关于文件修改的具体内容可以参考链接https://blog.csdn.net/guandongsheng110/article/details/118578189

### 启动

- 双击start-cluster.bat文件，期望结果会有两个执行框

- Flink启动后，会有一个WEB的管理页面，默认地址为：http://localhost:8081/.

  ![](/Windows/Flink.png)
  
### 可能存在的问题

- 由于运行flink必须要求java为8or11版本，与很多同学目前电脑上的java版本不同，因此我们需要
  在JAVA1.8与其他的JAVA版本中进行自由切换

- 解决建议
  
  - 安装好JDK8，安装地址：https://www.oracle.com/java/technologies/downloads/#java8
  - 对jdk环境变量进行如下设置:
  JAVA8_HOME =jdk1.8的安装路径根目录；
  JAVA17_HOME =jdk1.17的安装路径根目录；
  JAVA_HOME = %JAVA17_HOME% (注意:如果你想切换jdk版本，就在此处设置即可)
  - 根据使用需求切换不同jdk版本，直接修改JAVA_HOME = %JAVA17_HOME% 或 %JAVA8_HOME% (其他配置无需做任何修改)
  
  关于JAVA版本切换有其他问题可以参考链接：https://blog.csdn.net/YaoChung/article/details/126193635
  




# 在Windows上部署InLong

### 操作步骤（仅供参考）

1. 下载&解压InLong项目

- 前往inlong官网https://inlong.apache.org/zh-CN/docs/deployment/docker，
  在docker部署中获取 apache-inlong-[version]-bin.tar.gz，解压文件
  
   ![](/Windows/0.png)

2. 开启Docker
- 打开Docker Desktop：

   ![](/Windows/Docker.png)

3. 开启Flink
- 双击start-cluster.bat用于启动批处理文件，

  ![](/Windows/start_flink.png)
  
- 我们可以前往http://localhost:8081/进入Flink管理的默认地址：

   ![](/Windows/Flink.png)

4. 部署
- 方案一: 在inlong官网下载文件的终端输入以下代码，启动所有组件：

  cd docker/docker-compose
  
  docker-compose up -d
  
   ![](/Windows/arrange.png)
   
- 方案二: 如果部分同学通过方案一部署过程中，出现下载包时无限等待等情况无法完成部署，也可以加载已打包好的镜像完成部署。打包好的镜像地址如下，下载下来后放到docker/docker-compose文件夹下，
终端输入命令type images.tar | docker load.等待一阵子后重新输入命令docker-compose up -d
```
     链接：https://pan.baidu.com/s/1vMqmQ-lB2EHH_lkAjv4PBg 
     提取码：87vy
```
   
    

   ![](/Windows/4.png)

5. 使用
- 当所有容器都成功启动后，可以访问 http://localhost，并使用以下默认用户名admin，密码
inlong完成登录

   ![](/Windows/2.png)
  
- 登录完成后出现如下界面，完成InLong的部署

   ![](/Windows/3.png)