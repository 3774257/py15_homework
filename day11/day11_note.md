##今日内容
+   Gevent协程
+   Select\Poll\Epoll异步IO与事件驱动
+   RabbitMQ队列

##协程
    协程，又称微线程，纤程。英文名Coroutine。一句话说明什么是线程：协程是一种用户态的轻量级线程。
    协程拥有自己的寄存器上下文和栈。协程调度切换时，将寄存器上下文和栈保存到其他地方，在切回来的时候，
    恢复先前保存的寄存器上下文和栈。因此：协程能保留上一次调用时的状态（即所有局部状态的一个特定组合），
    每次过程重入时，就相当于进入上一次调用的状态，换种说法：进入上一次离开时所处逻辑流的位置。
    协程的好处：
        无需线程上下文切换的开销
        无需原子操作锁定及同步的开销
        "原子操作(atomic operation)是不需要synchronized"，所谓原子操作是指不会被线程调度机制
        打断的操作；这种操作一旦开始，就一直运行到结束，中间不会有任何 context switch （切换到另
        一个线程）。原子操作可以是一个步骤，也可以是多个操作步骤，但是其顺序是不可以被打乱，或者切割
        掉只执行部分。视作整体是原子性的核心。
        方便切换控制流，简化编程模型
        高并发+高扩展性+低成本：一个CPU支持上万的协程都不是问题。所以很适合用于高并发处理。
    
    缺点：
        无法利用多核资源：协程的本质是个单线程,它不能同时将 单个CPU 的多个核用上,协程需要和进程配合
        才能运行在多CPU上.当然我们日常所编写的绝大部分应用都没有这个必要，除非是cpu密集型应用。
        进行阻塞（Blocking）操作（如IO时）会阻塞掉整个程序
    
    使用yield实现协程操作例子　　　　
        import time
        import queue
        def consumer(name):
            print("--->starting eating baozi...")
            while True:
                new_baozi = yield
                print("[%s] is eating baozi %s" % (name,new_baozi))
                #time.sleep(1)
         
        def producer():
         
            r = con.__next__()
            r = con2.__next__()
            n = 0
            while n < 5:
                n +=1
                con.send(n)
                con2.send(n)
                print("\033[32;1m[producer]\033[0m is making baozi %s" %n )
         
         
        if __name__ == '__main__':
            con = consumer("c1")
            con2 = consumer("c2")
            p = producer()
    看楼上的例子，我问你这算不算做是协程呢？你说，我他妈哪知道呀，你前面说了一堆废话，但是并没告诉
    我协程的标准形态呀，我腚眼一想，觉得你说也对，那好，我们先给协程一个标准定义，即符合什么条件就能称之为协程：
    必须在只有一个单线程里实现并发
    修改共享数据不需加锁
    用户程序里自己保存多个控制流的上下文栈
    一个协程遇到IO操作自动切换到其它协程
    基于上面这4点定义，我们刚才用yield实现的程并不能算是合格的线程，因为它有一点功能没实现，哪一点呢？

    Greenlet
    greenlet是一个用C实现的协程模块，相比与python自带的yield，它可以使你在任意函数之间随意切换，而不需把
    这个函数先声明为generator。
    例子：
        # -*- coding:utf-8 -*-
        from greenlet import greenlet
 
        def test1():
            print(12)
            gr2.switch()
            print(34)
            gr2.switch()

        def test2():
            print(56)
            gr1.switch()
            print(78)
        gr1 = greenlet(test1)
        gr2 = greenlet(test2)
        gr1.switch()
    感觉确实用着比generator还简单了呢，但好像还没有解决一个问题，就是遇到IO操作，自动切换，对不对？

    Gevent 
    
    Gevent 是一个第三方库，可以轻松通过gevent实现并发同步或异步编程，在gevent中用到的主要模式是
    Greenlet, 它是以C扩展模块形式接入Python的轻量级协程。 Greenlet全部运行在主程序操作系统进程的内部，但它们被协作式地调度。
    例子：
        import gevent
         
        def func1():
            print('\033[31;1m李闯在跟海涛搞...\033[0m')
            gevent.sleep(2)
            print('\033[31;1m李闯又回去跟继续跟海涛搞...\033[0m')
         
        def func2():
            print('\033[32;1m李闯切换到了跟海龙搞...\033[0m')
            gevent.sleep(1)
            print('\033[32;1m李闯搞完了海涛，回来继续跟海龙搞...\033[0m')

        gevent.joinall([
            gevent.spawn(func1),
            gevent.spawn(func2),
            #gevent.spawn(func3),
        ])

    输出：
        李闯在跟海涛搞...
        李闯切换到了跟海龙搞...
        李闯搞完了海涛，回来继续跟海龙搞...
        李闯又回去跟继续跟海涛搞...
    
    同步与异步的性能区别：
    例如：
        import gevent
 
        def task(pid):
            """
            Some non-deterministic task
            """
            gevent.sleep(0.5)
            print('Task %s done' % pid)
         
        def synchronous():
            for i in range(1,10):
                task(i)
         
        def asynchronous():
            threads = [gevent.spawn(task, i) for i in range(10)]
            gevent.joinall(threads)
         
        print('Synchronous:')
        synchronous()
         
        print('Asynchronous:')
        asynchronous()
    上面程序的重要部分是将task函数封装到Greenlet内部线程的gevent.spawn。 初始化的greenlet
    列表存放在数组threads中，此数组被传给gevent.joinall 函数，后者阻塞当前流程，并执行所有给
    定的greenlet。执行流程只会在 所有greenlet执行完后才会继续向下走。　　
    
    遇到IO阻塞时会自动切换任务
    例如：
        from gevent import monkey; monkey.patch_all()
        import gevent
        from  urllib.request import urlopen
         
        def f(url):
            print('GET: %s' % url)
            resp = urlopen(url)
            data = resp.read()
            print('%d bytes received from %s.' % (len(data), url))
         
        gevent.joinall([
                gevent.spawn(f, 'https://www.python.org/'),
                gevent.spawn(f, 'https://www.yahoo.com/'),
                gevent.spawn(f, 'https://github.com/'),
        ])
         
        
    通过gevent实现单线程下的多socket并发
    例如：
        server 端
  
        import sys
        import socket
        import time
        import gevent
         
        from gevent import socket,monkey
        monkey.patch_all()

        def server(port):
            s = socket.socket()
            s.bind(('0.0.0.0', port))
            s.listen(500)
            while True:
                cli, addr = s.accept()
                gevent.spawn(handle_request, cli)

        def handle_request(conn):
            try:
                while True:
                    data = conn.recv(1024)
                    print("recv:", data)
                    conn.send(data)
                    if not data:
                        conn.shutdown(socket.SHUT_WR)
         
            except Exception as  ex:
                print(ex)
            finally:
                conn.close()
        if __name__ == '__main__':
            server(8001)
        　　
        
        client 端 　　
        
        import socket
        HOST = 'localhost'    # The remote host
        PORT = 8001           # The same port as used by the server
        s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        s.connect((HOST, PORT))
        while True:
            msg = bytes(input(">>:"),encoding="utf8")
            s.sendall(msg)
            data = s.recv(1024)
            #print(data)
         
            print('Received', repr(data))
        s.close()
        
        并发1000个连接
        
        import socket
        import threading
        
        def sock_conn():
        
            client = socket.socket()
        
            client.connect(("localhost",8001))
            count = 0
            while True:
                #msg = input(">>:").strip()
                #if len(msg) == 0:continue
                client.send( ("hello %s" %count).encode("utf-8"))
        
                data = client.recv(1024)
        
                print("[%s]recv from server:" % threading.get_ident(),data.decode()) #结果
                count +=1
            client.close()
        
        
        for i in range(100):
            t = threading.Thread(target=sock_conn)
            t.start()
        未完待续。。。。