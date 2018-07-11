# hadoop2.8.3伪分布模式

jessfx

---

## 0. 前言

实践本文章前，请完成[hadoop2.8.3单机模式](https://github.com/jessfx/hadoop/blob/master/hadoop2.8.3%E5%8D%95%E6%9C%BA%E6%A8%A1%E5%BC%8F.md)

**系统版本：ubuntu16.04**

---

## 1. 环境配置

参考hadoop2.8.3单机模式环境配置

---

## 2. ssh免密登录

### 2.1 安装ssh

    sudo apt-get install openssh-server

### 2.2 免密登录

    ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
    cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
    chmod 0600 ~/.ssh/authorized_keys

---

## 3. hadoop配置

### 3.1 配置

执行命令

    mkdir $HADOOP_HOME/tmp
    mkdir $HADOOP_HOME/dfs
    mkdir $HADOOP_HOME/dfs/name
    mkdir $HADOOP_HOME/dfs/data

**bin/hadoop-evn.sh**
加入jdk环境变量

    export $JAVA_HOME=/opt/jvm/jdk1.8

**bin/yarn-evn.sh**
加入jdk环境变量

    export $JAVA_HOME=/opt/jvm/jdk1.8

**etc/hadoop/core-site.xml**

    <configuration>
        #指定hdfs访问端口
        <property>
            <name>fs.defaultFS</name>
            <value>hdfs://localhost:9000</value>
        </property>
        <property>
            <name>hadoop.tmp.dir</name>
            <value>file:/usr/local/hadoop/tmp</value>
        </property>
    </configuration>

**etc/hadoop/hdfs-site.xml**

    <configuration>
        #hdfs冗余度
        <property>
            <name>dfs.replication</name>
            <value>1</value>
        </property>
        <property>
            <name>dfs.namenode.secondary.http-address</name>
            <value>localhost:9001</value>
        </property>
        <property>
            <name>dfs.namenode.name.dir</name>
            <value>file:/usr/local/hadoop/dfs/name</value>
        </property>
        <property>
            <name>dfs.datanode.data.dir</name>
            <value>file:/usr/local/hadoop/dfs/data</value>
        </property>
        <property>
            <name>dfs.webhdfs.enabled</name>
            <value>true</value>
        </property>
        <property>
            <name>dfs.permission.enabled</name>
            <value>false</value>
        </property>
    </configuration>

#### yarn单点模式配置

**etc/hadoop/mapred-site.xml**

    <configuration>
        <property>
            <name>mapreduce.framework.name</name>
            <value>yarn</value>
        </property>
        <property>
            <name>mapreduce.jobhistory.address</name>
            <value>localhost:10020</value>
        </property>
        <property>
            <name>mapreduce.jobhistory.webapp.address</name>
            <value>localhost:19888</value>
        </property>
    </configuration>

**etc/hadoop/yarn-site.xml**

    <configuration>
        <property>
            <name>yarn.nodemanager.aux-services</name>
            <value>mapreduce_shuffle</value>
        </property>
        <property>
            <name>yarn.resourcemanager.address</name>
            <value>localhost:8032</value>
        </property>
        <property>
            <name>yarn.resourcemanager.scheduler.address</name>
            <value>localhost:8030</value>
        </property>
        <property>
            <name>yarn.resourcemanager.resource-tracker.address</name>
            <value>localhost:8031</value>
        </property>
        <property>
            <name>yarn.resourcemanager.admin.address</name>
            <value>localhost:8033</value>
        </property>
        <property>
            <name>yarn.resourcemanager.webapp.address</name>
            <value>localhost:8088</value>
        </property>
    </configuration>

更多参数配置参考hadoop2.8.3参数一览或[集群模式官方文档](http://hadoop.apache.org/docs/r2.8.3/hadoop-project-dist/hadoop-common/ClusterSetup.html)

### 3.2 格式化namenode

    hdfs namenode -format

---

## 4. 测试

执行命令

    start-all.sh

执行命令jps

输出以下内容

    NameNode
    Jps
    DataNode
    ResourceManager
    SecondaryNameNode
    NodeManager

到此伪分布式安装成功
