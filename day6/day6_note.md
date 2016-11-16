#今日内容
+   常用模块
+   re正则

##1.1、常用模块之os模块
    模块方法	                        说明
    os.getcwd()	                获取当前工作目录，即当前python脚本工作的目录路径
    os.chdir(“dirname”)	        改变当前脚本工作目录；相当于shell下cd
    os.curdir	                返回当前目录: (‘.’)
    os.pardir	                获取当前目录的父目录字符串名：(‘..’)
    os.makedirs(‘dirname1/dirname2’)	可生成多层递归目录
    os.removedirs(‘dirname1’)	若目录为空，则删除，并递归到上一级目录，如若也为空，则删除，依此类推
    os.mkdir(‘dirname’)	        生成单级目录；相当于shell中mkdir dirname
    os.rmdir(‘dirname’)	        删除单级空目录，若目录不为空则无法删除，报错；相当于shell中rmdir dirname
    os.listdir(‘dirname’)	        列出指定目录下的所有文件和子目录，包括隐藏文件，并以列表方式打印
    os.remove()	                删除一个文件
    os.rename(“oldname”,”newname”)	重命名文件/目录
    os.stat(‘path/filename’)	获取文件/目录信息
    os.sep	                        输出操作系统特定的路径分隔符，win下为\\,Linux下为/
    os.linesep	                输出当前平台使用的行终止符，win下为\t\n,Linux下为\n
    os.pathsep	                输出用于分割文件路径的字符串
    os.name	                        输出字符串指示当前使用平台。win->nt; Linux->posix
    os.system(“bash command”)	运行shell命令，直接显示
    os.environ	                获取系统环境变量
    os.path.abspath(path)	        返回path规范化的绝对路径
    os.path.split(path)	        将path分割成目录和文件名二元组返回
    os.path.dirname(path)	        返回path的目录。其实就是os.path.split(path)的第一个元素
    os.path.basename(path)	        返回path最后的文件名。如何path以／或\结尾，那么就会返回空值。即os.path.split(path)的第二个元素
    os.path.exists(path)	        如果path存在，返回True；如果path不存在，返回False
    os.path.isabs(path)	        如果path是绝对路径，返回True
    os.path.isfile(path)	        如果path是一个存在的文件，返回True。否则返回False
    os.path.isdir(path)	        如果path是一个存在的目录，则返回True。否则返回False
    os.path.join(path1[, path2[,…]])	将多个路径组合后返回，第一个绝对路径之前的参数将被忽略
    os.path.getatime(path)	        返回path所指向的文件或者目录的最后存取时间
    os.path.getmtime(path)	        返回path所指向的文件或者目录的最后修改时间
    常用模块操作过程:
        获取当前路径:
        >>> os.getcwd()
        '/Users/jijianming'
        删除文件
        os.remove(filename)
        该名
        os.rename(oldname,newname)
        判断文件是否存在
        os.path.exists(filename)
        获取文件所在目录
        os.path.dirname(os.path.abspath(filename))
##1.2、常用模块之sys
    模块方法	                    解释说明
    sys.argv	            传递到Python脚本的命令行参数列表，第一个元素是程序本身路径
    sys.executable	        返回Python解释器在当前系统中的绝对路径
    sys.exit([arg])	        程序中间的退出，arg=0为正常退出
    sys.path	            返回模块的搜索路径，初始化时使用PYTHONPATH环境变量的值
    sys.platform	        返回操作系统平台名称，Linux是linux2，Windows是win32
    sys.stdout.write(str)	输出的时候把换行符\n去掉
    val = sys.stdin.readline()[:-1]	拿到的值去掉\n换行符
    sys.version	            获取Python解释程序的版本信息
    操作:
        #获取传参
        sys.argv()
