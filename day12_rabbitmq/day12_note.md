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
        
        RabbitMQ中的消息都只能存储在Queue中，生产者（下图中的P）生产消息并最终投递到Queue中，消费者（下图中的C）可以从Queue中获取消息并消费。
       ![image](https://github.com/jijianming/py15_homework/blob/master/my_pictures/day12-02.png)
        
        多个消费者可以订阅同一个Queue，这时Queue中的消息会被平均分摊给多个消费者进行处理，而不是每个消费者都收到所有的消息并处理。
       ![image](https://github.com/jijianming/py15_homework/blob/master/my_pictures/day12-03.png)

+   Message acknowledgment(消息确认)
        
        在实际应用中，可能会发生消费者收到Queue中的消息，但没有处理完成就宕机（或出现其他意外）的情况，这种情况下就可能会导致消息丢失。
        为了避免这种情况发生，我们可以要求消费者在消费完消息后发送一个回执给RabbitMQ，RabbitMQ收到消息回执（Message acknowledgment）
        后才将该消息从Queue中移除；如果RabbitMQ没有收到回执并检测到消费者的RabbitMQ连接断开，则RabbitMQ会将该消息发送给其他消费者
        （如果存在多个消费者）进行处理。这里不存在timeout概念，一个消费者处理消息时间再长也不会导致该消息被发送给其他消费者，除非它的RabbitMQ
        连接断开。这里会产生另外一个问题，如果我们的开发人员在处理完业务逻辑后，忘记发送回执给RabbitMQ，这将会导致严重的bug——Queue中堆积的
        消息会越来越多；消费者重启后会重复消费这些消息并重复执行业务逻辑…
       
+   Message durability(消息持久化)
        
        如果我们希望即使在RabbitMQ服务重启的情况下，也不会丢失消息，我们可以将Queue与Message都设置为可持久化的（durable），
        这样可以保证绝大部分情况下我们的RabbitMQ消息不会丢失。但依然解决不了小概率丢失事件的发生（比如RabbitMQ服务器已经接收到
        生产者的消息，但还没来得及持久化该消息时RabbitMQ服务器就断电了），如果我们需要对这种小概率事件也要管理起来，那么我们要用
        到事务。由于这里仅为RabbitMQ的简单介绍，所以这里将不讲解RabbitMQ相关的事务。