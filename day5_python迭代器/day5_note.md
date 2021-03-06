#今日内容
+   迭代器&生成器
+   装饰器
+   json和pickle数据序列化

###1.1、迭代器的介绍:
    迭代器是访问集合内元素的一种方式。迭代器对象从集合的第一个元素开始访问，直到所有的元素都被访问一遍后结束。
    迭代器不能回退，只能往前进行迭代。这并不是什么很大的缺点，因为人们几乎不需要在迭代途中进行回退操作。  
    对于原生支持随机访问的数据结构（如tuple、list），迭代器和经典for循环的索引访问相比并无优势，反而丢失了索引值（可以使用内建函数enumerate()找回这个索引值，这是后话）。但对于无法随机访问的数据结构（比如set）而言，迭代器是唯一的访问元素的方式。  
    迭代器的另一个优点就是它不要求你事先准备好整个迭代过程中所有的元素。迭代器仅仅在迭代至某个元素时才计算该元素，而在这之前或之后，元素可以不存在或者被销毁。这个特点使得它特别适合用于遍历一些巨大的或是无限的集合，比如几个G的文件，或是斐波那契数列等等。这个特点被称为延迟计算或惰性求值(Lazy evaluation)。    
    迭代器更大的功劳是提供了一个统一的访问集合的接口。只要是实现了__iter__()方法的对象，就可以使用迭代器进行访问。
###1.2、迭代器的使用:
    使用内建的函数iter(iterable)可以获取迭代器对象
    演示:
        >>> lst = range(10)
        >>> lst
        range(0, 10)
        >>> lst = iter(lst)
        >>> lst
        <range_iterator object at 0x10109c360>
    使用迭代器的next()方法可以访问下一个元素
    演示:
        >>> lst.__next__()
        0
        >>> next(lst)
        1
        >>> lst.__next__()
        2
        >>> next(lst)
        3
###1.3、生成器的介绍:
    通过列表生成式，我们可以直接创建一个列表。但是，受到内存限制，列表容量肯定是有限的。而且，创建一个包含100万个元素的列表，不仅占用很大的存储空间，如果我们仅仅需要访问前面几个元素，那后面绝大多数元素占用的空间都白白浪费了。
    所以，如果列表元素可以按照某种算法推算出来，那我们是否可以在循环的过程中不断推算出后续的元素呢？这样就不必创建完整的list，从而节省大量的空间。在Python中，这种一边循环一边计算的机制，称为生成器：generator。
    