##1.3、常用模块之json
    序列化（Serialization）：将对象的状态信息转换为可以存储或可以通过网络传输的过程，传输的格式可以是JSON、XML等。
    反序列化就是从存储区域（JSON，XML）读取反序列化对象的状态，重新创建该对象。
    
    方法	                    说明
    json.loads(obj)	将字符串序列化成Python的基本数据类型，注意单引号与双引号
    json.dumps(obj)	将Python的基本数据类型序列化成字符串
    json.load(obj)	读取文件中的字符串，序列化成Python的基本数据类型
    json.dump(obj)	将Python的基本数据类型序列化成字符串并写入到文件中
    操作:
        #将字符串序列化成字典
        >>> dict_str = '{"name":"walker","age":22}'
        #注意字符串的引号,要双引号才行,单引号会报错
        >>> print(type(dict_str))
        <class 'str'>
        >>> dict_json = json.loads(dict_str)
        >>> print(type(dict_json))
        <class 'dict'>
        dump、load和dumps、loads使用方法一样,不过是保存在文件中
###1.4、常用模块之time和datetime
    方法名	                            说明
    time.sleep(int)	                等待时间
    time.time()	                    输出时间戳，从1970年1月1号到现在用了多少秒
    time.ctime()	                返回当前的系统时间
    time.gmtime()	                将时间戳转换成struct_time格式
    time.localtime()	                以struct_time格式返回本地时间
    time.mktime(time.localtime())	将struct_time格式转回成时间戳格式
    time.strftime(“%Y-%m-%d %H:%M:%S”,time.gmtime())	将struct_time格式转成指定的字符串格式
    time.strptime(“2016-01-28”,”%Y-%m-%d”)	将字符串格式转换成struct_time格式
    datetime.date.today()	        打印输出当前的系统日期
    datetime.date.fromtimestamp(time.time())	将时间戳转成日期格式
    datetime.datetime.now()	        打印当前的系统时间
    current_time.replace(2016,5,12)	返回当前时间,但指定的值将被替换
    datetime.datetime.strptime(“21/11/06 16:30”, “%d/%m/%y %H:%M”)	将字符串转换成日期格式
    
    print(time.clock())            #返回处理器时间,3.3开始已废弃 , 改成了time.process_time()测量处理器运算时间,不包括sleep时间,不稳定,mac上测不出来
    print(time.altzone)           #返回与utc时间的时间差,以秒计算\
    print(time.asctime())         #返回时间格式"Fri Aug 19 11:14:16 2016",
    print(time.localtime())       #返回本地时间 的struct time对象格式
    print(time.gmtime(time.time()-800000)) #返回utc时间的struc时间对象格式
    
    print(time.asctime(time.localtime())) #返回时间格式"Fri Aug 19 11:14:16 2016",
    print(time.ctime())            #返回Fri Aug 19 12:38:29 2016 格式, 同上
    
    #日期字符串 转成  时间戳
    string_2_struct = time.strptime("2016/05/22","%Y/%m/%d") #将 日期字符串 转成 struct时间对象格式
    print(string_2_struct)
    struct_2_stamp = time.mktime(string_2_struct) #将struct时间对象转成时间戳
    print(struct_2_stamp) 
    #将时间戳转为字符串格式
    print(time.gmtime(time.time()-86640)) #将utc时间戳转换成struct_time格式
    print(time.strftime("%Y-%m-%d %H:%M:%S",time.gmtime()) ) #将utc struct_time格式转成指定的字符串格式
    #时间加减
    import datetime    
    print(datetime.datetime.now()) #返回 2016-08-19 12:47:03.941925
    print(datetime.date.fromtimestamp(time.time()) )  # 时间戳直接转成日期格式 2016-08-19
    print(datetime.datetime.now() )
    print(datetime.datetime.now() + datetime.timedelta(3)) #当前时间+3天
    print(datetime.datetime.now() + datetime.timedelta(-3)) #当前时间-3天
    print(datetime.datetime.now() + datetime.timedelta(hours=3)) #当前时间+3小时
    print(datetime.datetime.now() + datetime.timedelta(minutes=30)) #当前时间+30分
    c_time  = datetime.datetime.now()
    print(c_time.replace(minute=3,hour=2)) #时间替换
    操作:
        #取当前时间
        >>> time.strftime('%Y-%m-%d %H:%M:%S')
        '2016-11-16 12:22:47'
##1.5、常用模块之