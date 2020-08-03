# KafkaServer

## KafkaServer如何启动的

### IDE环境debugkafka启动过程

#### 启动zookeeper

在kafka目录下运行`./bin/zookeeper-server-start.sh config/zookeeper.properties`

#### IDE启动kafka server

编辑Run/Debug Configurations，添加一个Application类型的启动，Main class设置为kafka.Kafka。
VM options填写`-Dlog4j.configuration=/Users/liuzhengyang/Code/opensource/kafka/config/log4j.properties` 注意路径改为自己的
Program arguments填写config/server.properties文件的全路径
然后点击idea上的debug按钮启动

#### 实际启动流程

1. 创建zkClient, zk相关数据结构和使用方式后面介绍

2. 从zk的/cluster/id里读取clusterId，如果没有，尝试生成一个uuid作为clusterId，并尝试创建，如果创建成功返回，如果有其他进程同时创建了则再查询一次

kafka.server.KafkaServer#getOrGenerateClusterId

3. 读取`log.dirs`文件夹下读取meta.properties，作为BrokerMetadata取出clusterId，和zk上的clusterId对比，如果不相等，抛出异常。
这种一般出现在zk集群连错的情况

4. 从之前的meta.properties中获取或生成brokerId

5. 初始化dynamicConfig

config.dynamicConfig.initialize(zkClient)

6. 创建KafkaScheduler

7. 创建jmxReporter等监控组件

8. 创建LogManager

9. 创建MetadataCache

10. 创建SocketServer

11. 创建ReplicaManager

12. 创建KafkaController

13. 创建GroupCoordinator

14. 创建TransactionCoordinator

15. 创建FetchManager

16. 创建DynamicConfigManager

17. 标记当前已经启动完成

## Kafka如何处理send和consume请求

## Kafka是如何存储消息的

## Kafka的leader selection和rebalance是如何实现的

## Kafka的不同级别的消息可靠性是如何实现的

## Kafka是如何使用zookeeper的，如果不适用zk，该如何实现(raft协议)

## 其他

quota机制
isr如何维护
KafkaController是什么
transaction是什么，用在什么场景
stream是什么，用在什么场景