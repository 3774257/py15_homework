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
        
    +   声明queue代码：

            import pika
            credentials = pika.PlainCredentials('walker','123456')
            connection = pika.BlockingConnection(pika.ConnectionParameters(host='127.0.0.1',credentials=credentials))
            channel = connection.channel()
            #声明queue
            channel.queue_declare(queue='20161226',durable=True)#durable queue持久化

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
    +   例子：
            
        +   queue持久化:
            
                channel.queue_declare(queue='hello', durable=True)
        +   内容持久化:
                
                channel.basic_publish(exchange='',
                      routing_key="task_queue",
                      body=message,
                      properties=pika.BasicProperties(
                         delivery_mode = 2, # 消息持久化
                      ))
+   Prefetch count(轮训分发):
        
        前面我们讲到如果有多个消费者同时订阅同一个Queue中的消息，Queue中的消息会被平摊给多个消费者。
        这时如果每个消息的处理时间不同，就有可能会导致某些消费者一直在忙，而另外一些消费者很快就处理完
        手头工作并一直空闲的情况。我们可以通过设置prefetchCount来限制Queue每次发送给每个消费者的
        消息数，比如我们设置prefetchCount=1，则Queue每次给每个消费者发送一条消息；消费者处理完这
        条消息后Queue会再给该消费者发送一条消息。
       ![image](https://github.com/jijianming/py15_homework/blob/master/my_pictures/day12-04.png)    
    +   例子：
    
            channel.basic_qos(prefetch_count=1)
            
+   带消息持久化+公平分发的完整代码:
    
    +   生产者:
        
            #!/usr/bin/env python
            import pika
            import sys
             
            credentials = pika.PlainCredentials('walker','123456')
            connection = pika.BlockingConnection(pika.ConnectionParameters(host='127.0.0.1',credentials=credentials))
            channel = connection.channel()
             
            channel.queue_declare(queue='task_queue', durable=True)
             
            message = ' '.join(sys.argv[1:]) or "Hello World!"
            channel.basic_publish(exchange='',
                                  routing_key='task_queue',
                                  body=message,
                                  properties=pika.BasicProperties(
                                     delivery_mode = 2, # make message persistent
                                  ))
            print(" [x] Sent %r" % message)
            connection.close()
    +   消费者:
            
            #!/usr/bin/env python
            import pika
            import time
             
            credentials = pika.PlainCredentials('walker','123456')
            connection = pika.BlockingConnection(pika.ConnectionParameters(host='127.0.0.1',credentials=credentials))
            channel = connection.channel()
             
            channel.queue_declare(queue='task_queue', durable=True)
            print(' [*] Waiting for messages. To exit press CTRL+C')
             
            def callback(ch, method, properties, body):
                print(" [x] Received %r" % body)
                time.sleep(body.count(b'.'))
                print(" [x] Done")
                ch.basic_ack(delivery_tag = method.delivery_tag)
             
            channel.basic_qos(prefetch_count=1)
            channel.basic_consume(callback,
                                  queue='task_queue')
             
            channel.start_consuming()

+   Exchange(交换器):
        
        在上一节我们看到生产者将消息投递到Queue中，实际上这在RabbitMQ中这种事情永远都不会发生。
        实际的情况是，生产者将消息发送到Exchange（交换器，下图中的X），由Exchange将消息路由到
        一个或多个Queue中（或者丢弃）。
       ![image](https://github.com/jijianming/py15_homework/blob/master/my_pictures/day12-05.png)
        
        Exchange是按照什么逻辑将消息路由到Queue的？这个将在Binding一节介绍。
        RabbitMQ中的Exchange有四种类型，不同的类型有着不同的路由策略，
        这将在Exchange Types一节介绍。
+   routing key:
        
        生产者在将消息发送给Exchange的时候，一般会指定一个routing key，
        来指定这个消息的路由规则，而这个routing key需要与Exchange Type
        及binding key联合使用才能最终生效。在Exchange Type与binding key
        固定的情况下（在正常使用时一般这些内容都是固定配置好的），我们的生产者就
        可以在发送消息给Exchange时，通过指定routing key来决定消息流向哪里。
        RabbitMQ为routing key设定的长度限制为255 bytes。
+   Binding
        
        RabbitMQ中通过Binding将Exchange与Queue关联起来，这样RabbitMQ就知道如何正确地将消息路由到指定的Queue了。
       ![image](https://github.com/jijianming/py15_homework/blob/master/my_pictures/day12-06.png)
        
+   Binding key

        在绑定（Binding）Exchange与Queue的同时，一般会指定一个binding key；
        消费者将消息发送给Exchange时，一般会指定一个routing key；当binding key
        与routing key相匹配时，消息将会被路由到对应的Queue中。这个将在Exchange 
        Types章节会列举实际的例子加以说明。在绑定多个Queue到同一个Exchange的时候，
        这些Binding允许使用相同的binding key。binding key 并不是在所有情况下都
        生效，它依赖于Exchange Type，比如fanout类型的Exchange就会无视binding 
        key，而是将消息路由到所有绑定到该Exchange的Queue。
+   Exchange Types

        RabbitMQ常用的Exchange Type有fanout、direct、topic、headers这四种
        （AMQP规范里还提到两种Exchange Type，分别为system与自定义，这里不予以描述),
        Exchange在定义的时候是有类型的，以决定到底是哪些Queue符合条件，可以接收消息
        fanout: 所有bind到此exchange的queue都可以接收消息
        direct: 通过routingKey和exchange决定的那个唯一的queue可以接收消息
        topic:所有符合routingKey(此时可以是一个表达式)的routingKey所bind的queue可以接收消息
        下面分别进行介绍.
    +   fanout
            
            fanout类型的Exchange路由规则非常简单，它会把所有发送到该Exchange的消息路由到所有与它绑定的Queue中。
           ![image](https://github.com/jijianming/py15_homework/blob/master/my_pictures/day12-06.png)
            
            上图中，生产者（P）发送到Exchange（X）的所有消息都会路由到图中的两个Queue，并最终被两个消费者（C1与C2）消费。
            
        +   例子:
            
            +    生产者：
                   
                import pika
                import sys
                
                credentials = pika.PlainCredentials('walker','123456')
                connection = pika.BlockingConnection(pika.ConnectionParameters(host='127.0.0.1',credentials=credentials))
                channel = connection.channel()
                
                channel.exchange_declare(exchange='logs',
                                     type='fanout')
                
                message = ' '.join(sys.argv[1:]) or "info: Hello World!"
                channel.basic_publish(exchange='logs',
                                  routing_key='',
                                  body=message)
                print(" [x] Sent %r" % message)
                connection.close()
               
            +   消费者:
            
            
                import pika
                
                credentials = pika.PlainCredentials('walker','123456')
                connection = pika.BlockingConnection(pika.ConnectionParameters(host='127.0.0.1',credentials=credentials))
                channel = connection.channel()
                channel.exchange_declare(exchange='logs',
                type='fanout')
                
                result = channel.queue_declare(exclusive=True) #不指定queue名字,rabbit会随机分配一个名字,exclusive=True会在使用此queue的消费者断开后,自动将queue删除
                queue_name = result.method.queue
                
                channel.queue_bind(exchange='logs',
                queue=queue_name)
                
                print(' [*] Waiting for logs. To exit press CTRL+C')
                
                def callback(ch, method, properties, body):
                print(" [x] %r" % body)
                
                channel.basic_consume(callback,
                queue=queue_name,
                no_ack=True)
                channel.start_consuming()