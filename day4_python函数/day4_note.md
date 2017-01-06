#今日内容
+   函数

###函数的定义和特性
    定义:
        函数是指将一组语句的集合通过一个名字(函数名)封装起来，要想执行这个函数，只需调用其函数名即可
        函数是组织好的，可重复使用的，用来实现单一，或相关联功能的代码段。
    特性:
        减少重复代码
        使程序变的可扩展
        使程序变得易维护
        
        
###函数的语法
    Python 定义函数使用 def 关键字，一般格式如下：
        def 函数名（参数列表）:
            函数体
        默认情况下，参数值和参数名称是按函数声明中定义的的顺序匹配起来的。
    实例:
        # x为函数的参数
        >>> def num(x):
        ...  print(x)
        ...
        # 1等于x
        >>> num("1")
        1
            
###函数参数与局部变量
    形参:变量只有在被调用时才分配内存单元，在调用结束时，即刻释放所分配的内存单元。因此，形参只在函数内部有效。函数调用结束返回主调用函数后则不能再使用该形参变量
    实参:可以是常量、变量、表达式、函数等，无论实参是何种类型的量，在进行函数调用时，它们都必须有确定的值，以便把这些值传送给形参。因此应预先用赋值，输入等办法使参数获得确定值
    例如:
        def Name(x)#x就是形参
            print(x)
            
        Name('zhangsan')#zhangsan就是实参
        zhangsan
    默认参数:
    调用函数时，如果没有传递参数，则会使用默认参数。默认参数必须放在形参的后面,
    以下实例中如果没有传入 age 参数，则使用默认值
    例如:
        def printinfo( name, age = 35 ):#age就是默认参数
           "打印任何传入的字符串"
           print ("名字: ", name);
           print ("年龄: ", age);
           return;
         
        #调用printinfo函数
        printinfo('zhangsan');#当调用时不传age值时、会使用默认值
        print ("------------------------")
        printinfo('zhangsan',20);#传时、会使用传的值
    结果:
        名字:  zhangsan
        年龄:  35
        ------------------------
        名字:  zhangsan
        年龄:  20
    关键参数:
    正常情况下，给函数传参数要按顺序，不想按顺序就可以用关键参数，只需指定参数名即可，但记住一个要求就是，关键参数必须放在位置参数之后。
    
    例如:
        def printinfo(name,age):
            print ("名字: ", name);
            print ("年龄: ", age);
            return;
        printinfo(22,name='zhangsan')#关键字参数必须在位置参数的后面
        printinfo('zhangsan',22)#默认是这样的
    结果:
        名字:  zhangsan
        年龄:  22
        ------------------------
        名字:  zhangsan
        年龄:  22
    非固定参数:
    若你的函数在定义时不确定用户想传入多少个参数，就可以使用非固定参数。
    
    例如:
        def stu_register(name,age,*args): # *args 会把多传入的参数变成一个元组形式
            print(name,age,args)
     
        stu_register("Alex",22)
        #输出
        #Alex 22 () #后面这个()就是args,只是因为没传值,所以为空
         
        stu_register("Jack",32,"CN","Python")
        #输出
        # Jack 32 ('CN', 'Python')
    还可以有一个**kwargs
        def stu_register(name,age,*args,**kwargs): # *kwargs 会把多传入的参数变成一个dict形式
            print(name,age,args,kwargs)
 
        stu_register("Alex",22)
        #输出
        #Alex 22 () {}#后面这个{}就是kwargs,只是因为没传值,所以为空
         
        stu_register("Jack",32,"CN","Python",sex="Male",province="ShanDong")
        #输出
        # Jack 32 ('CN', 'Python') {'province': 'ShanDong', 'sex': 'Male'}
    
    局部变量:
    例如:
        name = "Alex Li"
 
        def change_name(name):
            print("before change:",name)
            name = "金角大王,一个有Tesla的男人"
            print("after change", name)
  
        change_name(name)   
        print("在外面看看name改了么?",name)
    结果:
        before change: Alex Li
        after change 金角大王,一个有Tesla的男人
        在外面看看name改了么? Alex Li
    
    全局与局部变量:
        在子程序中定义的变量称为局部变量，在程序的一开始定义的变量称为全局变量。
        全局变量作用域是整个程序，局部变量作用域是定义该变量的子程序。
        当全局变量与局部变量同名时：
        在定义局部变量的子程序内，局部变量起作用；在其它地方全局变量起作用
   
###函数返回值
    要想获取函数的执行结果，就可以用return语句把结果返回
    注意:
    函数在执行过程中只要遇到return语句，就会停止执行并返回结果，so 也可以理解为 return 语句代表着函数的结束
    如果未在函数中指定return,那这个函数的返回值为None 
###匿名函数
    python 使用 lambda 来创建匿名函数。
    所谓匿名，意即不再使用 def 语句这样标准的形式定义一个函数。
    lambda 只是一个表达式，函数体比 def 简单很多。
    lambda的主体是一个表达式，而不是一个代码块。仅仅能在lambda表达式中封装有限的逻辑进去。
    lambda 函数拥有自己的命名空间，且不能访问自有参数列表之外或全局命名空间里的参数。
    虽然lambda函数看起来只能写一行，却不等同于C或C++的内联函数，后者的目的是调用小函数时不占用栈内存从而增加运行效率。
    语法:
        lambda 函数的语法只包含一个语句，如下：
        lambda [arg1 [,arg2,.....argn]]:expression
    实例：
        sum = lambda arg1, arg2: arg1 + arg2
        # 调用sum函数
        print ("相加后的值为 : ", sum( 10, 20 ))
        print ("相加后的值为 : ", sum( 20, 20 ))
    结果：
        相加后的值为 :  30
        相加后的值为 :  40
###内部函数
    这个以后单独写一篇博文。
