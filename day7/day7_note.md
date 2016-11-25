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

##2、面向对象
    概述:
        面向过程：根据业务逻辑从上到下写垒代码
        函数式：将某功能代码封装到函数中，日后便无需重复编写，仅调用函数即可
        面向对象：对函数进行分类和封装，让开发“更快更好更强...
    创建:
        面向对象编程是一种编程方式，此编程方式的落地需要使用 “类” 和 “对象” 来实现，所以，面向对象编程其实就是对 “类” 和 “对象” 的使用。
        类就是一个模板，模板里可以包含多个函数，函数里实现一些功能
        对象则是根据模板创建的实例，通过实例对象可以执行类中的函数
    面向对象三大特性:
        面向对象的三大特性是指：封装、继承和多态。
        一、封装
            封装，顾名思义就是将内容封装到某个地方，以后再去调用被封装在某处的内容。
            所以，在使用面向对象的封装特性时，需要：
            将内容封装到某处
            从某处调用被封装的内容
        二、继承
            继承，面向对象中的继承和现实生活中的继承相同，即：子可以继承父的内容。
            当类是经典类时，多继承情况下，会按照深度优先方式查找
            当类是新式类时，多继承情况下，会按照广度优先方式查找
            经典类和新式类，从字面上可以看出一个老一个新，新的必然包含了跟多的功能，也是之后推荐的写法，从写法上区分的话，如果 当前类或者父类继承了object类，那么该类便是新式类，否则便是经典类
            举例:
                #经典类多继承
                class D:
                    def bar(self):
                        print 'D.bar'
             
                class C(D):                
                    def bar(self):
                        print 'C.bar'
                
                class B(D):                
                    def bar(self):
                        print 'B.bar'
                
                class A(B, C):
                
                    def bar(self):
                        print 'A.bar'                
                a = A()
                # 执行bar方法时
                # 首先去A类中查找，如果A类中没有，则继续去B类中找，如果B类中么有，则继续去D类中找，如果D类中么有，则继续去C类中找，如果还是未找到，则报错
                # 所以，查找顺序：A --> B --> D --> C
                # 在上述查找bar方法的过程中，一旦找到，则寻找过程立即中断，便不会再继续找了
                a.bar()
                #新式类多继承
                class D(object):
                    def bar(self):
                        print 'D.bar'
                               
                class C(D):                
                    def bar(self):
                        print 'C.bar'
                                
                class B(D):                
                    def bar(self):
                        print 'B.bar'
                                
                class A(B, C):                
                    def bar(self):
                        print 'A.bar'                
                a = A()
                # 执行bar方法时
                # 首先去A类中查找，如果A类中没有，则继续去B类中找，如果B类中么有，则继续去C类中找，如果C类中么有，则继续去D类中找，如果还是未找到，则报错
                # 所以，查找顺序：A --> B --> C --> D
                # 在上述查找bar方法的过程中，一旦找到，则寻找过程立即中断，便不会再继续找了
                a.bar()
                经典类：首先去A类中查找，如果A类中没有，则继续去B类中找，如果B类中么有，则继续去D类中找，如果D类中么有，则继续去C类中找，如果还是未找到，则报错
                新式类：首先去A类中查找，如果A类中没有，则继续去B类中找，如果B类中么有，则继续去C类中找，如果C类中么有，则继续去D类中找，如果还是未找到，则报错   
                注意：在上述查找过程中，一旦找到，则寻找过程立即中断，便不会再继续找了
                
                
        三、多态 
            多态性（polymorphisn）是允许你将父对象设置成为和一个或更多的他的子对象相等的技术，赋值之后，父对象就可以根据当前赋值给它的子对象的特性以不同的方式运作。简单的说，就是一句话：允许将子类类型的指针赋值给父类类型的指针。
            那么，多态的作用是什么呢？我们知道，封装可以隐藏实现细节，使得代码模块化；继承可以扩展已存在的代码模块（类）；它们的目的都是为了——代码重用。而多态则是为了实现另一个目的——接口重用！多态的作用，就是为了类在继承和派生的时候，保证使用“家谱”中任一类的实例的某一属性时的正确调用
            Pyhon不直接支持多态，但可以间接实现
            
    类的成员:
        类的成员可以分为三大类：字段、方法和属行
        所有成员中，只有普通字段的内容保存对象中，即：根据此类创建了多少对象，在内存中就有多少个普通字段。而其他的成员，则都是保存在类中，
        即：无论对象的多少，在内存中只创建一份。
        一、字段:
            字段包括：普通字段和静态字段，他们在定义和使用中有所区别，
            而最本质的区别是内存中保存的位置不同，
            普通字段属于对象
            静态字段属于类
        例如:
            class Province(object):
                # 静态字段
                country ＝ '中国'
                def __init__(self, name):
                    # 普通字段
                    self.name = name
            
            # 直接访问普通字段
            obj = Province('河北省')
            print obj.name
            
            # 直接访问静态字段
            Province.country
        二、方法:
            方法包括：普通方法、静态方法和类方法，三种方法在内存中都归属于类，区别在于调用方式不同。
            普通方法：由对象调用；至少一个self参数；执行普通方法时，自动将调用该方法的对象赋值给self；
            类方法：由类调用； 至少一个cls参数；执行类方法时，自动将调用该方法的类复制给cls；
            静态方法：由类调用；无默认参数；
        例如:
            class Foo(object):

                def __init__(self, name):
                    self.name = name
            
                def ord_func(self):
                    """ 定义普通方法，至少有一个self参数 """
            
                    # print self.name
                    print '普通方法'
            
                @classmethod
                def class_func(cls):
                    """ 定义类方法，至少有一个cls参数 """
            
                    print '类方法'
            
                @staticmethod
                def static_func():
                    """ 定义静态方法 ，无默认参数"""
            
                    print '静态方法'
                    
            # 调用普通方法
            f = Foo()
            f.ord_func()
            # 调用类方法
            Foo.class_func()
            # 调用静态方法
            Foo.static_func()
            相同点：对于所有的方法而言，均属于类（非对象）中，所以，在内存中也只保存一份。
            不同点：方法调用者不同、调用方法时自动传入的参数不同
        三、属性:
            也是普通方法的一种,
            属性方法的作用就是通过@property把一个方法变成一个静态属性
            调用的时候无需()