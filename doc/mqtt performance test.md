## Links
[[README](../README.md)]
[[mqtt](<../doc/mqtt.md>)]
[[mqtt performance test](<../doc/mqtt performance test.md>)]

# MQTT 性能测试

## 测试环境

- 阿里云MQTT新用户套餐

    ![阿里云MQTT新用户套餐](<./mqtt performance test/aliyun-mqtt-price-newuser-01.jpg>)

    ![阿里云MQTT实例](<./mqtt performance test/aliyun-mqtt-instance-02.jpg>)

- 配置信息

  - 服务部署区域：亚太 - 华南1 - 深圳
  - 连接数上限：100个
  - TPS上限：100t/s
  - 订阅关系数上限：1000个

- 使用限制

    > [阿里云 MQTT 使用限制](https://help.aliyun.com/document_detail/63620.html?spm=a2c4g.11186623.2.14.17e871d7qjPHSj#concept-63620-zh)

## 用例：阿里云 / 单连接 / 单线程

通过 restful api 封装 mqtt 发送，jmeter，压测

### -- 压测 01 --

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

    接收消息的速度，远比发送消息的速度慢，订阅者最终只收到50000多条，约一半的消息没收到。
    
    消息没有全部接收的具体原因，有待确定，有两种可能性：

     - 没有做限流的情况下，导致消息堆积到了上限，部分被丢弃。
     - 我目前的测试环境限制，TPS上限：100t/s。

- 消息接收顺序

    接收顺序不保证，如截图。

- 结果 - 发布

    ![发布](<./mqtt performance test/perftest01-01-01.jpg>)

- 结果 - 订阅

    ![订阅](<./mqtt performance test/perftest01-01-02.jpg>)

- 结果 - 订阅顺序不保证

    ![订阅](<./mqtt performance test/perftest01-01-03.jpg>)

### -- 压测 02 --

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

### -- 压测 03 --

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

### -- 压测 04 --

#### 参数

- 带驱逐能力的队列（EvictingQueue - google Guava库）
  - 固定容量36000条消息
  - 当容量用满时，驱逐队列最前的消息，以获取空间，并把新消息加到队列最后面
- 线程1 - 模拟 DA 往队列推数据
  - 20 tps
- 每条数据大小大约3000字符
- 线程2 - 从队列取数据，并发送到MQTT
- 网络延时：50ms
- 网络丢包：50%

#### 压测结果

- 线程1发送速度，与线程2的队列速度一致，EvictingQueue队列中的消息全部及时发送

    ![线程1发送速度，与线程2的队列速度一致，EvictingQueue队列中的消息全部及时发送](<./mqtt performance test/perftest01-04-01.jpg>)

- 部分消息发送失败

    ![线程1发送速度，与线程2的队列速度一致，EvictingQueue队列中的消息全部及时发送](<./mqtt performance test/perftest01-04-02.jpg>)

## 名词解释

### QoS

- QoS0

    ![QoS0](<./mqtt performance test/qos0.webp>)

- QoS1

    ![QoS1](<./mqtt performance test/qos1.webp>)

- QoS2

    ![QoS2](<./mqtt performance test/qos2.webp>)




