##本周内容
+   异常处理
+   反射
+   socket
##1、异常处理
    try:
        #主代码块
        pass
    except KeyError,e:
        # 异常时，执行该块
        pass
    else:
        # 主代码块执行完，执行该块
        pass
    finally:
        # 无论异常与否，最终执行该块
        pass
    python常用的异常:
    异常名称	                描述
    BaseException	        所有异常的基类
    SystemExit	            解释器请求退出
    KeyboardInterrupt	    用户中断执行(通常是输入^C)
    Exception	            常规错误的基类
    StopIteration	        迭代器没有更多的值
    GeneratorExit	        生成器(generator)发生异常来通知退出
    StandardError	        所有的内建标准异常的基类
    ArithmeticError	        所有数值计算错误的基类
    FloatingPointError	    浮点计算错误
    OverflowError	        数值运算超出最大限制
    ZeroDivisionError	    除(或取模)零 (所有数据类型)
    AssertionError	        断言语句失败
    AttributeError	        对象没有这个属性
    EOFError	            没有内建输入,到达EOF 标记
    EnvironmentError	    操作系统错误的基类
    IOError	                输入/输出操作失败
    OSError	                操作系统错误
    WindowsError	        系统调用失败
    ImportError	            导入模块/对象失败
    LookupError	            无效数据查询的基类
    IndexError	            序列中没有此索引(index)
    KeyError	            映射中没有这个键
    MemoryError	            内存溢出错误(对于Python 解释器不是致命的)
    NameError	            未声明/初始化对象 (没有属性)
    UnboundLocalError	    访问未初始化的本地变量
    ReferenceError	        弱引用(Weak reference)试图访问已经垃圾回收了的对象
    RuntimeError	        一般的运行时错误
    NotImplementedError	    尚未实现的方法
    SyntaxError	Python      语法错误
    IndentationError	    缩进错误
    TabError	            Tab 和空格混用
    SystemError	            一般的解释器系统错误
    TypeError	            对类型无效的操作
    ValueError	            传入无效的参数
    UnicodeError	        Unicode 相关的错误
    UnicodeDecodeError	    Unicode 解码时的错误
    UnicodeEncodeError	    Unicode 编码时错误
    UnicodeTranslateError	Unicode 转换时错误
    Warning	                警告的基类
    DeprecationWarning	    关于被弃用的特征的警告
    FutureWarning	        关于构造将来语义会有改变的警告
    OverflowWarning	        旧的关于自动提升为长整型(long)的警告
    PendingDeprecationWarning	关于特性将会被废弃的警告
    RuntimeWarning	        可疑的运行时行为(runtime behavior)的警告
    SyntaxWarning	        可疑的语法的警告
    UserWarning	            用户代码生成的警告
##反射:
    python中的反射功能是由以下四个内置函数提供：hasattr、getattr、setattr、delattr，改四个函数分别用于对对象内部执行：
    检查是否含有某成员、获取成员、设置成员、删除成员
    反射是通过字符串的形式操作对象相关的成员
    hasatter(容器，‘名称’)           #以字符串的形式判断某个对象中是否含有指定的属性
    getatter(容器，‘名称’)           #以字符串的形式去某个对象获取指定的属性
    setatter(容器，‘名称’，值)       #以字符串的形式去某个对象设置指定的属性
    delatter(容器，‘名称’)           #以字符串的形式去某个对象删除指定的属性
    例如:
        class Foo(object):
            staticField = "old boy"
            def __init__(self):
                self.name = 'wupeiqi'
            def func(self):
                return 'func'
            @staticmethod
            def bar():
                return 'bar'
                
        print (getattr(Foo, 'staticField'))
        print (getattr(Foo, 'func'))
        print (getattr(Foo, 'bar'))
        
