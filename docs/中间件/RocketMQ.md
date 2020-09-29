## RocketMq

常见消息队列：RabbitMQ,RocketMQ,Kafka
RocketMQ特点：延迟消息队列，应用场景，代替定时器的主动轮询，消息队列->服务器。

消息队列可以提高系统并发能力，但是异步难调试。

可以解耦，降低应用的复杂程度。

可以减少服务器压力，起到销峰的作用。

windows环境下的RocketMQ:

```
一 配置
1 统环境变量配置
变量名：ROCKETMQ_HOME
变量值：MQ解压路径\MQ文件夹名
```

```
二 运行
1 启动NAMESERVER：Cmd命令框执行进入至‘MQ文件夹\bin’下，然后执行‘start mqnamesrv.cmd’，启动NAMESERVER。成功后会弹出提示框，此框勿关闭。
2 启动BROKER： Cmd命令框执行进入至‘MQ文件夹\bin’下，然后执行‘start mqbroker.cmd -n 127.0.0.1:9876 autoCreateTopicEnable=true’，启动BROKER。成功后会弹出提示框，此框勿关闭。
3.启动生产者：
重新打开一个cmd窗口，跳转到bin目录下，依次执行以下命令：
set NAMESRV_ADDR=localhost:9876
tools.cmd org.apache.rocketmq.example.quickstart.Producer
4.启动消费者
set NAMESRV_ADDR=localhost:9876
tools.cmd org.apache.rocketmq.example.quickstart.Consumer
```

