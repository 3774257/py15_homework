##本节内容
+   进程、线程的区别
+   python GIL全局解释器锁
+   线程
+   进程

##线程与进程的区别
    程序并不能单独运行，只有将程序装载到内存中，系统为它分配资源才能运行，而这种执行的程序就称之为进程。程序和进程的区别就在于：
    程序是指令的集合，它是进程运行的静态描述文本；进程是程序的一次执行活动，属于动态概念。
    在多道编程中，我们允许多个程序同时加载到内存中，在操作系统的调度下，可以实现并发地执行。这是这样的设计，大大提高了CPU的利用率。
    进程的出现让每个用户感觉到自己独享CPU，因此，进程就是为了在CPU上实现多道编程而提出的。    
    有了进程为什么还要线程？    
    进程有很多优点，它提供了多道编程，让我们感觉我们每个人都拥有自己的CPU和其他资源，可以提高计算机的利用率。很多人就不理解了，
    既然进程这么优秀，为什么还要线程呢？其实，仔细观察就会发现进程还是有很多缺陷的，主要体现在两点上：   
    进程只能在一个时间干一件事，如果想同时干两件事或多件事，进程就无能为力了。
    进程在执行的过程中如果阻塞，例如等待输入，整个进程就会挂起，即使进程中有些工作不依赖于输入的数据，也将无法执行。
    例如，我们在使用qq聊天， qq做为一个独立进程如果同一时间只能干一件事，那他如何实现在同一时刻 即能监听键盘输入、又能监听其它人给你发的消息、
    同时还能把别人发的消息显示在屏幕上呢？你会说，操作系统不是有分时么？但我的亲，分时是指在不同进程间的分时呀， 即操作系统处理一会你的qq任务，
    又切换到word文档任务上了，每个cpu时间片分给你的qq程序时，你的qq还是只能同时干一件事呀。
    再直白一点， 一个操作系统就像是一个工厂，工厂里面有很多个生产车间，不同的车间生产不同的产品，每个车间就相当于一个进程，且你的工厂又穷，
    供电不足，同一时间只能给一个车间供电，为了能让所有车间都能同时生产，你的工厂的电工只能给不同的车间分时供电，但是轮到你的qq车间时，
    发现只有一个干活的工人，结果生产效率极低，为了解决这个问题，应该怎么办呢？。。。。没错，你肯定想到了，就是多加几个工人，让几个人工人并行工作，
    这每个工人，就是线程！
    进程：
        一个程序要运行时所需的所有资源的集合
        进程是资源的集合相当于一个车间
        一个进程至少需要一个线程，这个线程称为主线程
        一个进程里可以有多个线程
        cpu cores越多，代表着你可以真正并发的线程越多
        两个进程之间的数据是完全独立的。互相不能访问。
        
        
    线程：
        一道单一的指令的控制流寄生在进程中
        单一进程里的多个线程是共享数据的
        多个线程在涉及修改同一个数据时，一定要加锁
##python GIL全局解释器锁
    无论你启多少个线程，你有多少个cpu, Python在执行的时候会淡定的在同一时刻只允许一个线程运行
