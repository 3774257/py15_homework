#今日内容
+   subprocess模块
+   面向对象

##1、subprocess的使用
    call #执行命令，返回状态码
        ret = subprocess.call(["ls", "-l"], shell=False)
        ret = subprocess.call("ls -l", shell=True)
    
    check_call  #执行命令，如果执行状态码是 0 ，则返回0，否则抛异常
        subprocess.check_call(["ls", "-l"])
        subprocess.check_call("exit 1", shell=True)
    
    check_output    #执行命令，如果状态码是 0 ，则返回执行结果，否则抛异常
        subprocess.check_output(["echo", "Hello World!"])
        subprocess.check_output("exit 1", shell=True)
    
    subprocess.Popen(...)   #用于执行复杂的系统命令
    参数：
        args：shell命令，可以是字符串或者序列类型（如：list，元组）
        bufsize：指定缓冲。0 无缓冲,1 行缓冲,其他 缓冲区大小,负值 系统缓冲
        stdin, stdout, stderr：分别表示程序的标准输入、输出、错误句柄
        preexec_fn：只在Unix平台下有效，用于指定一个可执行对象（callable object），它将在子进程运行之前被调用
        close_sfs：在windows平台下，如果close_fds被设置为True，则新创建的子进程将不会继承父进程的输入、输出、错误管道。
        所以不能将close_fds设置为True同时重定向子进程的标准输入、输出与错误(stdin, stdout, stderr)。
        shell：同上
        cwd：用于设置子进程的当前目录
        env：用于指定子进程的环境变量。如果env = None，子进程的环境变量将从父进程中继承。
        universal_newlines：不同系统的换行符不同，True -> 同意使用 \n
        startupinfo与createionflags只在windows下有效
        将被传递给底层的CreateProcess()函数，用于设置子进程的一些属性，如：主窗口的外观，进程的优先级等等 
    
    import subprocess
    ret1 = subprocess.Popen(["mkdir","t1"])
    ret2 = subprocess.Popen("mkdir t2", shell=True)
