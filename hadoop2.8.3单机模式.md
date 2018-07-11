# hadoop2.8.3单机模式

jessfx

---

## 0.前言

**系统环境：ubuntu16.04**

[hadoop2.8.3官方文档](http://hadoop.apache.org/docs/)

---

## 1. 环境配置

### 1.1 系统全局变量配置

编辑/etc/profile

    #jdk解压目录
    export $JAVA_HOME=/opt/jvm/jdk1.8
    #hadoop解压目录
    export $HADOOP_HOME=/usr/local/hadoop
    export $PATH=$PATH:$JAVA_HOME/bin;$HADOOP_HOME/bin;$HADOOP_HOME/sbin;

执行命令，使系统变量生效

    source /etc/profile

### 1.2 jdk环境配置

####**推荐安装Oracle jdk**

[下载Oracle jdk](http://www.oracle.com/technetwork/java/javase/downloads/index.html)

解压到/opt/jvm下，并更改文件夹名字为jdk1.8

---

## 2. 单机模式hadoop安装

### 2.1 下载hadoop

[hadoop各版本下载地址](https://archive.apache.org/dist/hadoop/common/)

下载hadoop-2.8.3.tar.gz

### 2.2 解压hadoop

解压到/usr/local下，并改文件名为hadoop

#### 到此hadoop单机模式配置完成