##线程:
    线程是操作系统能够进行运算调度的最小单位。它被包含在进程之中，是进程中的实际运作单位。一条线程指的是进程中一个单一顺序的控制流，
    一个进程中可以并发多个线程，每条线程并行执行不同的任务。
    1、语法
        import threading
        import time
         
        def sayhi(num): #定义每个线程要运行的函数
         
            print("running on number:%s" %num)
         
            time.sleep(3)
         
        if __name__ == '__main__':
         
            t1 = threading.Thread(target=sayhi,args=(1,)) #生成一个线程实例
            t2 = threading.Thread(target=sayhi,args=(2,)) #生成另一个线程实例
         
            t1.start() #启动线程
            t2.start() #启动另一个线程
         
            print(t1.getName()) #获取线程名
            print(t2.getName())
            
        start           线程准备就绪，等待CPU调度
        setName         为线程设置名称
        getName         获取线程名称
        setDaemon       设置为后台线程或前台线程（默认）
                        如果是后台线程，主线程执行过程中，后台线程也在进行，主线程执行完毕后，后台线程不论成功与否，均停止
                        如果是前台线程，主线程执行过程中，前台线程也在进行，主线程执行完毕后，等待前台线程也执行完成后，程序停止
        join            逐个执行每个线程，执行完毕后继续往下执行，该方法使得多线程变得无意义
        
        import time
        import threading
         
         
        def run(n):
         
            print('[%s]------running----\n' % n)
            time.sleep(2)
            print('--done--')
         
        def main():
            for i in range(5):
                t = threading.Thread(target=run,args=[i,])
                t.start()
                t.join(1)
                print('starting thread', t.getName())
         
         
        m = threading.Thread(target=main,args=[])
        m.setDaemon(True) #将main线程设置为Daemon线程,它做为程序主线程的守护线程,
        #当主线程退出时,m线程也会退出,由m启动的其它子线程会同时退出,不管是否执行完任务
        m.start()
        m.join(timeout=2)
        print("---main thread done----")
    2、线程锁(互斥锁Mutex)
        一个进程下可以启动多个线程，多个线程共享父进程的内存空间，也就意味着每个线程可以访问同一份数据，
        此时，如果2个线程同时要修改同一份数据，会出现什么状况？
            import time
            import threading
             
            def addNum():
                global num #在每个线程中都获取这个全局变量
                print('--get num:',num )
                time.sleep(1)
                num  -=1 #对此公共变量进行-1操作         
            num = 100  #设定一个共享变量
            thread_list = []
            for i in range(100):
                t = threading.Thread(target=addNum)
                t.start()
                thread_list.append(t)         
            for t in thread_list: #等待所有线程执行完毕
                t.join()
            print('final num:', num )
        
        正常来讲，这个num结果应该是0， 但在python 2.7上多运行几次，会发现，最后打印出来的num结果不总是0，为什么每次运行的结果不一样呢？ 
        哈，很简单，假设你有A,B两个线程，此时都 要对num 进行减1操作， 由于2个线程是并发同时运行的，所以2个线程很有可能同时拿走了num=100这个初始变量交给cpu去运算，
        当A线程去处完的结果是99，但此时B线程运算完的结果也是99，两个线程同时CPU运算的结果再赋值给num变量后，结果就都是99。那怎么办呢？ 很简单，每个线程在要修改公共数据时，
        为了避免自己在还没改完的时候别人也来修改此数据，可以给这个数据加一把锁， 这样其它线程想修改此数据时就必须等待你修改完毕并把锁释放掉后才能再访问此数据。 
        *注：不要在3.x上运行，不知为什么，3.x上的结果总是正确的，可能是自动加了锁
        加锁版本:
            import time
            import threading
             
            def addNum():
                global num #在每个线程中都获取这个全局变量
                print('--get num:',num )
                time.sleep(1)
                lock.acquire() #修改数据前加锁
                num  -=1 #对此公共变量进行-1操作
                lock.release() #修改后释放
             
            num = 100  #设定一个共享变量
            thread_list = []
            lock = threading.Lock() #生成全局锁
            for i in range(100):
                t = threading.Thread(target=addNum)
                t.start()
                thread_list.append(t)
             
            for t in thread_list: #等待所有线程执行完毕
                t.join()
             
            print('final num:', num )
    3、Semaphore(信号量)
        互斥锁 同时只允许一个线程更改数据，而Semaphore是同时允许一定数量的线程更改数据 ，比如厕所有3个坑，那最多只允许3个人上厕所，后面的人只能等里面有人出来了才能再进去。
        import threading,time
 
        def run(n):
            semaphore.acquire()
            time.sleep(1)
            print("run the thread: %s\n" %n)
            semaphore.release()
         
        if __name__ == '__main__':
         
            num= 0
            semaphore  = threading.BoundedSemaphore(5) #最多允许5个线程同时运行
            for i in range(20):
                t = threading.Thread(target=run,args=(i,))
                t.start()
         
        while threading.active_count() != 1:
            pass #print threading.active_count()
        else:
            print('----all threads done---')
            print(num)
    4、事件（event）
        python线程的事件用于主线程控制其他线程的执行，事件主要提供了三个方法 set、wait、clear。
        
        事件处理的机制：全局定义了一个“Flag”，如果“Flag”值为 False，那么当程序执行 event.wait 方法时就会阻塞，
        如果“Flag”值为True，那么event.wait 方法时便不再阻塞。
        clear：将“Flag”设置为False
        set：将“Flag”设置为True
        import threading,time
        import random
        def light():
            if not event.isSet():
                event.set() #wait就不阻塞 #绿灯状态
            count = 0
            while True:
                if count < 10:
                    print('\033[42;1m--green light on---\033[0m')
                elif count <13:
                    print('\033[43;1m--yellow light on---\033[0m')
                elif count <20:
                    if event.isSet():
                        event.clear()
                    print('\033[41;1m--red light on---\033[0m')
                else:
                    count = 0
                    event.set() #打开绿灯
                time.sleep(1)
                count +=1
        def car(n):
            while 1:
                time.sleep(random.randrange(10))
                if  event.isSet(): #绿灯
                    print("car [%s] is running.." % n)
                else:
                    print("car [%s] is waiting for the red light.." %n)
        if __name__ == '__main__':
            event = threading.Event()
            Light = threading.Thread(target=light)
            Light.start()
            for i in range(3):
                t = threading.Thread(target=car,args=(i,))
                t.start()
    5、生产者消费者模型
        在并发编程中使用生产者和消费者模式能够解决绝大多数并发问题。该模式通过平衡生产线程和消费线程的工作能力来提高程序的整体处理数据的速度。
        为什么要使用生产者和消费者模式
        
        在线程世界里，生产者就是生产数据的线程，消费者就是消费数据的线程。在多线程开发当中，如果生产者处理速度很快，而消费者处理速度很慢，那
        么生产者就必须等待消费者处理完，才能继续生产数据。同样的道理，如果消费者的处理能力大于生产者，那么消费者就必须等待生产者。为了解决这
        个问题于是引入了生产者和消费者模式。 
        什么是生产者消费者模式
        生产者消费者模式是通过一个容器来解决生产者和消费者的强耦合问题。生产者和消费者彼此之间不直接通讯，而通过阻塞队列来进行通讯，所以生产
        者生产完数据之后不用等待消费者处理，直接扔给阻塞队列，消费者不找生产者要数据，而是直接从阻塞队列里取，阻塞队列就相当于一个缓冲区，平
        衡了生产者和消费者的处理能力。
        例如:
            import threading
            import queue
             
            def producer():
                for i in range(10):
                    q.put("骨头 %s" % i )
                print("开始等待所有的骨头被取走...")
                q.join()
                print("所有的骨头被取完了...")
            def consumer(n):
                while q.qsize() >0:
                    print("%s 取到" %n  , q.get())
                    q.task_done() #告知这个任务执行完了  
            q = queue.Queue()
            p = threading.Thread(target=producer,)
            p.start()
            c1 = consumer("李闯")
