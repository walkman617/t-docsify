
# Angel 安装与配置

参考：https://github.com/YeHuanjie/start-on-Angel

## 更新apt源列表
```
sudo su
sudo apt-get update
```


## 安装JAVA8

```
apt install openjdk-8-jre-headless

#开放权限：enviroment 上传文件使用的文件夹
sudo chmod -R 777 enviroment

#下载java8安装包并上传到服务器中
scp jdk-8u281-linux-x64.tar.gz ubuntu@43.138.74.63:/home/enviroment

#解压
tar -zxvf jdk-8u281-linux-x64.tar.gz -C /usr/local/

#配置环境
sudo update-alternatives --config java

vim /etc/profile
#java8
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64/jre
export PATH=$PATH:$JAVA_HOME/bin

#激活配置
source /etc/profile
```

## 安装scala

下载scala2.13.0

wget https://downloads.lightbend.com/scala/2.13.0/scala-2.13.0.tgz 

解压

tar -zxvf scala-2.13.0.tgz -C /usr/local/

修改环境变量

vim /etc/profile

```shell
#scala2.13.0
export SCALA_HOME=/usr/local/scala-2.13.0
export PATH=$PATH:$SCALA_HOME/bin
```

激活

source /etc/profile

## 安装maven

下载

  ```
wget https://archive.apache.org/dist/maven/maven-3/3.3.9/binaries/apache-maven-3.3.9-bin.tar.gz
  ```

解压到 `/usr/local`

  ```
tar -zxvf apache-maven-3.3.9-bin.tar.gz -C /usr/local/
  ```

重命名 `maven-3.3.9`

  ```
cd /usr/local/
mv apache-maven-3.3.9 maven-3.3.9
  ```

配置环境

vim /etc/profile

```shell
#maven3.3.9
export MAVEN_HOME=/usr/local/maven-3.3.9/bin
export PATH=$PATH:$MAVEN_HOME
```

激活

source /etc/profile

JAVA_HOME environment variable is not set.

配置maven国内源

1找到linux下安装目录

```
which mvn
```

2修改在conf文件夹下的settings.xml

