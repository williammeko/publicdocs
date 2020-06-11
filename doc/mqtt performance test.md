## Links
[[README](../README.md)]
[[mqtt performance test](<../doc/mqtt performance test.md>)]


# MQTT 性能测试

## 测试环境

阿里云MQTT新用户套餐

![阿里云MQTT新用户套餐](<./mqtt performance test/aliyun-mqtt-price-newuser-01.jpg>)

![阿里云MQTT实例](<./mqtt performance test/aliyun-mqtt-instance-02.jpg>)

配置信息

 - 服务部署区域：亚太 - 华南1 - 深圳
 - 连接数上限：100个
 - TPS上限：100t/s
 - 订阅关系数上限：1000个

## 用例：阿里云 / 单连接 / 单线程

通过 restful api 封装 mqtt 发送，jmeter，压测

### -- 压测01 --

#### 参数

```
maxInflight=10
qos=0
retained=false
```

#### 压测结果

- QoS=0

    发送者发送完数据之后，不关心消息是否已经投递到了接收者那边。此时发送方吞吐量很大，单连接单线程的情况下，能达到2000多笔/秒。发送100000笔，没有报错。

- 订阅

    接收消息的速度，远比发送消息的速度慢，订阅者最终只收到50000多条，约一半的消息没收到，具体原因有待确定。但初步诊断可能是没有做限流的情况下，导致消息堆积到了上限，部分被丢弃，这个有待确定。

- 消息接收顺序

    接收顺序不保证，如截图。

- 结果 - 发布

    ![发布](<./mqtt performance test/perftest01-01-01.jpg>)

- 结果 - 订阅

    ![订阅](<./mqtt performance test/perftest01-01-02.jpg>)

- 结果 - 订阅顺序不保证

    ![订阅](<./mqtt performance test/perftest01-01-03.jpg>)

### -- 压测02 --

#### 参数

```
maxInflight=10
qos=1
retained=false
```

#### 压测结果

- QoS=1

    MQTT 通过简单的 ACK 机制来保证消息至少送达一次。此时发送方吞吐量只有15，发送没有报错。

- 订阅

    接收消息的速度，跟发送消息的基本上速度一致，也就是大概15笔/秒。

    但是压测过程中，有个怪异现象，订阅方在每次连接后，大概每接收1000条消息时，连接就会再次被断开，断开原因待查。而我代码做了逻辑，在每次断开连接的时候，会重连，并重新订阅，在重连重订阅的过程后，消息会丢失大约几条，丢失率大概是 5/1000。

- 消息接收顺序

    经过多次测试，消息是按发送顺序接收的。

- 结果 - 发布

    ![发布](<./mqtt performance test/perftest01-02-01.jpg>)

- 结果 - 订阅

    ![订阅](<./mqtt performance test/perftest01-02-02.jpg>)

### -- 压测03 --

#### 参数

```
maxInflight=10
qos=2
retained=false
```

#### 压测结果

- QoS=2

    MQTT 通过至少2次网络往返，来保证消息送达，且只送达一次。此时发送方吞吐量只有8.8笔/秒，发送没有报错。

- 订阅

    接收消息的速度，跟发送消息的基本上速度一致，也就是大概8.8笔/秒。

    但是压测过程中，有个怪异现象，订阅方在每次连接后，大概每接收1000条消息时，连接就会再次被断开，断开原因待查。而我代码做了逻辑，在每次断开连接的时候，会重连，并重新订阅，在重连重订阅的过程后，消息会丢失大约几条，丢失率大概是 5/1000。

- 消息接收顺序

    经过多次测试，消息是按发送顺序接收的。

- 结果 - 发布

    ![发布](<./mqtt performance test/perftest01-03-01.jpg>)

- 结果 - 订阅

    ![订阅](<./mqtt performance test/perftest01-03-02.jpg>)

## 名词解释

### QoS

- QoS0

    ![QoS0](<./mqtt performance test/qos0.webp>)

- QoS1

    ![QoS1](<./mqtt performance test/qos1.webp>)

- QoS2

    ![QoS2](<./mqtt performance test/qos2.webp>)




