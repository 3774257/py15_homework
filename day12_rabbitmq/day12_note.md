##今日作业
+   rabbitmq


##安装：
    安装python rabbitMQ module 
    pip install pika
    or
    easy_install pika
    or
    源码
    https://pypi.python.org/pypi/pika
##为什么要用消息队列？
    你是否遇到过两个（多个）系统间需要通过定时任务来同步某些数据？你是否在为异构系统的不同进程间相互调用、通讯的问题而苦恼、挣扎？
    如果是，那么恭喜你，消息服务让你可以很轻松地解决这些问题。
    消息服务擅长于解决多系统、异构系统间的数据交换（消息通知/通讯）问题，你也可以把它用于系统间服务的相互调用（RPC）。本文将要介
    绍的RabbitMQ就是当前最主流的消息中间件之一。
    
##RabbitMQ简介
    AMQP，即Advanced Message Queuing Protocol，高级消息队列协议，是应用层协议的一个开放标准，为面向消息的中间件设计。消
    息中间件主要用于组件之间的解耦，消息的发送者无需知道消息使用者的存在，反之亦然。AMQP的主要特征是面向消息、队列、路由（包括
    点对点和发布/订阅）、可靠性、安全。RabbitMQ是一个开源的AMQP实现，服务器端用Erlang语言编写，支持多种客户端，如：Python、
    Ruby、.NET、Java、JMS、C、PHP、ActionScript、XMPP、STOMP等，支持AJAX。用于在分布式系统中存储转发消息，在易用性、扩展性、
    高可用性等方面表现不俗。
##RabbitMQ基础
    ConnectionFactory、Connection、Channel都是RabbitMQ对外提供的API中最基本的对象。
    Connection是RabbitMQ的socket链接，它封装了socket协议相关部分逻辑。ConnectionFactory为Connection的制造工厂。
    Channel是我们与RabbitMQ打交道的最重要的一个接口，我们大部分的业务操作是在Channel这个接口中完成的，包括定义Queue、定义Exchange、
    绑定Queue与Exchange、发布消息等   
    
+   Queue
                
        Queue（队列）是RabbitMQ的内部对象，用于存储消息，用下图表示 
       ![image](https://github.com/jijianming/py15_homework/blob/master/my_pictures/day12-01.png)
       