##3、socket
    socket通常也称作"套接字"，用于描述IP地址和端口，是一个通信链的句柄，应用程序通常通过"套接字"向网络发出请求或者应答网络请求。
    socket起源于Unix，而Unix/Linux基本哲学之一就是“一切皆文件”，对于文件用【打开】【读写】【关闭】模式来操作。socket就是该模式的一个实现，socket即是一种特殊的文件，
    一些socket函数就是对其进行的操作（读/写IO、打开、关闭）
    socket和file的区别：
        file模块是针对某个指定文件进行【打开】【读写】【关闭】
        socket模块是针对 服务器端 和 客户端Socket 进行【打开】【读写】【关闭】
    功能:
        sk = socket.socket(socket.AF_INET,socket.SOCK_STREAM,0)
            参数一：地址簇
    
            　　socket.AF_INET IPv4（默认）
            　　socket.AF_INET6 IPv6
            
            　　socket.AF_UNIX 只能够用于单一的Unix系统进程间通信
            
            参数二：类型
            
            　　socket.SOCK_STREAM　　流式socket , for TCP （默认）
            　　socket.SOCK_DGRAM　　 数据报式socket , for UDP
            
            　　socket.SOCK_RAW 原始套接字，普通的套接字无法处理ICMP、IGMP等网络报文，而SOCK_RAW可以；其次，SOCK_RAW也可以处理特殊的IPv4报文；此外，利用原始套接字，可以通过IP_HDRINCL套接字选项由用户构造IP头。
            　　socket.SOCK_RDM 是一种可靠的UDP形式，即保证交付数据报但不保证顺序。SOCK_RAM用来提供对原始协议的低级访问，在需要执行某些特殊操作时使用，如发送ICMP报文。SOCK_RAM通常仅限于高级用户或管理员运行的程序使用。
            　　socket.SOCK_SEQPACKET 可靠的连续数据包服务
            
            参数三：协议
            
            　　0　　（默认）与特定的地址家族相关的协议,如果是 0 ，则系统就会根据地址格式和套接类别,自动选择一个合适的协议
        
        sk.bind(address)        
            将套接字绑定到地址。address地址的格式取决于地址族。在AF_INET下，以元组（host,port）的形式表示地址。        
        sk.listen(backlog)        
            开始监听传入连接。backlog指定在拒绝连接之前，可以挂起的最大连接数量。        
            backlog等于5，表示内核已经接到了连接请求，但服务器还没有调用accept进行处理的连接个数最大为5
            这个值不能无限大，因为要在内核中维护连接队列        
        sk.setblocking(bool)        
            是否阻塞（默认True），如果设置False，那么accept和recv时一旦无数据，则报错。        
        sk.accept()        
            接受连接并返回（conn,address）,其中conn是新的套接字对象，可以用来接收和发送数据。address是连接客户端的地址。        
            接收TCP 客户的连接（阻塞式）等待连接的到来   
        sk.connect(address)        
            连接到address处的套接字。一般，address的格式为元组（hostname,port）,如果连接出错，返回socket.error错误。        
        sk.connect_ex(address)        
            同上，只不过会有返回值，连接成功时返回 0 ，连接失败时候返回编码，例如：10061        
        sk.close()        
            关闭套接字        
        sk.recv(bufsize[,flag])        
            接受套接字的数据。数据以字符串形式返回，bufsize指定最多可以接收的数量。flag提供有关消息的其他信息，通常可以忽略。        
        sk.recvfrom(bufsize[.flag])        
            与recv()类似，但返回值是（data,address）。其中data是包含接收数据的字符串，address是发送数据的套接字地址。        
        sk.send(string[,flag])        
            将string中的数据发送到连接的套接字。返回值是要发送的字节数量，该数量可能小于string的字节大小。即：可能未将指定内容全部发送。        
        sk.sendall(string[,flag])        
            将string中的数据发送到连接的套接字，但在返回之前会尝试发送所有数据。成功返回None，失败则抛出异常。        
            内部通过递归调用send，将所有内容发送出去。        
        sk.sendto(string[,flag],address)        
            将数据发送到套接字，address是形式为（ipaddr，port）的元组，指定远程地址。返回值是发送的字节数。该函数主要用于UDP协议。        
        sk.settimeout(timeout)        
            设置套接字操作的超时期，timeout是一个浮点数，单位是秒。值为None表示没有超时期。一般，超时期应该在刚创建套接字时设置，
            因为它们可能用于连接的操作（如 client 连接最多等待5s ）        
        sk.getpeername()        
            返回连接套接字的远程地址。返回值通常是元组（ipaddr,port）。        
        sk.getsockname()        
            返回套接字自己的地址。通常是一个元组(ipaddr,port)        
        sk.fileno()        
            套接字的文件描述符