[maven (tencent.com)](https://mirrors.cloud.tencent.com/help/maven.html)



```
<mirror>
    <id>nexus-tencentyun</id>
    <mirrorOf>*</mirrorOf>
    <name>Nexus tencentyun</name>
    <url>http://mirrors.cloud.tencent.com/nexus/repository/maven-public/</url>
</mirror> 
```

## 安装 protobuf

下载

```
wget https://github.com/google/protobuf/releases/download/v2.5.0/protobuf-2.5.0.tar.gz
```

解压

```
tar -zxvf protobuf-2.5.0.tar.gz -C /usr/local/
```

编译protobuf2.5.0

```
cd /usr/local/protobuf-2.5.0
./autogen.sh
./configure
make
 # 如果没有克隆子模块，make check会失败但是可以继续make install,但是使用某些功能时可能会出错
make check
sudo make install
sudo ldconfig # refresh shared library cache.
```



添加到环境

vim /etc/profile



```shell
添加环境变量：
#protobuf2.5.0
export PROTOBUF_HOME=/usr/local/protobuf-2.5.0
export PATH=$PATH:/usr/local/protobuf/bin
```



## 下载 Angel

1. 在 environment 文件夹里下载

```
wget https://github.com/Angel-ML/angel/archive/refs/tags/Release-3.2.0.zip
```

2. 然后解压到当前文件夹

Release-3.2.0.zip

3. 使用 maven 构建应用

```
mvn clean package -Dmaven.test.skip=true
```

4. 运行测试用例

```
cd /home/enviroment/angel-Release-3.2.0/spark-on-angel/examples/src/main/scala/com/tencent/angel/spark/examples/local
/opt/modules/jdk1.8.0_281/bin/java -javaagent:/usr/local/IDEA/idea-IU-223.8617.56/lib/idea_rt.jar=41847:/usr/local/IDEA/idea-IU-223.8617.56/bin -Dfile.encoding=UTF-8 -classpath /opt/modules/jdk1.8.0_281/jre/lib/charsets.jar:/opt/modules/jdk1.8.0_281/jre/lib/deploy.jar:/opt/modules/jdk1.8.0_281/jre/lib/ext/cldrdata.jar:/opt/modules/jdk1.8.0_281/jre/lib/ext/dnsns.jar:/opt/modules/jdk1.8.0_281/jre/lib/ext/jaccess.jar:/opt/modules/jdk1.8.0_281/jre/lib/ext/jfxrt.jar:/opt/modules/jdk1.8.0_281/jre/lib/ext/localedata.jar:/opt/modules/jdk1.8.0_281/jre/lib/ext/nashorn.jar:/opt/modules/jdk1.8.0_281/jre/lib/ext/sunec.jar:/opt/modules/jdk1.8.0_281/jre/lib/ext/sunjce_provider.jar:/opt/modules/jdk1.8.0_281/jre/lib/ext/sunpkcs11.jar:/opt/modules/jdk1.8.0_281/jre/lib/ext/zipfs.jar:/opt/modules/jdk1.8.0_281/jre/lib/javaws.jar:/opt/modules/jdk1.8.0_281/jre/lib/jce.jar:/opt/modules/jdk1.8.0_281/jre/lib/jfr.jar:/opt/modules/jdk1.8.0_281/jre/lib/jfxswt.jar:/opt/modules/jdk1.8.0_281/jre/lib/jsse.jar:/opt/modules/jdk1.8.0_281/jre/lib/management-agent.jar:/opt/modules/jdk1.8.0_281/jre/lib/plugin.jar:/opt/modules/jdk1.8.0_281/jre/lib/resources.jar:/opt/modules/jdk1.8.0_281/jre/lib/rt.jar:/home/zzd/angel/angel-Release-3.2.0/spark-on-angel/examples/target/classes:/home/zzd/angel/angel-Release-3.2.0/spark-on-angel/core/target/classes:/home/zzd/angel/angel-Release-3.2.0/spark-on-angel/mllib/target/classes:/home/zzd/angel/angel-Release-3.2.0/spark-on-angel/graph/target/classes:/root/.m2/repository/edu/princeton/cs/algs4/1.0.3/algs4-1.0.3.jar:/home/zzd/angel/angel-Release-3.2.0/angel-ps/mllib/target/classes:/home/zzd/angel/angel-Release-3.2.0/angel-ps/tools/target/classes:/root/.m2/repository/com/yahoo/datasketches/sketches-core/0.8.1/sketches-core-0.8.1.jar:/root/.m2/repository/com/yahoo/datasketches/memory/0.8.1/memory-0.8.1.jar:/root/.m2/repository/org/ehcache/sizeof/0.3.0/sizeof-0.3.0.jar:/root/.m2/repository/org/slf4j/slf4j-api/1.6.2/slf4j-api-1.6.2.jar:/root/.m2/repository/com/esotericsoftware/kryo-shaded/4.0.0/kryo-shaded-4.0.0.jar:/root/.m2/repository/com/esotericsoftware/minlog/1.3.0/minlog-1.3.0.jar:/root/.m2/repository/org/objenesis/objenesis/2.2/objenesis-2.2.jar:/root/.m2/repository/de/javakaffee/kryo-serializers/0.42/kryo-serializers-0.42.jar:/root/.m2/repository/com/esotericsoftware/kryo/4.0.0/kryo-4.0.0.jar:/root/.m2/repository/com/esotericsoftware/reflectasm/1.11.3/reflectasm-1.11.3.jar:/root/.m2/repository/org/ow2/asm/asm/5.0.4/asm-5.0.4.jar:/root/.m2/repository/org/slf4j/slf4j-log4j12/1.6.2/slf4j-log4j12-1.6.2.jar:/root/.m2/repository/commons-pool/commons-pool/1.6/commons-pool-1.6.jar:/root/.m2/repository/org/json4s/json4s-jackson_2.11/3.5.3/json4s-jackson_2.11-3.5.3.jar:/root/.m2/repository/org/json4s/json4s-core_2.11/3.5.3/json4s-core_2.11-3.5.3.jar:/root/.m2/repository/org/json4s/json4s-ast_2.11/3.5.3/json4s-ast_2.11-3.5.3.jar:/root/.m2/repository/org/json4s/json4s-scalap_2.11/3.5.3/json4s-scalap_2.11-3.5.3.jar:/root/.m2/repository/org/apache/commons/commons-math/2.2/commons-math-2.2.jar:/root/.m2/repository/com/google/protobuf/protobuf-java/2.5.0/protobuf-java-2.5.0.jar:/root/.m2/repository/org/cloudera/htrace/htrace-core/2.05/htrace-core-2.05.jar:/root/.m2/repository/com/google/guava/guava/12.0.1/guava-12.0.1.jar:/root/.m2/repository/commons-logging/commons-logging/1.1.1/commons-logging-1.1.1.jar:/root/.m2/repository/org/mortbay/jetty/jetty-util/6.1.26/jetty-util-6.1.26.jar:/root/.m2/repository/io/netty/netty-all/4.1.42.Final/netty-all-4.1.42.Final.jar:/root/.m2/repository/it/unimi/dsi/fastutil/7.1.0/fastutil-7.1.0.jar:/root/.m2/repository/net/agkn/hll/1.6.0/hll-1.6.0.jar:/root/.m2/repository/org/apache/hadoop/hadoop-common/2.7.3/hadoop-common-2.7.3.jar:/root/.m2/repository/org/apache/hadoop/hadoop-annotations/2.7.3/hadoop-annotations-2.7.3.jar:/opt/modules/jdk1.8.0_281/lib/tools.jar:/root/.m2/repository/commons-cli/commons-cli/1.2/commons-cli-1.2.jar:/root/.m2/repository/xmlenc/xmlenc/0.52/xmlenc-0.52.jar:/root/.m2/repository/commons-httpclient/commons-httpclient/3.1/commons-httpclient-3.1.jar:/root/.m2/repository/commons-codec/commons-codec/1.4/commons-codec-1.4.jar:/root/.m2/repository/commons-io/commons-io/2.4/commons-io-2.4.jar:/root/.m2/repository/commons-collections/commons-collections/3.2.2/commons-collections-3.2.2.jar:/root/.m2/repository/javax/servlet/servlet-api/2.5/servlet-api-2.5.jar:/root/.m2/repository/org/mortbay/jetty/jetty/6.1.26/jetty-6.1.26.jar:/root/.m2/repository/javax/servlet/jsp/jsp-api/2.1/jsp-api-2.1.jar:/root/.m2/repository/com/sun/jersey/jersey-core/1.9/jersey-core-1.9.jar:/root/.m2/repository/com/sun/jersey/jersey-json/1.9/jersey-json-1.9.jar:/root/.m2/repository/com/sun/xml/bind/jaxb-impl/2.2.3-1/jaxb-impl-2.2.3-1.jar:/root/.m2/repository/com/sun/jersey/jersey-server/1.9/jersey-server-1.9.jar:/root/.m2/repository/asm/asm/3.1/asm-3.1.jar:/root/.m2/repository/net/java/dev/jets3t/jets3t/0.9.0/jets3t-0.9.0.jar:/root/.m2/repository/org/apache/httpcomponents/httpcore/4.1.2/httpcore-4.1.2.jar:/root/.m2/repository/com/jamesmurty/utils/java-xmlbuilder/0.4/java-xmlbuilder-0.4.jar:/root/.m2/repository/commons-lang/commons-lang/2.6/commons-lang-2.6.jar:/root/.m2/repository/commons-configuration/commons-configuration/1.6/commons-configuration-1.6.jar:/root/.m2/repository/commons-digester/commons-digester/1.8/commons-digester-1.8.jar:/root/.m2/repository/commons-beanutils/commons-beanutils/1.7.0/commons-beanutils-1.7.0.jar:/root/.m2/repository/commons-beanutils/commons-beanutils-core/1.8.0/commons-beanutils-core-1.8.0.jar:/root/.m2/repository/org/codehaus/jackson/jackson-core-asl/1.9.13/jackson-core-asl-1.9.13.jar:/root/.m2/repository/org/codehaus/jackson/jackson-mapper-asl/1.9.13/jackson-mapper-asl-1.9.13.jar:/root/.m2/repository/com/jcraft/jsch/0.1.42/jsch-0.1.42.jar:/root/.m2/repository/org/apache/curator/curator-client/2.7.1/curator-client-2.7.1.jar:/root/.m2/repository/org/apache/htrace/htrace-core/3.1.0-incubating/htrace-core-3.1.0-incubating.jar:/root/.m2/repository/org/apache/commons/commons-compress/1.4.1/commons-compress-1.4.1.jar:/root/.m2/repository/org/apache/hadoop/hadoop-auth/2.7.3/hadoop-auth-2.7.3.jar:/root/.m2/repository/org/apache/httpcomponents/httpclient/4.2.5/httpclient-4.2.5.jar:/root/.m2/repository/org/apache/directory/server/apacheds-kerberos-codec/2.0.0-M15/apacheds-kerberos-codec-2.0.0-M15.jar:/root/.m2/repository/org/apache/directory/server/apacheds-i18n/2.0.0-M15/apacheds-i18n-2.0.0-M15.jar:/root/.m2/repository/org/apache/directory/api/api-asn1-api/1.0.0-M20/api-asn1-api-1.0.0-M20.jar:/root/.m2/repository/org/apache/directory/api/api-util/1.0.0-M20/api-util-1.0.0-M20.jar:/root/.m2/repository/org/apache/curator/curator-framework/2.7.1/curator-framework-2.7.1.jar:/root/.m2/repository/org/apache/hadoop/hadoop-yarn-api/2.7.3/hadoop-yarn-api-2.7.3.jar:/root/.m2/repository/org/apache/hadoop/hadoop-yarn-applications-distributedshell/2.7.3/hadoop-yarn-applications-distributedshell-2.7.3.jar:/root/.m2/repository/org/apache/hadoop/hadoop-yarn-applications-unmanaged-am-launcher/2.7.3/hadoop-yarn-applications-unmanaged-am-launcher-2.7.3.jar:/root/.m2/repository/org/apache/hadoop/hadoop-yarn-client/2.7.3/hadoop-yarn-client-2.7.3.jar:/root/.m2/repository/org/apache/hadoop/hadoop-yarn-common/2.7.3/hadoop-yarn-common-2.7.3.jar:/root/.m2/repository/javax/xml/bind/jaxb-api/2.2.2/jaxb-api-2.2.2.jar:/root/.m2/repository/javax/xml/stream/stax-api/1.0-2/stax-api-1.0-2.jar:/root/.m2/repository/com/sun/jersey/jersey-client/1.9/jersey-client-1.9.jar:/root/.m2/repository/org/codehaus/jackson/jackson-jaxrs/1.9.13/jackson-jaxrs-1.9.13.jar:/root/.m2/repository/org/codehaus/jackson/jackson-xc/1.9.13/jackson-xc-1.9.13.jar:/root/.m2/repository/com/google/inject/extensions/guice-servlet/3.0/guice-servlet-3.0.jar:/root/.m2/repository/com/google/inject/guice/3.0/guice-3.0.jar:/root/.m2/repository/javax/inject/javax.inject/1/javax.inject-1.jar:/root/.m2/repository/aopalliance/aopalliance/1.0/aopalliance-1.0.jar:/root/.m2/repository/com/sun/jersey/contribs/jersey-guice/1.9/jersey-guice-1.9.jar:/root/.m2/repository/org/apache/hadoop/hadoop-yarn-server-common/2.7.3/hadoop-yarn-server-common-2.7.3.jar:/root/.m2/repository/org/fusesource/leveldbjni/leveldbjni-all/1.8/leveldbjni-all-1.8.jar:/root/.m2/repository/org/apache/hadoop/hadoop-yarn-server-nodemanager/2.7.3/hadoop-yarn-server-nodemanager-2.7.3.jar:/root/.m2/repository/org/codehaus/jettison/jettison/1.1/jettison-1.1.jar:/root/.m2/repository/org/apache/hadoop/hadoop-yarn-server-resourcemanager/2.7.3/hadoop-yarn-server-resourcemanager-2.7.3.jar:/root/.m2/repository/org/apache/hadoop/hadoop-yarn-server-applicationhistoryservice/2.7.3/hadoop-yarn-server-applicationhistoryservice-2.7.3.jar:/root/.m2/repository/org/apache/zookeeper/zookeeper/3.4.6/zookeeper-3.4.6-tests.jar:/root/.m2/repository/org/apache/hadoop/hadoop-yarn-server-web-proxy/2.7.3/hadoop-yarn-server-web-proxy-2.7.3.jar:/root/.m2/repository/org/apache/hadoop/hadoop-hdfs/2.7.3/hadoop-hdfs-2.7.3.jar:/root/.m2/repository/commons-daemon/commons-daemon/1.0.13/commons-daemon-1.0.13.jar:/root/.m2/repository/io/netty/netty/3.6.2.Final/netty-3.6.2.Final.jar:/root/.m2/repository/xerces/xercesImpl/2.9.1/xercesImpl-2.9.1.jar:/root/.m2/repository/xml-apis/xml-apis/1.3.04/xml-apis-1.3.04.jar:/root/.m2/repository/org/apache/hadoop/hadoop-hdfs-nfs/2.7.3/hadoop-hdfs-nfs-2.7.3.jar:/root/.m2/repository/org/apache/hadoop/hadoop-mapreduce-client-app/2.7.3/hadoop-mapreduce-client-app-2.7.3.jar:/root/.m2/repository/org/apache/hadoop/hadoop-mapreduce-client-shuffle/2.7.3/hadoop-mapreduce-client-shuffle-2.7.3.jar:/root/.m2/repository/org/apache/hadoop/hadoop-mapreduce-client-common/2.7.3/hadoop-mapreduce-client-common-2.7.3.jar:/root/.m2/repository/org/apache/hadoop/hadoop-mapreduce-client-core/2.7.3/hadoop-mapreduce-client-core-2.7.3.jar:/root/.m2/repository/org/apache/hadoop/hadoop-mapreduce-client-hs/2.7.3/hadoop-mapreduce-client-hs-2.7.3.jar:/root/.m2/repository/org/apache/hadoop/hadoop-mapreduce-client-hs-plugins/2.7.3/hadoop-mapreduce-client-hs-plugins-2.7.3.jar:/root/.m2/repository/org/apache/hadoop/hadoop-mapreduce-client-jobclient/2.7.3/hadoop-mapreduce-client-jobclient-2.7.3.jar:/root/.m2/repository/org/scala-lang/scala-library/2.11.8/scala-library-2.11.8.jar:/root/.m2/repository/org/scalanlp/breeze_2.11/0.13/breeze_2.11-0.13.jar:/root/.m2/repository/org/scalanlp/breeze-macros_2.11/0.13/breeze-macros_2.11-0.13.jar:/root/.m2/repository/com/github/fommil/netlib/core/1.1.2/core-1.1.2.jar:/root/.m2/repository/net/sourceforge/f2j/arpack_combined_all/0.1/arpack_combined_all-0.1.jar:/root/.m2/repository/net/sf/opencsv/opencsv/2.3/opencsv-2.3.jar:/root/.m2/repository/com/github/rwl/jtransforms/2.4.0/jtransforms-2.4.0.jar:/root/.m2/repository/junit/junit/4.8.2/junit-4.8.2.jar:/root/.m2/repository/org/spire-math/spire_2.11/0.13.0/spire_2.11-0.13.0.jar:/root/.m2/repository/org/spire-math/spire-macros_2.11/0.13.0/spire-macros_2.11-0.13.0.jar:/root/.m2/repository/org/typelevel/machinist_2.11/0.6.1/machinist_2.11-0.6.1.jar:/root/.m2/repository/com/chuusai/shapeless_2.11/2.3.2/shapeless_2.11-2.3.2.jar:/root/.m2/repository/org/typelevel/macro-compat_2.11/1.1.1/macro-compat_2.11-1.1.1.jar:/root/.m2/repository/net/sf/py4j/py4j/0.10.4/py4j-0.10.4.jar:/root/.m2/repository/com/google/code/gson/gson/2.8.5/gson-2.8.5.jar:/root/.m2/repository/org/apache/commons/commons-math3/3.6.1/commons-math3-3.6.1.jar:/root/.m2/repository/org/apache/spark/spark-core_2.11/2.4.0/spark-core_2.11-2.4.0.jar:/root/.m2/repository/org/apache/avro/avro/1.8.2/avro-1.8.2.jar:/root/.m2/repository/com/thoughtworks/paranamer/paranamer/2.7/paranamer-2.7.jar:/root/.m2/repository/org/tukaani/xz/1.5/xz-1.5.jar:/root/.m2/repository/org/apache/avro/avro-mapred/1.8.2/avro-mapred-1.8.2-hadoop2.jar:/root/.m2/repository/org/apache/avro/avro-ipc/1.8.2/avro-ipc-1.8.2.jar:/root/.m2/repository/com/twitter/chill_2.11/0.9.3/chill_2.11-0.9.3.jar:/root/.m2/repository/com/twitter/chill-java/0.9.3/chill-java-0.9.3.jar:/root/.m2/repository/org/apache/xbean/xbean-asm6-shaded/4.8/xbean-asm6-shaded-4.8.jar:/root/.m2/repository/org/apache/hadoop/hadoop-client/2.6.5/hadoop-client-2.6.5.jar:/root/.m2/repository/org/apache/spark/spark-launcher_2.11/2.4.0/spark-launcher_2.11-2.4.0.jar:/root/.m2/repository/org/apache/spark/spark-kvstore_2.11/2.4.0/spark-kvstore_2.11-2.4.0.jar:/root/.m2/repository/com/fasterxml/jackson/core/jackson-core/2.6.7/jackson-core-2.6.7.jar:/root/.m2/repository/com/fasterxml/jackson/core/jackson-annotations/2.6.7/jackson-annotations-2.6.7.jar:/root/.m2/repository/org/apache/spark/spark-network-common_2.11/2.4.0/spark-network-common_2.11-2.4.0.jar:/root/.m2/repository/org/apache/spark/spark-network-shuffle_2.11/2.4.0/spark-network-shuffle_2.11-2.4.0.jar:/root/.m2/repository/org/apache/spark/spark-unsafe_2.11/2.4.0/spark-unsafe_2.11-2.4.0.jar:/root/.m2/repository/javax/activation/activation/1.1.1/activation-1.1.1.jar:/root/.m2/repository/org/apache/curator/curator-recipes/2.6.0/curator-recipes-2.6.0.jar:/root/.m2/repository/org/apache/zookeeper/zookeeper/3.4.6/zookeeper-3.4.6.jar:/root/.m2/repository/javax/servlet/javax.servlet-api/3.1.0/javax.servlet-api-3.1.0.jar:/root/.m2/repository/org/apache/commons/commons-lang3/3.5/commons-lang3-3.5.jar:/root/.m2/repository/com/google/code/findbugs/jsr305/1.3.9/jsr305-1.3.9.jar:/root/.m2/repository/org/slf4j/jul-to-slf4j/1.7.16/jul-to-slf4j-1.7.16.jar:/root/.m2/repository/org/slf4j/jcl-over-slf4j/1.7.16/jcl-over-slf4j-1.7.16.jar:/root/.m2/repository/log4j/log4j/1.2.17/log4j-1.2.17.jar:/root/.m2/repository/com/ning/compress-lzf/1.0.3/compress-lzf-1.0.3.jar:/root/.m2/repository/org/xerial/snappy/snappy-java/1.1.7.1/snappy-java-1.1.7.1.jar:/root/.m2/repository/org/lz4/lz4-java/1.4.0/lz4-java-1.4.0.jar:/root/.m2/repository/com/github/luben/zstd-jni/1.3.2-2/zstd-jni-1.3.2-2.jar:/root/.m2/repository/org/roaringbitmap/RoaringBitmap/0.5.11/RoaringBitmap-0.5.11.jar:/root/.m2/repository/commons-net/commons-net/3.1/commons-net-3.1.jar:/root/.m2/repository/org/glassfish/jersey/core/jersey-client/2.22.2/jersey-client-2.22.2.jar:/root/.m2/repository/javax/ws/rs/javax.ws.rs-api/2.0.1/javax.ws.rs-api-2.0.1.jar:/root/.m2/repository/org/glassfish/hk2/hk2-api/2.4.0-b34/hk2-api-2.4.0-b34.jar:/root/.m2/repository/org/glassfish/hk2/hk2-utils/2.4.0-b34/hk2-utils-2.4.0-b34.jar:/root/.m2/repository/org/glassfish/hk2/external/aopalliance-repackaged/2.4.0-b34/aopalliance-repackaged-2.4.0-b34.jar:/root/.m2/repository/org/glassfish/hk2/external/javax.inject/2.4.0-b34/javax.inject-2.4.0-b34.jar:/root/.m2/repository/org/glassfish/hk2/hk2-locator/2.4.0-b34/hk2-locator-2.4.0-b34.jar:/root/.m2/repository/org/javassist/javassist/3.18.1-GA/javassist-3.18.1-GA.jar:/root/.m2/repository/org/glassfish/jersey/core/jersey-common/2.22.2/jersey-common-2.22.2.jar:/root/.m2/repository/javax/annotation/javax.annotation-api/1.2/javax.annotation-api-1.2.jar:/root/.m2/repository/org/glassfish/jersey/bundles/repackaged/jersey-guava/2.22.2/jersey-guava-2.22.2.jar:/root/.m2/repository/org/glassfish/hk2/osgi-resource-locator/1.0.1/osgi-resource-locator-1.0.1.jar:/root/.m2/repository/org/glassfish/jersey/core/jersey-server/2.22.2/jersey-server-2.22.2.jar:/root/.m2/repository/org/glassfish/jersey/media/jersey-media-jaxb/2.22.2/jersey-media-jaxb-2.22.2.jar:/root/.m2/repository/javax/validation/validation-api/1.1.0.Final/validation-api-1.1.0.Final.jar:/root/.m2/repository/org/glassfish/jersey/containers/jersey-container-servlet/2.22.2/jersey-container-servlet-2.22.2.jar:/root/.m2/repository/org/glassfish/jersey/containers/jersey-container-servlet-core/2.22.2/jersey-container-servlet-core-2.22.2.jar:/root/.m2/repository/com/clearspring/analytics/stream/2.7.0/stream-2.7.0.jar:/root/.m2/repository/io/dropwizard/metrics/metrics-core/3.1.5/metrics-core-3.1.5.jar:/root/.m2/repository/io/dropwizard/metrics/metrics-jvm/3.1.5/metrics-jvm-3.1.5.jar:/root/.m2/repository/io/dropwizard/metrics/metrics-json/3.1.5/metrics-json-3.1.5.jar:/root/.m2/repository/io/dropwizard/metrics/metrics-graphite/3.1.5/metrics-graphite-3.1.5.jar:/root/.m2/repository/com/fasterxml/jackson/core/jackson-databind/2.6.7.1/jackson-databind-2.6.7.1.jar:/root/.m2/repository/com/fasterxml/jackson/module/jackson-module-scala_2.11/2.6.7.1/jackson-module-scala_2.11-2.6.7.1.jar:/root/.m2/repository/com/fasterxml/jackson/module/jackson-module-paranamer/2.7.9/jackson-module-paranamer-2.7.9.jar:/root/.m2/repository/org/apache/ivy/ivy/2.4.0/ivy-2.4.0.jar:/root/.m2/repository/oro/oro/2.0.8/oro-2.0.8.jar:/root/.m2/repository/net/razorvine/pyrolite/4.13/pyrolite-4.13.jar:/root/.m2/repository/org/apache/spark/spark-tags_2.11/2.4.0/spark-tags_2.11-2.4.0.jar:/root/.m2/repository/org/apache/commons/commons-crypto/1.0.0/commons-crypto-1.0.0.jar:/root/.m2/repository/org/spark-project/spark/unused/1.0.0/unused-1.0.0.jar:/root/.m2/repository/org/apache/spark/spark-mllib_2.11/2.4.0/spark-mllib_2.11-2.4.0.jar:/root/.m2/repository/org/scala-lang/modules/scala-parser-combinators_2.11/1.1.0/scala-parser-combinators_2.11-1.1.0.jar:/root/.m2/repository/org/apache/spark/spark-streaming_2.11/2.4.0/spark-streaming_2.11-2.4.0.jar:/root/.m2/repository/org/apache/spark/spark-sql_2.11/2.4.0/spark-sql_2.11-2.4.0.jar:/root/.m2/repository/com/univocity/univocity-parsers/2.7.3/univocity-parsers-2.7.3.jar:/root/.m2/repository/org/apache/spark/spark-sketch_2.11/2.4.0/spark-sketch_2.11-2.4.0.jar:/root/.m2/repository/org/apache/spark/spark-catalyst_2.11/2.4.0/spark-catalyst_2.11-2.4.0.jar:/root/.m2/repository/org/codehaus/janino/janino/3.0.9/janino-3.0.9.jar:/root/.m2/repository/org/codehaus/janino/commons-compiler/3.0.9/commons-compiler-3.0.9.jar:/root/.m2/repository/org/antlr/antlr4-runtime/4.7/antlr4-runtime-4.7.jar:/root/.m2/repository/org/apache/orc/orc-core/1.5.2/orc-core-1.5.2-nohive.jar:/root/.m2/repository/org/apache/orc/orc-shims/1.5.2/orc-shims-1.5.2.jar:/root/.m2/repository/io/airlift/aircompressor/0.10/aircompressor-0.10.jar:/root/.m2/repository/org/apache/orc/orc-mapreduce/1.5.2/orc-mapreduce-1.5.2-nohive.jar:/root/.m2/repository/org/apache/parquet/parquet-column/1.10.0/parquet-column-1.10.0.jar:/root/.m2/repository/org/apache/parquet/parquet-common/1.10.0/parquet-common-1.10.0.jar:/root/.m2/repository/org/apache/parquet/parquet-encoding/1.10.0/parquet-encoding-1.10.0.jar:/root/.m2/repository/org/apache/parquet/parquet-hadoop/1.10.0/parquet-hadoop-1.10.0.jar:/root/.m2/repository/org/apache/parquet/parquet-format/2.4.0/parquet-format-2.4.0.jar:/root/.m2/repository/org/apache/parquet/parquet-jackson/1.10.0/parquet-jackson-1.10.0.jar:/root/.m2/repository/org/apache/arrow/arrow-vector/0.10.0/arrow-vector-0.10.0.jar:/root/.m2/repository/org/apache/arrow/arrow-format/0.10.0/arrow-format-0.10.0.jar:/root/.m2/repository/org/apache/arrow/arrow-memory/0.10.0/arrow-memory-0.10.0.jar:/root/.m2/repository/joda-time/joda-time/2.9.9/joda-time-2.9.9.jar:/root/.m2/repository/com/carrotsearch/hppc/0.7.2/hppc-0.7.2.jar:/root/.m2/repository/com/vlkan/flatbuffers/1.2.0-3f79e055/flatbuffers-1.2.0-3f79e055.jar:/root/.m2/repository/org/apache/spark/spark-graphx_2.11/2.4.0/spark-graphx_2.11-2.4.0.jar:/root/.m2/repository/org/apache/spark/spark-mllib-local_2.11/2.4.0/spark-mllib-local_2.11-2.4.0.jar:/root/.m2/repository/org/apache/spark/spark-streaming-kafka-0-8_2.11/2.4.0/spark-streaming-kafka-0-8_2.11-2.4.0.jar:/root/.m2/repository/org/apache/kafka/kafka_2.11/0.8.2.1/kafka_2.11-0.8.2.1.jar:/root/.m2/repository/com/yammer/metrics/metrics-core/2.2.0/metrics-core-2.2.0.jar:/root/.m2/repository/com/101tec/zkclient/0.3/zkclient-0.3.jar:/root/.m2/repository/org/apache/kafka/kafka-clients/0.8.2.1/kafka-clients-0.8.2.1.jar:/root/.m2/repository/net/jpountz/lz4/lz4/1.2.0/lz4-1.2.0.jar:/home/zzd/angel/angel-Release-3.2.0/angel-ps/core/target/classes:/root/.m2/repository/org/apache/velocity/velocity/1.7/velocity-1.7.jar:/root/.m2/repository/com/github/fommil/netlib/netlib-native_ref-osx-x86_64/1.1/netlib-native_ref-osx-x86_64-1.1-natives.jar:/root/.m2/repository/com/github/fommil/netlib/native_ref-java/1.1/native_ref-java-1.1.jar:/root/.m2/repository/com/github/fommil/jniloader/1.1/jniloader-1.1.jar:/root/.m2/repository/com/github/fommil/netlib/netlib-native_ref-linux-x86_64/1.1/netlib-native_ref-linux-x86_64-1.1-natives.jar:/root/.m2/repository/com/github/fommil/netlib/netlib-native_ref-linux-i686/1.1/netlib-native_ref-linux-i686-1.1-natives.jar:/root/.m2/repository/com/github/fommil/netlib/netlib-native_ref-win-x86_64/1.1/netlib-native_ref-win-x86_64-1.1-natives.jar:/root/.m2/repository/com/github/fommil/netlib/netlib-native_ref-win-i686/1.1/netlib-native_ref-win-i686-1.1-natives.jar:/root/.m2/repository/com/github/fommil/netlib/netlib-native_ref-linux-armhf/1.1/netlib-native_ref-linux-armhf-1.1-natives.jar:/root/.m2/repository/com/github/fommil/netlib/netlib-native_system-osx-x86_64/1.1/netlib-native_system-osx-x86_64-1.1-natives.jar:/root/.m2/repository/com/github/fommil/netlib/native_system-java/1.1/native_system-java-1.1.jar:/root/.m2/repository/com/github/fommil/netlib/netlib-native_system-linux-x86_64/1.1/netlib-native_system-linux-x86_64-1.1-natives.jar:/root/.m2/repository/com/github/fommil/netlib/netlib-native_system-linux-i686/1.1/netlib-native_system-linux-i686-1.1-natives.jar:/root/.m2/repository/com/github/fommil/netlib/netlib-native_system-linux-armhf/1.1/netlib-native_system-linux-armhf-1.1-natives.jar:/root/.m2/repository/com/github/fommil/netlib/netlib-native_system-win-x86_64/1.1/netlib-native_system-win-x86_64-1.1-natives.jar:/root/.m2/repository/com/github/fommil/netlib/netlib-native_system-win-i686/1.1/netlib-native_system-win-i686-1.1-natives.jar:/root/.m2/repository/io/fabric8/kubernetes-client/3.0.0/kubernetes-client-3.0.0.jar:/root/.m2/repository/io/fabric8/kubernetes-model/2.0.0/kubernetes-model-2.0.0.jar:/root/.m2/repository/com/fasterxml/jackson/module/jackson-module-jaxb-annotations/2.7.5/jackson-module-jaxb-annotations-2.7.5.jar:/root/.m2/repository/com/squareup/okhttp3/okhttp/3.8.1/okhttp-3.8.1.jar:/root/.m2/repository/com/squareup/okio/okio/1.13.0/okio-1.13.0.jar:/root/.m2/repository/com/squareup/okhttp3/logging-interceptor/3.8.1/logging-interceptor-3.8.1.jar:/root/.m2/repository/com/fasterxml/jackson/dataformat/jackson-dataformat-yaml/2.7.7/jackson-dataformat-yaml-2.7.7.jar:/root/.m2/repository/org/yaml/snakeyaml/1.15/snakeyaml-1.15.jar:/root/.m2/repository/io/fabric8/zjsonpatch/0.3.0/zjsonpatch-0.3.0.jar:/root/.m2/repository/com/github/mifmif/generex/1.0.1/generex-1.0.1.jar:/root/.m2/repository/dk/brics/automaton/automaton/1.11-8/automaton-1.11-8.jar:/home/zzd/angel/angel-Release-3.2.0/angel-ps/graph/target/classes:/home/zzd/angel/angel-Release-3.2.0/angel-ps/psf/target/classes:/root/.m2/repository/org/scala-lang/scala-reflect/2.11.7/scala-reflect-2.11.7.jar:/root/.m2/repository/org/scala-lang/modules/scala-xml_2.11/1.0.2/scala-xml_2.11-1.0.2.jar com.tencent.angel.spark.examples.local.DeepWalkExample
```
