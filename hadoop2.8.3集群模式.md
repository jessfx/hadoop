# hadoop2.8.3集群模式

---

## 0. 前言

[hadoop2.8.3集群式配置官方文档](http://hadoop.apache.org/docs/r2.8.3/hadoop-project-dist/hadoop-common/ClusterSetup.html)

**硬件环境：虚拟机/实体机2台或以上**

**系统版本：ubuntu16.04**

---

## 1. 系统配置

### 1.1 配置固定ip (虚拟机需要)

配置方案示例
设备 | hostname | ip | gateway
---|---|---|---
设备1 | master | 192.168.10.5 | 192.168.10.5
设备2 | slave1 | 192.168.10.6 | 192.168.10.5
master
**/etc/network/interfaces**

    auto lo
    iface lo inet loopback
    auto ens33 #ens33为网卡名称，使用ifconfig查看网卡名称
    iface ens33 inet static
    address 192.168.10.5 #IP地址
    netmask 255.255.255.0
    gateway 192.168.10.2 #网关

slave1同理

测试能否互相ping通

### 1.2 修改hostname和hosts文件

**/etc/hostname**

    #slave1改为slave1
    master

**/etc/hosts**

    127.0.0.0 localhost
    192.168.10.5 master
    192.168.10.6 slave1

slave1上同理

### 1.3 关闭防火墙

    sudo service ufw stop
    sudo service iptables stop

---

## 2. 环境配置

参考hadoop2.8.3单机模式环境配置

---

## 3. ssh免密登录

### 3.1 安装ssh

    sudo apt-get install openssh-server

### 3.2 免密登录

生成密钥，将id_dsa.pub（公钥）追加到授权的key中

    ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
    cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

将认证文件复制到其它节点上

    scp ~/.ssh/authorized_keys slave1:~/.ssh/

测试

    ssh slave1

---

## 4. hadoop配置

### 4.1 配置文件

**core-site.xml,hdfs-site.xml,mapred-site.xml,yarn-site.xml**

参考hadoop2.8.3伪分布模式hadoop配置，所有localhost替换为master

**etc/hadoop/slaves**

    master
    slave1

### 4.2 复制文件

将master上的hadoop复制到其他节点

---

## 5. 启动hadoop集群

在
**master**
上执行命令

    start-all.sh
    mr-jobhistory-deamon.sh historyserver start

**验证**

master上输入`jps`,将输出以下内容

    NameNode
    SecondaryNameNode
    DataNode
    ResourceManager
    NodeManager
    JobHistory
    Jps

slave1上输入`jps`,将输出以下内容

    DataNode
    NodeManager
    Jps