###1.4、生成器的使用:
    要创建一个generator，有很多种方法。第一种方法很简单，只要把一个列表生成式的[]改成()，就创建了一个generator：
    演示:
        >>> L = [x * x for x in range(10)]
        >>> L
        [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
        >>> g = (x * x for x in range(10))
        >>> g
        <generator object <genexpr> at 0x1022ef630>
    创建L和g的区别仅在于最外层的[]和()，L是一个list，而g是一个generator。
    我们可以直接打印出list的每一个元素，但我们怎么打印出generator的每一个元素呢？
    如果要一个一个打印出来，可以通过next()函数获得generator的下一个返回值：
    演示:
        >>> next(g)
        0
        >>> next(g)
        1
        >>> next(g)
        4
        >>> next(g)
        9
        >>> next(g)
        16
        >>> next(g)
        25
        >>> next(g)
        36
        >>> next(g)
        49
        >>> next(g)
        64
        >>> next(g)
        81
        >>> next(g)
        Traceback (most recent call last):
          File "<stdin>", line 1, in <module>
        StopIteration
    我们讲过，generator保存的是算法，每次调用next(g)，就计算出g的下一个元素的值，直到计算到最后一个元素，没有更多的元素时，抛出StopIteration的错误。
    斐波拉契数列用列表生成式写不出来，但是，用函数把它打印出来却很容易：
    演示:
        def fib(max):
        n, a, b = 0, 0, 1
        while n < max:
            print(b)
            a, b = b, a + b
            n = n + 1
        return 'done'
    结果:
        >>> fib(10)
        1
        1
        2
        3
        5
        8
        13
        21
        34
        55
        done
    仔细观察，可以看出，fib函数实际上是定义了斐波拉契数列的推算规则，可以从第一个元素开始，推算出后续任意的元素，这种逻辑其实非常类似generator
    也就是说，上面的函数和generator仅一步之遥。要把fib函数变成generator，只需要把print(b)改为yield b就可以了：
    def fib(max):
    n,a,b = 0,0,1

    while n < max:
        #print(b)
        yield  b
        a,b = b,a+b

        n += 1

    return 'done'
    这就是定义generator的另一种方法。如果一个函数定义中包含yield关键字，那么这个函数就不再是一个普通函数，而是一个generator：
    >>> f = fib(6)
    >>> f
    <generator object fib at 0x104feaaa0>
    这里，最难理解的就是generator和函数的执行流程不一样。函数是顺序执行，遇到return语句或者最后一行函数语句就返回。而变成generator的函数，在每次调用next()的时候执行，遇到yield语句返回，再次执行时从上次返回的yield语句处继续执行。
    例如:
        data = fib(10)
        print(data)
        
        print(data.__next__())
        print(data.__next__())
        print("干点别的事")
        print(data.__next__())
        print(data.__next__())
        print(data.__next__())
        print(data.__next__())
        print(data.__next__())
        
        #输出
        <generator object fib at 0x101be02b0>
        1
        干点别的事
        3
        8
###2.1、装饰器:
    通过一个小说就知道了:
    你是一家视频网站的后端开发工程师，你们网站有以下几个版块
    def home():
        print("---首页----")
     
    def america():
        print("----欧美专区----")
     
    def japan():
        print("----日韩专区----")
     
    def henan():
        print("----河南专区----")
    视频刚上线初期，为了吸引用户，你们采取了免费政策，所有视频免费观看，迅速吸引了一大批用户，免费一段时间后，每天巨大的带宽费用公司承受不了了，所以准备对比较受欢迎的几个版块收费，其中包括“欧美” 和 “河南”专区，你拿到这个需求后，想了想，想收费得先让其进行用户认证，认证通过后，再判定这个用户是否是VIP付费会员就可以了，是VIP就让看，不是VIP就不让看就行了呗。 你觉得这个需求很是简单，因为要对多个版块进行认证，那应该把认证功能提取出来单独写个模块，然后每个版块里调用 就可以了，与是你轻轻的就实现了下面的功能 。
    #_*_coding:utf-8_*_
    user_status = False #用户登录了就把这个改成True
    def login():
        _username = "alex" #假装这是DB里存的用户信息
        _password = "abc!23" #假装这是DB里存的用户信息
        global user_status
        if user_status == False:
            username = input("user:")
            password = input("pasword:")     
            if username == _username and password == _password:
                print("welcome login....")
                user_status = True
            else:
                print("wrong username or password!")
        else:
            print("用户已登录，验证通过...")
     
    def home():
        print("---首页----")
     
    def america():
        login() #执行前加上验证
        print("----欧美专区----")
     
    def japan():
        print("----日韩专区----")
     
    def henan():
        login() #执行前加上验证
        print("----河南专区----")
     
     
     
    home()
    america()
    henan()
    此时你信心满满的把这个代码提交给你的TEAM LEADER审核，没成想，没过5分钟，代码就被打回来了， TEAM LEADER给你反馈是，
    我现在有很多模块需要加认证模块，你的代码虽然实现了功能，但是需要更改需要加认证的各个模块的代码，这直接违反了软件开发中的一个原则“开放-封闭”原则，
    简单来说，它规定已经实现的功能代码不允许被修改，但可以被扩展，即:
    封闭：已实现的功能代码块
    开放：对扩展开发
    这个原则你还是第一次听说，我擦，再次感受了自己这个野生程序员与正规军的差距，BUT ANYWAY,老大要求的这个怎么实现呢？
    如何在不改原有功能代码的情况下加上认证功能呢？你一时想不出思路，只好带着这个问题回家继续憋，媳妇不在家，去隔壁老王家串门了，你正好落的清静，一不小心就想到了解决方案，不改源代码可以呀，    
    你师从沙河金角大王时，记得他教过你，高阶函数，就是把一个函数当做一个参数传给另外一个函数，当时大王说，有一天，你会用到它的，没想到这时这个知识点突然从脑子 里蹦出来了，我只需要写个认证方法，
    每次调用 需要验证的功能 时，直接 把这个功能 的函数名当做一个参数 传给 我的验证模块不就行了么，哈哈，机智如我，如是你啪啪啪改写了之前的代码 
    #_*_coding:utf-8_*_
    user_status = False #用户登录了就把这个改成True    
    def login(func): #把要执行的模块从这里传进来
        _username = "alex" #假装这是DB里存的用户信息
        _password = "abc!23" #假装这是DB里存的用户信息
        global user_status     
        if user_status == False:
            username = input("user:")
            password = input("pasword:")
     
            if username == _username and password == _password:
                print("welcome login....")
                user_status = True
            else:
                print("wrong username or password!")
     
        if user_status == True:
            func() # 看这里看这里，只要验证通过了，就调用相应功能
     
    def home():
        print("---首页----")
     
    def america():
        #login() #执行前加上验证
        print("----欧美专区----")
     
    def japan():
        print("----日韩专区----")
     
    def henan():
        #login() #执行前加上验证
        print("----河南专区----")
    home()
    login(america) #需要验证就调用 login，把需要验证的功能 当做一个参数传给login
    # home()
    # america()
    login(henan)
    你很开心，终于实现了老板的要求，不改变原功能代码的前提下，给功能加上了验证，此时，媳妇回来了，
    后面还跟着老王，你两家关系 非常 好，老王经常来串门，老王也是码农，你跟他分享了你写的代码，兴奋的等他看完 夸奖你NB,没成想，老王看后，并没有夸你，
    抱起你的儿子，笑笑说，你这个代码还是改改吧， 要不然会被开除的，WHAT? 会开除，明明实现了功能 呀， 老王讲，没错，你功能 是实现了，但是你又犯了一个大忌，什么大忌？     
    你改变了调用方式呀， 想一想，现在没每个需要认证的模块，都必须调用你的login()方法，并把自己的函数名传给你，
    人家之前可不是这么调用 的， 试想，如果 有100个模块需要认证，那这100个模块都得更改调用方式，这么多模块肯定不止是一个人写的，让每个人再去修改调用方式 才能加上认证，你会被骂死的。。。。
    你觉得老王说的对，但问题是，如何即不改变原功能代码，又不改变原有调用方式，还能加上认证呢？ 你苦思了一会，还是想不出，老王在逗你的儿子玩，你说，老王呀，快给我点思路 ，实在想不出来，老王背对着你问，    
    老王：学过匿名函数没有? 
    你：学过学过，就是lambda嘛    
    老王：那lambda与正常函数的区别是什么？    
    你：最直接的区别是，正常函数定义时需要写名字，但lambda不需要    
    老王：没错，那lambda定好后，为了多次调用 ，可否也给它命个名？    
    你：可以呀，可以写成plus = lambda x:x+1类似这样，以后再调用plus就可以了，
    但这样不就失去了lambda的意义了，明明人家叫匿名函数呀，你起了名字有什么用呢？    
    老王：我不是要跟你讨论它的意义 ，我想通过这个让你明白一个事实  
    说着，老王拿起你儿子的画板，在上面写了以下代码：

    def plus(n):
        return n+1
     
    plus2 = lambda x:x+1
    老王： 上面这两种写法是不是代表 同样的意思？    
    你：是的
    老王：我给lambda x:x+1 起了个名字叫plus2，是不是相当于def plus2(x) ?    
    你：我擦，你别说，还真是，但老王呀，你想说明什么呢？    
    老王： 没啥，只想告诉你，
    给函数赋值变量名就像def func_name　是一样的效果，如下面的plus(n)函数，你调用时可以用plus名，还可以再起个其它名字，如
    calc = plus    
    calc(n)
    你明白我想传达什么意思了么？    
    你：。。。。。。。。。。。这。。。。。。嗯 。。。。。不太。。。。明白 。。
    老王：。。。。这。。。。。呵呵。。。。。。好吧。。。。，那我在给你点一下，你之前写的下面这段调用 认证的代码 
    
    home()
    login(america) #需要验证就调用 login，把需要验证的功能 当做一个参数传给login
    # home()
    # america()
    login(henan)
    你之所改变了调用方式，是因为用户每次调用时需要执行login(henan)，类似的。其实稍一改就可以了呀
    
    home()
    america = login(america)
    henan = login(henan)
    这样你，其它人调用henan时，其实相当于调用了login(henan), 通过login里的验证后，就会自动调用henan功能。     
    你：我擦，还真是唉。。。，老王，还是你nb。。。不过，等等， 我这样写了好，那用户调用时，应该是下面这个样子

    home()
    america = login(america) #你在这里相当于把america这个函数替换了
    henan = login(henan)
     
    #那用户调用时依然写
    america()
    但问题在于，还不等用户调用 ，你的america = login(america)就会先自己把america执行了呀。。。。，你应该等我用户调用 的时候 再执行才对呀，不信我试给你看。。。   
    老王：哈哈，你说的没错，这样搞会出现这个问题？ 但你想想有没有解决办法 呢？   
    你：我擦，你指的思路呀，大哥。。。我哪知道 下一步怎么走。。。
    老王：算了，估计你也想不出来。。。 学过嵌套函数没有？    
    你：yes,然后呢？    
    老王：想实现一开始你写的america = login(america)不触发你函数的执行，只需要在这个login里面再定义一层函数，第一次调用america = login(america)只调用到外层login，这个login虽然会执行，
    但不会触发认证了，因为认证的所有代码被封装在login里层的新定义 的函数里了，login只返回 里层函数的函数名，这样下次再执行america()时， 就会调用里层函数啦。。。    
    你：。。。。。。什么？ 什么个意思，我蒙逼了。。。 
    老王：还是给你看代码吧。。

    def login(func): #把要执行的模块从这里传进来
     
        def inner():#再定义一层函数
            _username = "alex" #假装这是DB里存的用户信息
            _password = "abc!23" #假装这是DB里存的用户信息
            global user_status
     
            if user_status == False:
                username = input("user:")
                password = input("pasword:")
     
                if username == _username and password == _password:
                    print("welcome login....")
                    user_status = True
                else:
                    print("wrong username or password!")
     
            if user_status == True:
                func() # 看这里看这里，只要验证通过了，就调用相应功能
     
        return inner #用户调用login时，只会返回inner的内存地址，下次再调用时加上()才会执行inner函数
    此时你仔细着了老王写的代码　，感觉老王真不是一般人呀，连这种奇淫巧技都能想出来。。。，心中默默感谢上天赐你一个大牛邻居。    
    你: 老王呀，你这个姿势很nb呀，你独创的？    
    此时你媳妇噗嗤的笑出声来，你也不知道 她笑个球。。。    
    老王：呵呵， 这不是我独创的呀当然 ，这是开发中一个常用的玩法，叫语法糖，官方名称“装饰器”，其实上面的写法，还可以更简单
    可以把下面代码去掉
    america = login(america) #你在这里相当于把america这个函数替换了
    只在你要装饰的函数上面加上下面代码

    @login
    def america():
        #login() #执行前加上验证
        print("----欧美专区----")
     
    def japan():
        print("----日韩专区----")
     
    @login
    def henan():
        #login() #执行前加上验证
        print("----河南专区----")
    效果是一样的。    
    你开心的玩着老王教你的新姿势 ，玩着玩着就手贱给你的“河南专区”版块 加了个参数，然后，结果 出错了。。。   
    你：老王，老王，怎么传个参数就不行了呢？    
    老王：那必然呀，你调用henan时，其实是相当于调用的login，你的henan第一次调用时henan = login(henan)， 
    login就返回了inner的内存地址，第2次用户自己调用henan("3p"),实际上相当于调用的时inner,但你的inner定义时并没有设置参数，但你给他传了个参数，所以自然就报错了呀    
    你：但是我的 版块需要传参数呀，你不让我传不行呀。。。    
    老王：没说不让你传，稍做改动便可。。
   
    老王：你再试试就好了 。 
    
    你： 果然好使，大神就是大神呀。 。。 不过，如果有多个参数呢？
    老王：。。。。老弟，你不要什么都让我教你吧，非固定参数你没学过么？ *args,**kwargs...    
    你：噢 。。。还能这么搞?,nb,我再试试。 
    你身陷这种新玩法中无法自拔，竟没注意到老王已经离开，
    你媳妇告诉你说为了不打扰你加班，今晚带孩子去跟她姐妹住 ，你觉得媳妇真体贴，最终，你终于搞定了所有需求，完全遵循开放-封闭原则，最终代码如下 。
    #_*_coding:utf-8_*_
    user_status = False #用户登录了就把这个改成True     
    def login(func): #把要执行的模块从这里传进来     
        def inner(*args,**kwargs):#再定义一层函数
            _username = "alex" #假装这是DB里存的用户信息
            _password = "abc!23" #假装这是DB里存的用户信息
            global user_status     
            if user_status == False:
                username = input("user:")
                password = input("pasword:")     
                if username == _username and password == _password:
                    print("welcome login....")
                    user_status = True
                else:
                    print("wrong username or password!")
     
            if user_status == True:
                func(*args,**kwargs) # 看这里看这里，只要验证通过了，就调用相应功能     
        return inner #用户调用login时，只会返回inner的内存地址，下次再调用时加上()才会执行inner函数
          
    def home():
        print("---首页----")
     
    @login
    def america():
        #login() #执行前加上验证
        print("----欧美专区----")
     
    def japan():
        print("----日韩专区----")
     
    # @login
    def henan(style):
        '''
        :param style: 喜欢看什么类型的，就传进来
        :return:
        '''
        #login() #执行前加上验证
        print("----河南专区----")     
    home()
    # america = login(america) #你在这里相当于把america这个函数替换了
    henan = login(henan)     
    # #那用户调用时依然写
    america()     
    henan("3p")
    此时，你已累的不行了，洗洗就抓紧睡了，半夜，上厕所，隐隐听到隔壁老王家有微弱的女人的声音传来，你会心一笑，老王这家伙，
    不声不响找了女朋友也不带给我看看，改天一定要见下真人。。。。    
    第二2天早上，产品经理又提了新的需求，要允许用户选择用qq\weibo\weixin认证，此时的你，已深谙装饰器各种装逼技巧，轻松的就实现了新的需求。

    #_*_coding:utf-8_*_
    user_status = False #用户登录了就把这个改成True    
    def login(auth_type): #把要执行的模块从这里传进来
        def auth(func):
            def inner(*args,**kwargs):#再定义一层函数
                if auth_type == "qq":
                    _username = "alex" #假装这是DB里存的用户信息
                    _password = "abc!23" #假装这是DB里存的用户信息
                    global user_status
    
                    if user_status == False:
                        username = input("user:")
                        password = input("pasword:")
    
                        if username == _username and password == _password:
                            print("welcome login....")
                            user_status = True
                        else:
                            print("wrong username or password!")
                    if user_status == True:
                        return func(*args,**kwargs) # 看这里看这里，只要验证通过了，就调用相应功能
                else:
                    print("only support qq ")
            return inner #用户调用login时，只会返回inner的内存地址，下次再调用时加上()才会执行inner函数    
        return auth
    
    def home():
        print("---首页----")
    
    @login('qq')
    def america():
        #login() #执行前加上验证
        print("----欧美专区----")
    
    def japan():
        print("----日韩专区----")
    
    @login('weibo')
    def henan(style):
        '''
        :param style: 喜欢看什么类型的，就传进来
        :return:
        '''
        #login() #执行前加上验证
        print("----河南专区----")    
    home()
    # america = login(america) #你在这里相当于把america这个函数替换了
    #henan = login(henan)    
    # #那用户调用时依然写
    america()
    # henan("3p")
    未完待续。。。。