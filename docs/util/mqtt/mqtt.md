<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**目录**

- [简介](#%E7%AE%80%E4%BB%8B)
- [客户端组件](#%E5%AE%A2%E6%88%B7%E7%AB%AF%E7%BB%84%E4%BB%B6)
  - [demo](#demo)
- [服务端组件](#%E6%9C%8D%E5%8A%A1%E7%AB%AF%E7%BB%84%E4%BB%B6)
  - [简介](#%E7%AE%80%E4%BB%8B-1)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->


### 简介
MQTT是一个客户端服务端架构的发布/订阅模式的消息传输协议。它的设计思想是轻巧、开放、简单、规范，易于实现。这些特点使得它对很多场景来说都是很好的选择，特别是对于受限的环境如机器与机器的通信（M2M）以及物联网环境（IoT）  
[MQ协议TT协议](https://mcxiaoke.gitbooks.io/mqtt-cn/content/)

### 客户端组件
 Eclipse Paho：是Eclipse提供的一个访问MQTT服务器的一种开源客户端库。
 
 注意： 
 > QoS：服务质量，其作用是当网络过载或拥塞时，QoS 能确保重要业务量不受延迟，同时保证网络的高效运行  
 > - 服务质量0 - 表示一条消息最多应该发送一次（零次或一次）。该消息不会持久保存到磁盘，并且不会通过网络进行确认。这种QoS是最快的，但只能用于无价值的消息  
 > - 服务质量1 - 表示一条消息应至少传递一次（一次或多次）。该消息只能在持久存在的情况下才能被安全地传递，因此应用程序必须提供一种持久化方法。如果未指定持久性机制，则在发生客户端故障时不会传递消息。该消息将通过网络得到确认。这是默认的QoS。  
 > - 服务质量2 - 表示应该传递一次消息。该消息将被保存到磁盘，并且将受到整个网络的两阶段确认。该消息只能在持久存在的情况下才能被安全地传递，因此应用程序必须提供一种持久化方法。如果未指定持久性机制，则在发生客户端故障时不会传递消息。  
 >
 >
 >  如果未配置持久性，则在发生网络或服务器问题时，仍会传送QoS 1和2消息，因为客户端将在内存中保持状态。如果MQTT客户端关闭或失败，并且未配置持久性，则由于客户端状态将丢失，因此无法保持QoS 1和2消息的传输。  
 
 >publish： 向服务器上的主题发布消息， 主要用于即时通讯（IoT、 聊天...）
 
 >subscribe： 订阅主题， 服务器通过你订阅的主题向你发布消息， 这样就能实现服务器向APP推送信息了
 
 >ClientId： 客户端ID应该是唯一的， 多个用户同时使用同一个ID创建连接会不断挤线互踢导致连接不断中断， 服务器也是通过ID来区分不同用户的操作
 
 
#### demo
1.添加引用
```java
<dependency>
    <groupId>org.eclipse.paho</groupId>
    <artifactId>org.eclipse.paho.client.mqttv3</artifactId>
    <version>1.2.0</version>
</dependency>    
``` 

   
2.服务端
```java
package org.xcy.demo;

import org.eclipse.paho.client.mqttv3.*;
import org.eclipse.paho.client.mqttv3.persist.MemoryPersistence;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import java.nio.charset.StandardCharsets;
import java.util.Objects;

public class Server {
   private static final Logger LOGGER = LoggerFactory.getLogger(Server.class);

   private String clientId;
   private MqttClient client;
   private MqttTopic mqttTopic;

   public Server() throws MqttException {
      this.clientId = Config.uuidAsClientID();
      this.client = new MqttClient(Config.HOST, clientId, new MemoryPersistence());
      this.connect();
   }

   private void connect() throws MqttException {
      MqttConnectOptions options = new MqttConnectOptions();
      options.setCleanSession(false);
      options.setUserName(Config.USERNAME);
      options.setPassword(Config.PASSWORD.toCharArray());
      options.setConnectionTimeout(10);
      options.setKeepAliveInterval(20);
      options.setWill(Config.TOPIC, "close".getBytes(), 2, true);

      this.client.setCallback(new Callback());
      this.client.connect(options);
   }

   public void publish(MqttMessage message) throws MqttException {
      mqttTopic = this.client.getTopic(Config.TOPIC);
      MqttDeliveryToken token = this.mqttTopic.publish(message);
      token.waitForCompletion();
   }

   public void publish(MqttMessage message, String clientId) throws MqttException {
      mqttTopic = this.client.getTopic(Config.TOPIC + "/" + clientId);
      this.publish(message);
   }


   public void close() throws MqttException {
      if (Objects.isNull(this.client)) {
         return;
      }
      this.client.disconnect();
      this.client.close();
   }


   public static void main(String[] args) throws MqttException, InterruptedException {
     //publishMSG
     Server server = new Server();
     MqttMessage message = new MqttMessage("发给大家".getBytes(StandardCharsets.UTF_8));
     message.setRetained(true);
     message.setQos(1);
     server.publish(message);


     //单独个客户端1发送topic
     MqttMessage message2 = new MqttMessage("发给客户端2".getBytes(StandardCharsets.UTF_8));
     message.setRetained(true);
     message.setQos(1);
     server.publish(message2, "2");


     //单独个客户端1发送topic
     MqttMessage message1 = new MqttMessage("发给客户端1".getBytes(StandardCharsets.UTF_8));
     message.setRetained(true);
     message.setQos(1);
     server.publish(message1, "1");
     server.close();
   }
}
```

3.客户端

```java
package org.xcy.demo;

import org.eclipse.paho.client.mqttv3.MqttClient;
import org.eclipse.paho.client.mqttv3.MqttConnectOptions;
import org.eclipse.paho.client.mqttv3.MqttTopic;
import org.eclipse.paho.client.mqttv3.persist.MemoryPersistence;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class Client {
   private static final Logger LOGGER = LoggerFactory.getLogger(Client.class);

   public void start() {
       try {
           String clientId ="1"; //Config.uuidAsClientID();

           //Mqtt客户端
           MqttClient client = new MqttClient(Config.HOST, clientId, new MemoryPersistence());

           //Mqtt回调
           client.setCallback(new Callback());


           //Mqtt连接选项
           MqttConnectOptions options = new MqttConnectOptions();
           //置清空Session，false表示服务器会保留客户端的连接记录，true表示每次以新的身份连接到服务器
           options.setCleanSession(true);
           options.setUserName(Config.USERNAME);
           options.setPassword(Config.PASSWORD.toCharArray());
           //连接超时
           options.setConnectionTimeout(20);
           //客户端每隔10秒向服务端发送心跳包判断客户端是否在线
           options.setKeepAliveInterval(10);
           // 客户端是否自动尝试重新连接到服务器
           options.setAutomaticReconnect(true);

           // 最后的遗言(连接断开时， 发动"close"给订阅了topic该主题的用户告知连接已中断)
           MqttTopic topic = client.getTopic(Config.TOPIC+"/"+clientId);
           options.setWill(topic, "close".getBytes(), 2, true);


           client.connect(options);

           int[] Qos = {1,1};
           String[] topics =new String[]{Config.TOPIC,Config.TOPIC+"/"+clientId};
           client.subscribe(topics, Qos);

       } catch (Exception ex) {
           LOGGER.error(ex.getMessage(), ex);
       }
   }

   public static void main(String[] args) {
       Client client = new Client();
       client.start();
       System.out.println("client1 is running ... ");
   } 
}
```
   
4.回调类用于MQTT异步回调 
```java
package org.xcy.demo;

import org.eclipse.paho.client.mqttv3.IMqttDeliveryToken;
import org.eclipse.paho.client.mqttv3.MqttCallback;
import org.eclipse.paho.client.mqttv3.MqttException;
import org.eclipse.paho.client.mqttv3.MqttMessage;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import java.nio.charset.StandardCharsets;

public class Callback implements MqttCallback {

    private static final Logger LOGGER = LoggerFactory.getLogger(Callback.class);

    @Override
    public void connectionLost(Throwable cause) {
        LOGGER.debug("connection lost,cause:{}", cause);
    }

    @Override
    public void messageArrived(String topic, MqttMessage message) throws Exception {
        LOGGER.info("Received Topic:{} Message QOS:{} Message ID:{}",topic, message.getQos(), message.getId());
        LOGGER.info("Received Message Body:{}", new String(message.getPayload(), StandardCharsets.UTF_8));
    }

    @Override
    public void deliveryComplete(IMqttDeliveryToken token) {
        try {
            LOGGER.debug("delivery complete,message:{},complete:{}", token.getMessage(), token.isComplete());
        } catch (MqttException e) {
            LOGGER.error(e.getMessage(), e);
        }
    }
}
```
### 服务端组件
 emqx([官网](https://docs.emqx.io/docs/broker/v3/cn/getstarted.html#emq))
 
#### 简介
 EMQ X (Erlang/Enterprise/Elastic MQTT Broker) 是基于 Erlang/OTP 平台开发的开源物联网 MQTT 消息服务器
 
