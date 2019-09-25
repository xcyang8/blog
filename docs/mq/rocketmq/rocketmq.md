<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**目录**

- [RocketMQ](#rocketmq)
  - [介绍](#%E4%BB%8B%E7%BB%8D)
  - [安装](#%E5%AE%89%E8%A3%85)
    - [docker环境下安装](#docker%E7%8E%AF%E5%A2%83%E4%B8%8B%E5%AE%89%E8%A3%85)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->


## RocketMQ

### 介绍


### 安装

#### docker环境下安装
1.搜索images rocketmq  
没有官方版本 使用foxiswho/rocketmq
```
docker search rocketmq

NAME                            DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
styletang/rocketmq-console-ng   rocketmq-console-ng                             17                                      
foxiswho/rocketmq               rocketmq                                        14                                      
rocketmqinc/rocketmq            Image repository for Apache RocketMQ            12                                      
laoyumi/rocketmq                                                                10                                      [OK]
xlxwhy/rocketmq                 alibaba's rocketmq                              4                                       
huanwei/rocketmq-broker                                                         2                                       
shanyehcne/rocketmq-console     rocketmq-console                                1                                       
coder4/rocketmq                 rocketmq                                        0                                       [OK]
rocketmqinc/rocketmq-operator   The Kubernetes operator for RocketMQ            0                                       
huanwei/rocketmq                                                                0                                       
2019liurui/rocketmq-operator    Kubernetes Operator for RocketMQ !              0                                       
slpcat/rocketmq-console-ng                                                      0                                       
2019liurui/rocketmq-broker      RocketMQ broker image for RocketMQ-Operator     0                                       
huanwei/rocketmq-broker-k8s                                                     0                                       
pengzu/rocketmq-console-ng      web console for rocketmq ,this code is from …   0                                       
king019/rocketmq                rocketmq                                        0                                       
fengzt/rocketmq-broker          apache rocketmq 4.2.0 broker server(官方文档…       0                                       
slpcat/rocketmq                                                                 0                                       
fengzt/rocketmq-nameserver      apache rocketmq 4.2.0 nameserver                0                                       
icyblazek/rocketmq              RocketMQ                                        0                                       
huanwei/rocketmq-operator                                                       0                                       
wanliqun/rocketmq-client                                                        0                                       
arilot/rocketmq-broker          RocketMQ broker build                           0                                       
ecarpo/rocketmq                                                                 0                                       
2019liurui/rocketmq-namesrv     RocketMQ name service image for RocketMQ-Ope…   0    
```

2.拉取server,broker,console 

```
docker pull rocketmqinc/rocketmq:server-4.3.2

docker pull rocketmqinc/rocketmq:broker-4.3.2

docker pull styletang/rocketmq-console-ng

```

3.启动nameserver

```
docker run -p 9876:9876 --name rmqserver -t  foxiswho/rocketmq:server-4.3.2
```

4.启动broke
>必须两个端口
```
docker run -p 10911:10911 -p 10909:10909 --link rmqserver:namesrv --name rmqbroker -e "NAMESRV_ADDR=namesrv:9876" -e "JAVA_OPTS=-Duser.home=/opt" -e "JAVA_OPT_EXT=-server -Xms128m -Xmx128m" -v /Users/mac/conf/rocket-mq-broke/broker.conf:/etc/rocketmq/broker.conf -t foxiswho/rocketmq:broker-4.3.2
```

5.启动console
```
docker run -d --name rmqconsole -p 9001:8080 --link rmqserver:namesrv -e "JAVA_OPTS=-Drocketmq.namesrv.addr=namesrv:9876 -Dcom.rocketmq.sendMessageWithVIPChannel=false" -t styletang/rocketmq-console-ng
```

6.常见异常
> 不支持外网访问
```
org.apache.rocketmq.remoting.exception.RemotingConnectException: connect to <172.17.0.3:10909> failed

```

解决办法：  
加载外部配置，增加配置项brokerIP1 = xxx.xxx.xxx.xxx指你的外网地址或者本机ip地址（127.0.0.1不行）


