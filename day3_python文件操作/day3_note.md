#今日内容
+   字典
+   集合
+   文件处理
+   字符编码
+   推荐书籍：消费者行为学、林达看美国

###1、字典
    key定义规则：不可变(数字、字符串、元组)、唯一性
    value定义规则：可任意规则
####1.1、字典的创建
    第一种:
        person = {'name':'zhangsan','age':18}
    第二种:
        persopn = dict({'name'}:'zhangsan2','age':19)
####1.2、字典的方法
    1、clear(self)#清除字典所有内容
        演示:
            >>> user_dict = {'name':'zhangsan','age':18}
            >>> user_dict.clear()
            >>> user_dict
            {}
    2、copy(self)#拷贝、返回字典的浅复制
    3、fromkeys(s,v=None)
        '''
            s:key值
            v:可选参数,
        '''
        #创建一个新字典，以序列seq中元素做字典的键，value为字典所有键对应的初始值
        演示:
            >>> user_dict = dict.fromkeys(['a','b','c'],1)
            >>> user_dict
            {'c': 1, 'a': 1, 'b': 1}
    4、get(self, k, d=None)
        '''
            key –> 字典中要查找的键
            default –> 如果指定键的值不存在时，返回该默认值值
        '''
        #返回指定键的值，如果值不在字典中返回默认值
        演示:
            >>> user_dict = {'name':'zhangsan','age':18}
            >>> user_dict.get('name')
            'zhangsan'
            >>> print(user_dict.get('name1'))
            None
            >>> print(user_dict.get('name1','没有'))
            没有
    5、has_key(self, k)(适用于2.7)
        #用于判断键是否存在于字典中，如果键在字典dict里返回true，否则返回false
    6、items(self)
        #以列表返回可遍历的(键, 值)元组数组
        演示:
            >>> user_dict.items()
            dict_items([('name', 'zhangsan'), ('age', 18)])
    7、keys(self):
        #以列表的形式返回一个字典所有的键
        演示:
            >>> user_dict.keys()
            dict_keys(['name', 'age'])
    8、pop(self, k, d=None):
        #删除指定给定键所对应的值，返回这个值并从字典中把它移除
        演示:
            >>> user_dict.pop('age')
            18
            >>> user_dict
            {'name': 'zhangsan'}
    9、popitem(self):
        #随机返回并删除字典中的一对键和值，因为字典是无序的，没有所谓的”最后一项”或是其它顺序。
        演示:
            >>> user_dict = {'name':'zhangsan','age':18}
            >>> user_dict.popitem()
            ('name', 'zhangsan')
            >>> user_dict
            {'age': 18}
    10、update
        #把字典dic2的键/值对更新到dic1里
        演示:
            >>> dict1 = {'name':'zhangsan'}
            >>> dict2 = {'age':18}
            >>> dict1.update(dict2)
            >>> dict1
            {'name': 'zhangsan', 'age': 18}
    11、values
        #显示字典中所有的值
        演示:
            >>> dict1
            {'name': 'zhangsan', 'age': 18}
            >>> dict1.values()
            dict_values(['zhangsan', 18])
            
###2、集合
    集合（set）是一个无序不重复元素的序列。
    基本功能是进行成员关系测试和删除重复元素
####2.1、集合创建
    可以使用大括号({})或者 set()函数创建集合，注意：创建一个空集合必须用 set() 而不是 { }，因为 { } 是用来创建一个空字典。
    演示:
        >>> user_set = set()
        >>> type(user_set)
        <class 'set'>
        >>> user_set = ({})#创建空集合最好用set()
        >>> type(user_set)
        <class 'dict'>
        >>> 
####2.2、集合常用方法
    #!/usr/bin/python3
    student = {'Tom', 'Jim', 'Mary', 'Tom', 'Jack', 'Rose'}
    print(student)   # 输出集合，重复的元素被自动去掉
    # 成员测试
    if('Rose' in student) :
        print('Rose 在集合中')
    else :
        print('Rose 不在集合中')
    
    # set可以进行集合运算
    a = set('abracadabra')
    b = set('alacazam')
    print(a)
    print(b)
    print(a - b)     # a和b的差集
    print(a | b)     # a和b的并集
    print(a & b)     # a和b的交集
    print(a ^ b)     # a和b中不同时存在的元素
    结果如下:
    {'Jack', 'Rose', 'Mary', 'Jim', 'Tom'}
    Rose 在集合中
    {'r', 'b', 'a', 'c', 'd'}
    {'m', 'z', 'c', 'a', 'l'}
    {'r', 'b', 'd'}
    {'a', 'l', 'z', 'b', 'm', 'd', 'r', 'c'}
    {'a', 'c'}
    {'l', 'z', 'b', 'm', 'd', 'r'}
