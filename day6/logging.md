##logging模块
    概述:
        在实际项目中,需要对一些数据进行日志记录,并将日志记录到不同的存储单元中,例如数据库,文本,或者推送到图形化界面中,当需要时发现自己实现一个日志
        库其实是要很大的代价,因此,第三方的日志库上进行定制化处理 正文内容是对logging的理解和使用方式,非常方便
    四个主要类:
        logger      提供了应用程序可以直接使用的接口  
        handler     将(logger创建的)日志记录发送到合适的目的输出; 
        filter      提供了细度设备来决定输出哪条日志记录;用处不太大
        formatter   决定日志记录的最终输出格式
    模块级函数:
        logging.getLogger([name]): #返回一个logger对象,如果没有指定名字将返回root logger,最常用
        logging.basicConfig(): #给logger对象的配置管理函数 ,不常用 
        logging.debug()、logging.info()、logging.warning()、logging.error()、logging.critical(): #logger的日志级别
    level有以下几个级别:
        NOTSET < DEBUG < INFO < WARNING < ERROR < CRITICAL
    最简单用法:
        import logging
        logging.warning("user [alex] attempted wrong password more than 3 times")
        logging.critical("server is down")
         
        #输出
        WARNING:root:user [alex] attempted wrong password more than 3 times
        CRITICAL:root:server is down
    输出到文件:
        import logging
        logging.basicConfig(filename='example.log',level=logging.INFO)
        logging.debug('This message should go to the log file')
        logging.info('So should this')
        logging.warning('And this, too')
        其中下面这句中的level=loggin.INFO意思是，把日志纪录级别设置为INFO，
        也就是说，只有比日志是INFO或比INFO级别更高的日志才会被纪录到文件里，
        在这个例子， 第一条日志是不会被纪录的，如果希望纪录debug的日志，那把日志级别改成DEBUG就行了。
    
    感觉上面的日志格式忘记加上时间啦，日志不知道时间怎么行呢，
    下面就来加上!
        import logging
        logging.basicConfig(format='%(asctime)s %(message)s', datefmt='%m/%d/%Y %I:%M:%S %p')
        logging.warning('is when this event was logged.')
         
    输出
        12/12/2010 11:46:36 AM is when this event was logged.
    常用的格式:   
        %(name)s        #Logger的名字
        %(levelno)s     #数字形式的日志级        
        %(levelname)s   #文本形式的日志级别
        %(pathname)s    #调用日志输出函数的模块的完整路径名，可能没有
        %(filename)s    #调用日志输出函数的模块的文件名
        %(module)s      #调用日志输出函数的模块名   
        %(funcName)s    #调用日志输出函数的函数名
        %(lineno)d      #调用日志输出函数的语句所在的代码行
        %(created)f     #当前时间，用UNIX标准的表示时间的浮 点数表示
        %(relativeCreated)d     #输出日志信息时的，自Logger创建以 来的毫秒数
        %(asctime)s     #字符串形式的当前时间。默认格式是 “2003-07-08 16:49:45,896”。逗号后面的是毫秒
        %(thread)d      #线程ID。可能没有
        %(threadName)s  #线程名。可能没有
        %(process)d     #进程ID。可能没有
        %(message)s     #用户输出的消息
    四个主要类介绍:
        logger
        
        每个程序在输出信息之前都要获得一个Logger。Logger通常对应了程序的模块名，比如聊天工具的图形界面模块可以这样获得它的Logger：
        LOG=logging.getLogger(”chat.gui”)
        而核心模块可以这样：
        LOG=logging.getLogger(”chat.kernel”)
        
        Logger.setLevel(lel):指定最低的日志级别，低于lel的级别将被忽略。debug是最低的内置级别，critical为最高
        Logger.addFilter(filt)、Logger.removeFilter(filt):添加或删除指定的filter
        Logger.addHandler(hdlr)、Logger.removeHandler(hdlr)：增加或删除指定的handler
        Logger.debug()、Logger.info()、Logger.warning()、Logger.error()、Logger.critical()：可以设置的日志级别

        handler
        
        handler对象负责发送相关的信息到指定目的地。Python的日志系统有多种Handler可以使用。有些Handler可以把信息输出到控制台，
        有些Logger可以把信息输出到文件，还有些 Handler可以把信息发送到网络上。如果觉得不够用，还可以编写自己的Handler。可以通过addHandler()方法添加多个多handler
        Handler.setLevel(lel):指定被处理的信息级别，低于lel级别的信息将被忽略
        Handler.setFormatter()：给这个handler选择一个格式
        Handler.addFilter(filt)、Handler.removeFilter(filt)：新增或删除一个filter对象
        
        
        每个Logger可以附加多个Handler。接下来我们就来介绍一些常用的Handler：
        1) logging.StreamHandler
        使用这个Handler可以向类似与sys.stdout或者sys.stderr的任何文件对象(file object)输出信息。它的构造函数是：
        StreamHandler([strm])
        其中strm参数是一个文件对象。默认是sys.stderr
        
        
        2) logging.FileHandler
        和StreamHandler类似，用于向一个文件输出日志信息。不过FileHandler会帮你打开这个文件。它的构造函数是：
        FileHandler(filename[,mode])
        filename是文件名，必须指定一个文件名。
        mode是文件的打开方式。参见Python内置函数open()的用法。默认是’a'，即添加到文件末尾。
        
        3) logging.handlers.RotatingFileHandler
        这个Handler类似于上面的FileHandler，但是它可以管理文件大小。当文件达到一定大小之后，它会自动将当前日志文件改名，
        然后创建 一个新的同名日志文件继续输出。比如日志文件是chat.log。当chat.log达到指定的大小之后，RotatingFileHandler自动把 文件改名为chat.log.1。不过，如果chat.log.1已经存在，会先把chat.log.1重命名为chat.log.2。。。最后重新创建 chat.log，继续输出日志信息。它的构造函数是：
        RotatingFileHandler( filename[, mode[, maxBytes[, backupCount]]])
        其中filename和mode两个参数和FileHandler一样。
        maxBytes用于指定日志文件的最大文件大小。如果maxBytes为0，意味着日志文件可以无限大，这时上面描述的重命名过程就不会发生。
        backupCount用于指定保留的备份文件的个数。比如，如果指定为2，当上面描述的重命名过程发生时，原有的chat.log.2并不会被更名，而是被删除。

        4) logging.handlers.TimedRotatingFileHandler
        这个Handler和RotatingFileHandler类似，不过，它没有通过判断文件大小来决定何时重新创建日志文件，而是间隔一定时间就 自动创建新的日志文件。重命名的过程与RotatingFileHandler类似，不过新的文件不是附加数字，而是当前时间。它的构造函数是：
        TimedRotatingFileHandler( filename [,when [,interval [,backupCount]]])
        其中filename参数和backupCount参数和RotatingFileHandler具有相同的意义。
        interval是时间间隔。
        when参数是一个字符串。表示时间间隔的单位，不区分大小写。它有以下取值：
        S 秒
        M 分
        H 小时
        D 天
        W 每星期（interval==0时代表星期一）
        midnight 每天凌晨
    例子:(同时在屏幕和文件输出)
        import logging
        #create logger
        logger = logging.getLogger('TEST-LOG')
        logger.setLevel(logging.DEBUG)
         
         
        # create console handler and set level to debug
        ch = logging.StreamHandler()
        ch.setLevel(logging.DEBUG)
         
        # create file handler and set level to warning
        fh = logging.FileHandler("access.log")
        fh.setLevel(logging.WARNING)
        # create formatter
        formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')
         
        # add formatter to ch and fh
        ch.setFormatter(formatter)
        fh.setFormatter(formatter)
         
        # add ch and fh to logger
        logger.addHandler(ch)
        logger.addHandler(fh)
         
        # 'application' code
        logger.debug('debug message')
        logger.info('info message')
        logger.warn('warn message')
        logger.error('error message')
        logger.critical('critical message')
    例子:(日志自动切割)
        import logging
        from logging import handlers
        logger = logging.getLogger(__name__)
        
        log_file = "timelog.log"
        #按大小分割
        #fh = handlers.RotatingFileHandler(filename=log_file,maxBytes=10,backupCount=3)
        #按时间分割
        fh = handlers.TimedRotatingFileHandler(filename=log_file,when="S",interval=5,backupCount=3)
        formatter = logging.Formatter('%(asctime)s %(module)s:%(lineno)d %(message)s')
        fh.setFormatter(formatter)
        logger.addHandler(fh)
        
        logger.warning("test1")
        logger.warning("test12")
        logger.warning("test13")
        logger.warning("test14")