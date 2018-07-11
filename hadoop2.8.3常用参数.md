# hadoop2.8.3常用参数

jessfx

---

## 0. 前言

了解参数的作用能更好的帮助我们合理配置hadoop

## 1. core-site.xml

parameter|value|note
---|---|---
fs.defaultFS|NameNode URI|hdfs://host:port/
io.file.buffer.size|131072|sequencefile读写所使用的缓冲大小
hadoop.tmp.dir|tmp URI|hadoop临时目录

## 2. hdfs-site.xml

parameter|value|note
---|---|---
dfs.namenode.name.dir|存储NameNode命名空间和事务日志的本地文件系统路径|如果有多个地址，则存储会产生冗余
dfs.hosts|不启动DataNode的hosts名单|用于控制允许启动DataNodes
dfs.blocksize|268435456|HDFS blocksize 256MB 用于大型文件系统
dfs.namenode.handler.count|100|更多的NameNode服务器线程来处理DataNodes的rpc
dfs.datanode.data.dir|存储HDFS block的本地文件系统路径，可有多个路径|如果是多个目录，则数据会存储在所有命名目录中，通常位于不同的设备上

## 3. mapred-site.xml

** Mapreduce Application **
parameter|value|note
---|---|---
mapreduce.framework.name|yarn|设置mapreduce执行框架yarn
mapreduce.map.memory.mb|1536|一个map申请的内存大小，用于控制map数量
mapreduce.map.java.opts|-Xmx1024M|运行一个map的jvm堆大小
mapreduce.reduce.memory.mb|3072|一个reduce申请的内存大小
mapreduce.reduce.java.opts|-Xmx2560M|运行一个reduce的jvm堆大小
mapreduce.task.io.sort.mb||map环形缓存大小
mapreduce.task.io.sort.factor|100|排序时一次合并更多数据流
mapreduce.reduce.shuffle.parallelcopies|50|

** Mapreduce JobHistory Server **
parameter|value|note
---|---|---
mapreduce.jobhistory.address|host:port|默认端口10020
mapreduce.jobhistory.webapp.address|host:port|默认端口19888
mapreduce.jobhistory.intermediate-done-dir|mr-history/tmp|存放被MR任务写过的文件目录
mapreduce.jobhistory.done-dir|mr-history/done|MR历史服务器存放历史文件目录

## 4. yarn-site.xml

** ResourceManager和NodeManager共同参数 **
parameter|value|note
---|---|---
yarn.acl.enable|true/false|acl规则过滤
yarn.admin.acl|Admin ACL|用户acl规则过滤
yarn.log-aggregation-enable|false|log aggregation
** ResourceManager参数 **
parameter|value|note
---|---|---
yarn.resourcemanager.address|host:port|资源管理器地址
yarn.resourcemanager.scheduler.address|host:port|队列地址
yarn.resourcemanager.resource-tracker.address|host:port|资源追踪器地址
yarn.resourcemanager.admin.address|host:port|admin地址
yarn.resourcemanager.webapp.address|host:port|web-ui地址
yarn.resourcemanager.hosts|host:port|统一地址
yarn.resourcemanager.scheduler.class|Scheduler Class|指定资源分配器，CapacityScheduler，FairScheduler，FifoScheduler
yarn.scheduler.minimum-allocation-mb|任务队列的最小内存|单位MB
yarn.scheduler.maximum-allocation-mb|任务队列的最大内存|单位MB
yarn.resourcemanager.nodes.include-path/yarn.resourcemanager.exclude-path|使用中的NodeManagers/不使用的NodeManagers|用于控制资源总数

** NodeManager参数 **
parameter|value|note
---|---|---
yarn.nodemanager.memory-mb|NodeManager物理内存|决定了可用资源总量
yarn.nodemanager.vmem-pmem-ratio|任务虚拟内存和物理内存的最大比值|决定了每个任务最大虚拟内存，及最大虚拟内存
yarn.nodemanager.local-dirs|写入中间数据的本地文件系统目录|可有多个目录
yarn.nodemanager.logs-dirs|写入日子的本地文件系统目录|可有多个目录
yarn.nodemanager.log.retain-seconds|10800|retain文件超时时间，当log-aggregation关闭时生效
yarn.nodemanager.remote-app-log-dir|/logs|MR日志存放目录，当log-aggregation关闭时生效
yarn.nodemanager.remote-app-log-dir-suffix|logs|用户MR日志存放目录当log-aggregation关闭时生效
yarn.nodemanager.aux-services|mapreduce_shuffle|配置MR应用的shuffle服务
** HistoryServer参数 **
parameter|value|note
---|---|---
yarn.log-aggregation.retain-seconds|-1|
yarn.log-aggregation.retain-check-interval-seconds |-1|