###3、文件处理
    Python可以对文件内容进行添加、修改、删除，
    使用到的函数在Python3.5.x为open，在Python2.7.x同时支持file和open。
####3.1、Python文件打开方式
    文件句柄 = open('文件路径','打开模式')
    文件句柄就像变量名一样,打开模式默认为只读
####3.2、Python打开文件的模式
    模式	    描述
    r	    以只读方式打开文件。文件的指针将会放在文件的开头。这是默认模式。
    rb	    以二进制格式打开一个文件用于只读。文件指针将会放在文件的开头。这是默认模式。
    r+	    打开一个文件用于读写。文件指针将会放在文件的开头。
    rb+	    以二进制格式打开一个文件用于读写。文件指针将会放在文件的开头。
    w	    打开一个文件只用于写入。如果该文件已存在则将其覆盖。如果该文件不存在，创建新文件。
    wb	    以二进制格式打开一个文件只用于写入。如果该文件已存在则将其覆盖。如果该文件不存在，创建新文件。
    w+	    打开一个文件用于读写。如果该文件已存在则将其覆盖。如果该文件不存在，创建新文件。
    wb+	    以二进制格式打开一个文件用于读写。如果该文件已存在则将其覆盖。如果该文件不存在，创建新文件。
    a	    打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。也就是说，新的内容将会被写入到已有内容之后。如果该文件不存在，创建新文件进行写入。
    ab	    以二进制格式打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。也就是说，新的内容将会被写入到已有内容之后。如果该文件不存在，创建新文件进行写入。
    a+	    打开一个文件用于读写。如果该文件已存在，文件指针将会放在文件的结尾。文件打开时会是追加模式。如果该文件不存在，创建新文件用于读写。
    ab+	    以二进制格式打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。如果该文件不存在，创建新文件用于读写。
####3.3、Python文件读取方式
    
    模式	                说明
    read([size])	    读取文件全部内容，如果设置了size，那么久读取size字节
    readline([size])	    一行一行的读取
    readlines()	    读取到的每一行内容作为列表中的一个元素
    ps:python3中可以直接循环文件句柄
    演示:
        测试文件为:file.txt
        jijianmingdeMacBook-Pro:~ jijianming$ cat file.txt 
        1
        2
        3
        4
        5
        6
    python3:直接循环句柄
        >>> f = open('file.txt')
        >>> for i in f:
        ...     print(i)
        ... 
        1
        
        2
        
        3
        
        4
        
        5
        
        6
    read:
        # 以只读的方式打开文件file.txt
        f = open("file.txt","r")
        # 读取文件内容赋值给变量c
        c = f.read()
        # 关闭文件
        f.close()
        # 输出c的值
        print(c)
        1
        2
        3
        4
        5
        6
    readlines:
        >>> f = open('file.txt')
        >>> c = f.readlines()
        >>> print(c)
        ['1\n', '2\n', '3\n', '4\n', '5\n', '6\n']
    readline:
        >>> f = open('file.txt')
        >>> c = f.readline()
        >>> print(c)
        1
        >>> c = f.readline()
        >>> print(c)
        2
        >>> c = f.readline()
        >>> print(c)
        3
####3.4、Python文件写入方式   
    方法	                            说明
    write(str)	                将字符串写入文件
    writelines(sequence or strings)	写多行到文件，参数可以是一个可迭代的对象，列表、元组等
    演示:
        >>> f = open('file.txt','w')
        >>> f.write('abc')
        3
        >>> f.close()
        >>> f = open('file.txt','r')
        >>> f.readlines()
        ['abc']
        >>> f = open('file.txt','w')
        >>> f.write(['1','2','3','4','5'])
        Traceback (most recent call last):
          File "<stdin>", line 1, in <module>
        TypeError: write() argument must be str, not list
        >>> f.close()
        >>> f = open('file.txt','w')
        >>> f.writelines(['1','2','3','4','5'])
        >>> f.close()