##进程
    1、语法
        from multiprocessing import Process
        import time
        def f(name):
            time.sleep(2)
            print('hello', name)
         
        if __name__ == '__main__':
            p = Process(target=f, args=('bob',))
            p.start()
            p.join()
    2、进程间的通信
        不同进程间内存是不共享的，要想实现两个进程间的数据交换，可以用以下方法：
        Queues:
        使用方法跟threading里的queue差不多
            from multiprocessing import Process, Queue
             
            def f(q):
                q.put([42, None, 'hello'])
             
            if __name__ == '__main__':
                q = Queue()
                p = Process(target=f, args=(q,))
                p.start()
                print(q.get())    # print "[42, None, 'hello']"
                p.join()
        Pipes:
            from multiprocessing import Process, Pipe
 
            def f(conn):
                conn.send([42, None, 'hello'])
                conn.close()
             
            if __name__ == '__main__':
                parent_conn, child_conn = Pipe()
                p = Process(target=f, args=(child_conn,))
                p.start()
                print(parent_conn.recv())   # prints "[42, None, 'hello']"
                p.join()
        Managers:
            from multiprocessing import Process, Manager
 
            def f(d, l):
                d[1] = '1'
                d['2'] = 2
                d[0.25] = None
                l.append(1)
                print(l)
             
            if __name__ == '__main__':
                with Manager() as manager:
                    d = manager.dict()
             
                    l = manager.list(range(5))
                    p_list = []
                    for i in range(10):
                        p = Process(target=f, args=(d, l))
                        p.start()
                        p_list.append(p)
                    for res in p_list:
                        res.join()
             
                    print(d)
                    print(l)
    3、进程同步:
        没有使用锁从不同进程的输出容易得到所有混合。
        from multiprocessing import Process, Lock
        def f(l, i):
            l.acquire()
            try:
                print('hello world', i)
            finally:
                l.release()
         
        if __name__ == '__main__':
            lock = Lock()
            for num in range(10):
                Process(target=f, args=(lock, num)).start()
    4、进程池:
        进程池内部维护一个进程序列，当使用时，则去进程池中获取一个进程，如果进程池序列中没有可供使用的进进程，那么程序就会等待，直到进程池中有可用进程为止。
        进程池中有两个方法：
            apply
            apply_async
        例如:
            from  multiprocessing import Process,Pool
            import time
             
            def Foo(i):
                time.sleep(2)
                return i+100
             
            def Bar(arg):
                print('-->exec done:',arg)
             
            pool = Pool(5)
             
            for i in range(10):
                pool.apply_async(func=Foo, args=(i,),callback=Bar)
                #pool.apply(func=Foo, args=(i,))
             
            print('end')
            pool.close()
            pool.join()#进程池中进程执行完毕后再关闭，如果注释，那么程序直接关闭
        使用地址池,要保持进程同步,需要用到Manager中的Lock
            #!/usr/bin/env python3
            # -*- coding: utf-8 -*-
            # @Time    : 16/12/13 09:54
            # @Author  : walker
            # @Site    : 
            # @File    : ssh.py.py
            # @Software: PyCharm
            import configparser
            import optparse
            import sys,time,os
            from multiprocessing import Pool,Manager
            import paramiko
            class SSH(object):
                ip_list = []
                # paramiko.util.log_to_file('paramiko.log')
                def __init__(self):
                    self.parser = optparse.OptionParser()
                    self.parser.add_option('-H','--hostname',dest='hostname',help='remote hostname') #定义参数
                    self.parser.add_option('-g','--groupname',dest='groupname',help='remote groupname')
                    self.parser.add_option('--cmd','--cmd',dest='cmd',help='Execute command')
                    self.options,self.args = self.parser.parse_args()
                    self.conf = configparser.ConfigParser()
                    self.s = paramiko.SSHClient()
                    self.s.set_missing_host_key_policy(paramiko.AutoAddPolicy())#忽略第一次连接添加host文件
                    self.conf.read('config.cfg')#读取主机配置文件,没有实现指定配置文件
                    self.data = self.check_args()#检测参数合法性
                    if self.data:#参数正确
                        # print('检测参数正确...')
                        # print(self.data)
                        ip_info = self.check_ip()#检测主机相关信息
                        if ip_info:
                            # print('检测ip正确')
                            # print('ip_info',ip_info)
                            self.execute_command(ip_info)#执行命令
            
                def connect_server(self,l,args):
                    '''
                    连接远程服务器
                    '''
                    # print(args)
                    l.acquire()#加锁
                    # print('父进程:%s,子进程:%s'% (os.getppid(),os.getpid()))
                    print('ip:%s at:%s cmd:%s'.center(50, '-') % (args['ip'],time.strftime("%Y-%m-%d %H:%M:%S"),args['cmd']))
                    try:
            
                        self.s.connect(args['ip'], int(args['port']), args['user'],args['passwd'])#连接服务器
                    except Exception as e:
                        print('连接失败,请查看防火墙...')
                    else:
                        stdin, stdout, stderr = self.s.exec_command(args['cmd'])#远程执行命令
                        print(stdout.read().decode())#获取结果
                    l.release()#解锁
                    # time.sleep(1)
                def execute_command(self,ip_info):
                    '''
                    执行相关命令;
                    对Pool对象调用join()方法会等待所有子进程执行完毕，
                    调用join()之前必须先调用close()，
                    调用close()之后就不能继续添加新的Process
                    '''
                    # print('ip_list',ip_info)
                    p = Pool(8)#定义地址池
                    manager = Manager()
                    lock = manager.Lock()#初始化锁、保持进程同步
                    for ip in ip_info:
                        p.apply_async(self.connect_server, args=(lock,ip,))
                    p.close()
                    p.join()
                    print('All subprocesses done.')
                def check_ip(self):
                    '''
                    获取主机相关信息
                    :return:
                    '''
                    if self.data['hostname'] is None:#没有输入主机名的情况 python3 ssh.py -g xxx --cmd xxx
                        if self.data['groupname'] not in self.conf.sections():
                            print('找不到 %s 主机组...'% self.data['groupname'])
                            return False
                        else:
                            k = self.conf.items(self.data['groupname'])
                            if k[0][0] != k[0][1]:#防止 python3 ssh.py -g h1(主机名而不是主机组名) --cmd xxx
                                print('找不到 %s 主机组...'% self.data['groupname'])
                                return False
                            else:
                                #在没有输入主机名的情况下获取主机组里面的每个主机的相关信息
                                for i in self.conf.options(self.data['groupname']):
                                    ip = self.conf.get(i,'ip')
                                    port = self.conf.get(i,'port')
                                    user = self.conf.get(i, 'user')
                                    passwd = self.conf.get(i, 'passwd')
                                    cmd = self.data['cmd']
                                    ip_dict = {
                                        'ip': ip,
                                        'port': port,
                                        'user': user,
                                        'passwd': passwd,
                                        'cmd':cmd
                                    }
                                    self.ip_list.append(ip_dict)
                                return self.ip_list
            
                    elif self.data['groupname'] is None:#没有输入主机组名的情况 python3 ssh.py -H xxx --cmd xxx
                        if self.data['hostname'] not in self.conf.sections():
                            print('找不到 %s 主机...'% self.data['hostname'])
                            return False
                        else:
                            k = self.conf.items(self.data['hostname'])#防止 python3 ssh.py -H server_groups(主机组名而不是主机名) --cmd xxx
                            # print('k', k)
                            if k[0][0] == k[0][1]:
                                print('找不到 %s 主机...' % self.data['hostname'])
                                return False
                            else:
                                # print(self.conf.sections())
                                # print(self.conf.options(self.data['hostname']))
                                #在没有输入主机组名的情况下获取主机相关信息
                                ip = self.conf.get(self.data['hostname'],'ip')
                                port = self.conf.get(self.data['hostname'], 'port')
                                user = self.conf.get(self.data['hostname'], 'user')
                                passwd = self.conf.get(self.data['hostname'], 'passwd')
                                cmd = self.data['cmd']
                                ip_dict ={
                                    'ip':ip,
                                    'port':port,
                                    'user':user,
                                    'passwd':passwd,
                                    'cmd':cmd
                                }
                                self.ip_list.append(ip_dict)
                                return self.ip_list
                    else:
                        #主机名和主机组名都有情况,例如: python3 ssh.py -H h1 -g server_groups --cmd xxx
                        tmp_list = []#定义空列表
                        # print('a')
                        for i in self.data.values():#获取输入的信息
                            if i == self.data['cmd']:continue#过滤掉cmd的信息,只留主机和主机组信息
                            if i not in self.conf.sections():#输入的信息不在主机配置文件中
                                exit('找不到:{}'.format(i))
                            tmp_list.append(i)
                        # print('tmp_list',tmp_list)
                        for list in tmp_list:
                            k = self.conf.items(list)#主机组名里面的key values是相同的
                            if k[0][0] == k[0][1]:
                                for n in k:
                                    if n[0] in tmp_list:#过滤相同主机名
                                        continue
                                    else:
                                        tmp_list.append(n[0])
                                tmp_list.remove(list)
                        # print('after tmp_list', tmp_list)
                        for m in tmp_list:#获取过滤后的主机信息
                            ip = self.conf.get(m, 'ip')
                            port = self.conf.get(m, 'port')
                            user = self.conf.get(m, 'user')
                            passwd = self.conf.get(m, 'passwd')
                            cmd = self.data['cmd']
                            ip_dict = {
                                'ip': ip,
                                'port': port,
                                'user': user,
                                'passwd': passwd,
                                'cmd':cmd
                            }
                            self.ip_list.append(ip_dict)
            
                        return self.ip_list
            
                def check_args(self):
                    if self.options.cmd == None:#没有输入要执行的命令
                        print('请输入要执行的命令')
                        return False
                    if self.options.hostname is None and self.options.groupname is None:#主机和主机组必须要有一个
                        print('请输入要连接的服务器')
                        return False
                    if self.options.groupname is None:#若没有输入主机组
                        data = {
                            'hostname':self.options.hostname,
                            'groupname':None,
                            'cmd': self.options.cmd
                        }
                        return data
                    else:
                        if self.options.hostname is None:#若没有输入主机
                            data = {
                                'hostname': None,
                                'groupname': self.options.groupname,
                                'cmd':self.options.cmd
                            }
                            return data
                        else:#主机和主机组都有输入
                            data = {
                                'hostname': self.options.hostname,
                                'groupname': self.options.groupname,
                                'cmd': self.options.cmd
                            }
                            # print('主机、主机组都有输入')
                            return data
            
            if __name__ == '__main__':
                if len(sys.argv) == 1:
                    print('请执行%s -h'% sys.argv[0])
                else:
                    ssh = SSH()