####3.5、Python文件操作默认的方法
    方法                  说明
    f.close           关闭已经打开的文件
    f.flush           刷新缓冲区的内容到硬盘中
    f.isatty          判断文件是否是tty设备，如果是tty设备则返回True，否则返回False
    f.readable        文件是否可读，如果可读返回True，否则返回False
    f.tell            获取指针位置
    f.seek            指定文件中指针位置
    f.writable        文件是否可写
####3.6、同时打开多个文件
    with open('file1') as f_read , open('file2','w') as f_write:
    #是适用2.7以上版本
###4、字符编码
    首先要搞清楚，字符串在Python内部的表示是unicode编码，
    因此，在做编码转换时，通常需要以unicode作为中间编码，
    即先将其他编码的字符串解码（decode）成unicode，再从unicode编码（encode）成另一种编码。
    区别:
    先说python2
    py2里默认编码是ascii
    文件开头那个编码声明是告诉解释这个代码的程序 以什么编码格式 把这段代码读入到内存，因为到了内存里，这段代码其实是以bytes二进制格式存的，不过即使是2进制流，也可以按不同的编码格式转成2进制流，你懂么？
    如果在文件头声明了#_*_coding:utf-8*_，就可以写中文了， 不声明的话，python在处理这段代码时按ascii，显然会出错， 加了这个声明后，里面的代码就全是utf-8格式了
    在有#_*_coding:utf-8*_的情况下，你在声明变量如果写成name=u"大保健"，那这个字符就是unicode格式，不加这个u,那你声明的字符串就是utf-8格式
    utf-8 to gbk怎么转，utf8先decode成unicode,再encode成gbk
    再说python3

    py3里默认文件编码就是utf-8,所以可以直接写中文，也不需要文件头声明编码了，干的漂亮
    你声明的变量默认是unicode编码，不是utf-8, 因为默认即是unicode了（不像在py2里，你想直接声明成unicode还得在变量前加个u）, 此时你想转成gbk的话，直接your_str.encode("gbk")即可以
    但py3里，你在your_str.encode("gbk")时，感觉好像还加了一个动作，就是就是encode的数据变成了bytes里，我操，这是怎么个情况，因为在py3里，str and bytes做了明确的区分，你可以理解为bytes就是2进制流，你会说，我看到的不是010101这样的2进制呀， 那是因为python为了让你能对数据进行操作而在内存级别又帮你做了一层封装，否则让你直接看到一堆2进制，你能看出哪个字符对应哪段2进制么？什么？自己换算，得了吧，你连超过2位数的数字加减运算都费劲，还还是省省心吧。　　
    那你说，在py2里好像也有bytes呀，是的，不过py2里的bytes只是对str做了个别名，没有像py3一样给你显示的多出来一层封装，但其实其内部还是封装了的。 这么讲吧， 无论是2还是三， 从硬盘到内存，数据格式都是 010101二进制到-->b'\xe4\xbd\xa0\xe5\xa5\xbd' bytes类型－－>按照指定编码转成你能看懂的文字
    编码应用比较多的场景应该是爬虫了，互联网上很多网站用的编码格式很杂，虽然整体趋向都变成utf-8，但现在还是很杂，所以爬网页时就需要你进行各种编码的转换，不过生活正在变美好，期待一个不需要转码的世界。

    最后，编码is a piece of fucking shit, noboby likes it.
    decode的作用是将其他编码的字符串转换成unicode编码，如str1.decode('gb2312')，表示将gb2312编码的字符串转换成unicode编码。
    encode的作用是将unicode编码转换成其他编码的字符串，如str2.encode('gb2312')，表示将unicode编码的字符串转换成gb2312编码。
    GBK转换UTF-8格式流程
        1、首先通过解码(decode)转换为unicode;
        2、再通过编码( encode)转换成utf-8.
    UTF-8转换成GBK格式流程和上面一样.
    详细请浏览:
        http://www.cnblogs.com/yuanchenqi/articles/5956943.html
