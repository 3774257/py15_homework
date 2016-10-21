#今日内容
+   循环之for
+   循环之while
+   数据类型(字符串、数字、列表、字典、元组)

##循环之for
####1、功能
    python中循环大多数都是用for来完成,for循环是一种迭代循环机制不同于
    while循环,迭代即重复相同操作,每次操作都是给予上一次结果而进行的。
####2、语法
    name_list = ['zhangsan','lisi','wanger']
    for i in name_list:
        print(i)
    ps:基于enumerate方法来实现,每项前面有个编号
    for i in enumerate(name_list):
        print(i)
    结果如下:
        0 zhangsan
        1 lisi
        2 wanger
    continue、break、exit的区别:
    continue:跳过本次循环、继续下次循环;
    break:跳出整个循环,但会继续往下执行脚本;
    exit:退出整个脚本
##循环之while
####1、功能:
    也是用于循环不过是只要条件成立会一直循环下去。
####2、语法:
    while True:
        print('你真帅')
    while中也有break  continue exit和for中用法一样。
##数据类型
#####数字的分类:
    数字又分为:整型、长整型、布尔、浮点、复数
#####整型
    Python的整型相当于C中的long型,Python中的整数可以用十进制,八进制,十六进制表示。
        >>> 1
        1    #十进制
        >>> oct(1)
        '0o1'    #要用八进制表示整数时，数值前面要加上一个前缀“0”
        >>> hex(1)
        '0x1'    #要用十六进制表示整数时，数字前面要加上前缀0X或0x
#####长整型
    跟C语言不同，Python的长整型没有指定位宽，也就是说Python没有限制长整型数值的大小，
    但是实际上由于机器内存有限，所以我们使用的长整型数值不可能无限大。
    在使用过程中，我们如何区分长整型和整型数值呢？
    通常的做法是在数字尾部加上一个大写字母L或小写字母l以表示该整数是长整型的，例如：
    Python 2.7.10 (default, Oct 23 2015, 19:19:21) 
    [GCC 4.2.1 Compatible Apple LLVM 7.0.0 (clang-700.0.59.5)] on darwin
    Type "help", "copyright", "credits" or "license" for more information.
    >>> num = 222222222222222222222222222222222222222222222222222
    >>> num
    222222222222222222222222222222222222222222222222222L
    注意，自从Python2起，如果发生溢出，Python会自动将整型数据转换为长整型，
    所以如今在长整型数据后面不加字母L也不会导致严重后果了。
    
    python3.*
    长整型，整型统一归为整型
#####布尔bool
    True 和False
    1和0
#####浮点型
    浮点型就是带有小数点的、例如:1.2
#####复数
    复数由实数部分和虚数部分组成，一般形式为x＋yj，其中的x是复数的实数部分，y是复数的虚数部分，这里的x和y都是实数。
    注意，虚数部分的字母j大小写都可以，
    >>> 1.3 + 2.5j == 1.3 + 2.5J
    True
#####与数值相关的内置函数:
    int(x [,base])  #将x转换为一个整数
    long(x [,base]) #将x转换为一个长整数
    float(x)    #将x转换到一个浮点数
    complex(real [,imag])   #创建一个复数
    str(x)  #将对象 x 转换为字符串
    eval(str)   #用来计算在字符串中的有效Python表达式,并返回一个对象
    tuple(s)    #将序列 s 转换为一个元组
    list(s)     #将序列 s 转换为一个列表
    set(s)      #转换为可变集合
    dict(d)     #创建一个字典。d 必须是一个序列 (key,value)元组。
    chr(x)      #将一个整数转换为一个字符
    unichr(x)   #将一个整数转换为Unicode字符
    ord(x)      #将一个字符转换为它的整数值
    hex(x)      #将一个整数转换为一个十六进制字符串
    oct(x)      #将一个整数转换为一个八进制字符串
####字符串
    字符串类型是python的序列类型，是一种有序的的字符的集合
    用''或者""再或者""""""或者''''''扩起来的都是字符串
#####字符串的创建
    >>> name = 'zhangsan'
    >>> print(type(name))#type:用于查看类型
    <type 'str'>
    >>> name = "zhangsan"
    >>> print(type(name))
    <type 'str'>
    >>> name = """zhangsan"""
    >>> print(type(name))
    <type 'str'>
    >>> name = '''zhangsan'''
    >>> print(type(name))
    <type 'str'>
#####字符串的常用方法
       1、capitalize(self) #首字母变大写
              >>> name="hello,world."
              >>> name.capitalize()
              'Hellor,world.'
       2、center(self, width, fillchar=None) 
              #内容居中，width：字符串的总宽度；fillchar：填充字符，默认填充字符为空格
              >>> string="hello word"
              >>> string.center(20,"*")
              *****hello word*****'
       3、count(self, sub, start=None, end=None)
              #用于统计字符串里某个字符出现的次数,可选参数为在字符串搜索的开始与结束位置。
              '''
                sub –> 搜索的子字符串
                start –> 字符串开始搜索的位置。默认为第一个字符,第一个字符索引值为0。
                end –> 字符串中结束搜索的位置。字符中第一个字符的索引为 0。默认为字符串的最后一个位置。
              '''
              >>> string = 'hello,world.'
              >>> string.count('l')
              3
              >>> string.count('l',2,6)#指定范围2-6、顾头不顾尾
              2
       4、endswith(self, suffix, start=None, end=None)
              #判断字符串是否以指定后缀结尾，如果以指定后缀结尾返回True，否则返回False
              '''
                suffix –> 后缀，可能是一个字符串，或者也可能是寻找后缀的tuple。
                start –> 开始，切片从这里开始。
                end –> 结束，片到此为止。
              '''
              >>> string.endswith('.')
              True
              >>> string.endswith('d')
              False
              >>> string.endswith('d',-1)
              False
              >>> string.endswith('d',0,-1)#指定范围
              True
       5、expandtabs(self, tabsize=None)
              #把字符串中的tab符号(‘\t’)转为空格，tab符号(‘\t’)默认的空格数是8。
              #tabsize –> 指定转换字符串中的 tab 符号(‘\t’)转为空格的字符数。
              >>> string = 'hello\tworld.'
              >>> string
              'hello\tworld.'
              >>> string.expandtabs(20)
              'hello               world.'
       6、find(self, sub, start=None, end=None)
              #查找关键字，如果指定beg(开始)和end(结束)范围，则检查是否包含在指定范围内，如果包含子字符串返回开始的索引值，否则返回-1
              '''
                str –> 指定检索的字符串
                beg –> 开始索引，默认为0。
                end –> 结束索引，默认为字符串的长度。
              '''
              >>> string.find('l')
              2
              >>> string.find('l',3)
              3
              >>> string.find('l',4)
              9
              >>> string
              'hello\tworld.'
       7、index(self, sub, start=None, end=None)
              #检查字符的索引值、和find类似。找不到就报错、而find找不到则返回-1
              '''
                str –> 指定检索的字符串
                beg –> 开始索引，默认为0。
                end –> 结束索引，默认为字符串的长度。
              '''
              >>> string.index('l')
              2
              >>> string.index('l',3)
              3
              >>> string.index('l',4)
              9
              >>> string.index('a')
              Traceback (most recent call last):
              File "<stdin>", line 1, in <module>
              ValueError: substring not found
       8、isalnum(self)
              #检测字符串是否由字母和数字组成或者纯字母纯数字，返回True,否则返回False
              >>> string = 'fdsfds'
              >>> string.isalnum()
              True
              >>> string = 'fdsfdsfsfds123456'
              >>> string.isalnum()
              True
              >>> string = 'fdsfdsfsfds 123456'
              >>> string.isalnum()
              False
       9、isalpha(self)
              #检测字符串是否只有字母组成
              >>> string = 'fdsfds'
              >>> string.isalpha()
              True
              >>> string = 'fdsfds '
              >>> string.isalpha()
              False
              >>> string = 'fdsfds123432'
              >>> string.isalpha()
              False
              >>> string = '12432'
              >>> string.isalpha()
              False
       10、isdigit(self)
              #只能是数字
       11、islower(self)
              #检测字符串元素是否为小写字母
              >>> str = 'abc'
              >>> str.islower()
              True
              >>> str = '123'
              >>> str.islower()
              False
              >>> str = 'abc123'
              >>> str.islower()
              True
              >>> str = 'abc123B'
              >>> str.islower()
              False
       12、isspace(self)
              #是否有空格组成
              >>> str = 'fdsa fdsa'
              >>> str.isspace()
              False
              >>> str = ' '
              >>> str.isspace()
              True
              >>> str = '\t'
              >>> str.isspace()
              True
              >>> str = '\n'
              >>> str.isspace()
              True
       13、istitle(self)
              #检测字符串中所有的单词拼写首字母是否为大写，且其他字母为小写
              >>> str = 'abc Wd'
              >>> str.istitle()
              False
              >>> str = 'Abc Wd'
              >>> str.istitle()
              True
              >>> str = 'Abc d'
              >>> str.istitle()
              False
       14、isupper(self)
              #检测字符串中所有的字母是否都为大写。
              >>> str = 'ABC'
              >>> str.isupper()
              True
              >>> str = 'ABCd'
              >>> str.isupper()
              False
              >>> str = 'ABC123'
              >>> str.isupper()
              True
       15、ljust(self, width, fillchar=None)
              #返回一个原字符串左对齐,并使用空格填充至指定长度的新字符串。如果指定的长度小于原字符串的长度则返回原字符串。
              '''
                width –> 指定字符串长度
                fillchar –> 填充字符，默认为空格
              '''
              >>> str = 'hello'
              >>> str.ljust(10)
              'hello     '
              >>> str.ljust(10,'*')
              'hello*****'
       16、lower(self)
              #转换字符串中所有大写字符为小写
              >>> str = 'HeLlo'
              >>> str.lower()
              'hello'
       17、upper(self)
              #转换字符串中所有小写为大写
              >>> str =  'abc'
              >>> str.upper()
              'ABC'
       18、lstrip(self, chars=None)
              #截掉字符串左边的空格或指定字符
              #chars –> 指定截取的字符
              >>> str = '  fds   '
              >>> str.lstrip()
              'fds   '
              >>> str = '.fds.'
              >>> str.lstrip('.')
              'fds.'
       19、rstrip(self,chars=None)
              #截取右边的空格和指定字符
              用法和lstrip一样
       20、strip(self,chars=None)
              #截取左边和右边的空格和指定字符
              
       21、split(self,str)
              #用来根据指定的分隔符将字符串进行分割，
              #如果字符串包含指定的分隔符，则返回结果的列表，
              >>> str = 'a.b.c'
              >>> str.split('.')
              ['a', 'b', 'c']
       22、replace(self, old, new, count=None)
              #把字符串中的 old(旧字符串)替换成new(新字符串)，
              #如果指定第三个参数max，则替换不超过max次
              >>> str = 'aabbcc'
              >>> str.replace('a','d')
              'ddbbcc'
              >>> str = 'aabbcc'
              >>> str.replace('a','d',1)
              'dabbcc'
#####python中str函数isdigit、isdecimal、isnumeric的区别
              num = "1"  #unicode
              num.isdigit()   # True
              num.isdecimal() # True
              num.isnumeric() # True

              num = "1" # 全角
              num.isdigit()   # True
              num.isdecimal() # True
              num.isnumeric() # True

              num = b"1" # byte
              num.isdigit()   # True
              num.isdecimal() # AttributeError 'bytes' object has no attribute 'isdecimal'
              num.isnumeric() # AttributeError 'bytes' object has no attribute 'isnumeric'

              num = "IV" # 罗马数字
              num.isdigit()   # True
              num.isdecimal() # False
              num.isnumeric() # True

              num = "四" # 汉字
              num.isdigit()   # False
              num.isdecimal() # False
              num.isnumeric() # True

              ===================
              isdigit()
              True: Unicode数字，byte数字（单字节），全角数字（双字节），罗马数字
              False: 汉字数字
              Error: 无

              isdecimal()
              True: Unicode数字，，全角数字（双字节）
              False: 罗马数字，汉字数字
              Error: byte数字（单字节）

              isnumeric()
              True: Unicode数字，全角数字（双字节），罗马数字，汉字数字
              False: 无
              Error: byte数字（单字节）
####运算符
#####什么是运算符?
    举个简单的例子 4 +5 = 9 。 例子中，4 和 5 被称为操作数，"+" 称为运算符。
#####算术运算符
    运算符      	描述	                    实例
    +	加   两个对象相加	                a + b 输出结果 30
    -	减  得到负数或是一个数减去另一个数	    a - b 输出结果 -10
    *	乘  两个数相乘或是返回一个被重复若干次的字符串	a * b 输出结果 200
    /	除  x除以y	                    b / a 输出结果 2
    %	取模  返回除法的余数	            b % a 输出结果 0
    **	幂  返回x的y次幂	                a**b 为10的20次方， 输出结果 100000000000000000000
    //	取整除  返回商的整数部分	            9//2 输出结果 4 , 9.0//2.0 输出结果 4.0
    以下实例演示了Python所有算术运算符的操作:
        a = 21
        b = 10
        c = 0
                
        c = a + b
        print "1 - c 的值为：", c
                
        c = a - b
        print "2 - c 的值为：", c 
                
        c = a * b
        print "3 - c 的值为：", c 
                
        c = a / b
        print "4 - c 的值为：", c 
                
        c = a % b
        print "5 - c 的值为：", c
                
        # 修改变量 a 、b 、c
        a = 2
        b = 3
        c = a**b 
        print "6 - c 的值为：", c
                
        a = 10
        b = 5
        c = a//b 
        print "7 - c 的值为：", c  
    结果为
        1 - c 的值为： 31
        2 - c 的值为： 11
        3 - c 的值为： 210
        4 - c 的值为： 2
        5 - c 的值为： 1
        6 - c 的值为： 8
        7 - c 的值为： 2
#####Python比较运算符
    ps:以下假设变量a为10，变量b为20
        运算符	                描述	                            实例
        ==	            等于 - 比较对象是否相等       	        (a == b) 返回 False。
        !=	            不等于 - 比较两个对象是否不相等	        (a != b) 返回 true.
        <>	            不等于 - 比较两个对象是否不相等	        (a <> b) 返回 true。这个运算符类似 != 。
        >	            大于 - 返回x是否大于y	                (a > b) 返回 False。
        <	            小于 - 返回x是否小于y。所有比较运算符返回1表示真，返回0表示假。这分别与特殊的变量True和False等价。注意，这些变量名的大写。	(a < b) 返回 true。
        >=	            大于等于	- 返回x是否大于等于y。	        (a >= b) 返回 False。
        <=	            小于等于 -	返回x是否小于等于y。	     (a <= b) 返回 true。
    以下实例演示了Python所有比较运算符的操作：
        a = 21
        b = 10
        c = 0
        
        if ( a == b ):
           print "1 - a 等于 b"
        else:
           print "1 - a 不等于 b"
        
        if ( a != b ):
           print "2 - a 不等于 b"
        else:
           print "2 - a 等于 b"
        
        if ( a <> b ):
           print "3 - a 不等于 b"
        else:
           print "3 - a 等于 b"
        
        if ( a < b ):
           print "4 - a 小于 b" 
        else:
           print "4 - a 大于等于 b"
        
        if ( a > b ):
           print "5 - a 大于 b"
        else:
           print "5 - a 小于等于 b"
        
        # 修改变量 a 和 b 的值
        a = 5;
        b = 20;
        if ( a <= b ):
           print "6 - a 小于等于 b"
        else:
           print "6 - a 大于  b"
        
        if ( b >= a ):
           print "7 - b 大于等于 b"
        else:
           print "7 - b 小于 b"
    以上实例输出结果：
        1 - a 不等于 b
        2 - a 不等于 b
        3 - a 不等于 b
        4 - a 大于等于 b
        5 - a 大于 b
        6 - a 小于等于 b
        7 - b 大于等于 b
#####Python赋值运算符
    按位运算符是把数字看作二进制来进行计算的。Python中的按位运算法则如下：
    下表中变量 a 为 60，b 为 13。
    运算符	                描述	                                            实例
    &	按位与运算符：参与运算的两个值,如果两个相应位都为1,则该位的结果为1,否则为0	(a & b) 输出结果 12 ，二进制解释： 0000 1100
    |	按位或运算符：只要对应的二个二进位有一个为1时，结果位就为1。	(a | b) 输出结果 61 ，二进制解释： 0011 1101
    ^	按位异或运算符：当两对应的二进位相异时，结果为1	(a ^ b) 输出结果 49 ，二进制解释： 0011 0001
    ~	按位取反运算符：对数据的每个二进制位取反,即把1变为0,把0变为1	(~a ) 输出结果 -61 ，二进制解释： 1100 0011， 在一个有符号二进制数的补码形式。
    <<	左移动运算符：运算数的各二进位全部左移若干位，由"<<"右边的数指定移动的位数，高位丢弃，低位补0。	a << 2 输出结果 240 ，二进制解释： 1111 0000
    >>	右移动运算符：把">>"左边的运算数的各二进位全部右移若干位，">>"右边的数指定移动的位数	a >> 2 输出结果 15 ，二进制解释： 0000 1111
    以下实例演示了Python所有位运算符的操作：
        a = 60            # 60 = 0011 1100 
        b = 13            # 13 = 0000 1101 
        c = 0
        
        c = a & b;        # 12 = 0000 1100
        print "1 - c 的值为：", c
        
        c = a | b;        # 61 = 0011 1101 
        print "2 - c 的值为：", c
        
        c = a ^ b;        # 49 = 0011 0001
        print "3 - c 的值为：", c
        
        c = ~a;           # -61 = 1100 0011
        print "4 - c 的值为：", c
        
        c = a << 2;       # 240 = 1111 0000
        print "5 - c 的值为：", c
        
        c = a >> 2;       # 15 = 0000 1111
        print "6 - c 的值为：", c
    以上实例输出结果：
        1 - c 的值为： 12
        2 - c 的值为： 61
        3 - c 的值为： 49
        4 - c 的值为： -61
        5 - c 的值为： 240
        6 - c 的值为： 15
#####Python逻辑运算符
    Python语言支持逻辑运算符，以下假设变量 a 为 10, b为 20:
    运算符	逻辑表达式	    描述	                                                        实例
    and	    x and y	    布尔"与" - 如果 x 为 False，x and y 返回 False，否则它返回 y 的计算值。	(a and b) 返回 20。
    or	    x or y	    布尔"或"	- 如果 x 是非 0，它返回 x 的值，否则它返回 y 的计算值。	        (a or b) 返回 10。
    not	    not x	    布尔"非" - 如果 x 为 True，返回 False 。如果 x 为 False，它返回 True。	not(a and b) 返回 False
    以上实例输出结果：        
        a = 10
        b = 20            
        if ( a and b ):
           print "1 - 变量 a 和 b 都为 true"
        else:
           print "1 - 变量 a 和 b 有一个不为 true"
        
        if ( a or b ):
           print "2 - 变量 a 和 b 都为 true，或其中一个变量为 true"
        else:
           print "2 - 变量 a 和 b 都不为 true"
        
        # 修改变量 a 的值
        a = 0
        if ( a and b ):
           print "3 - 变量 a 和 b 都为 true"
        else:
           print "3 - 变量 a 和 b 有一个不为 true"
        
        if ( a or b ):
           print "4 - 变量 a 和 b 都为 true，或其中一个变量为 true"
        else:
           print "4 - 变量 a 和 b 都不为 true"
        
        if not( a and b ):
           print "5 - 变量 a 和 b 都为 false，或其中一个变量为 false"
        else:
           print "5 - 变量 a 和 b 都为 true"
    以上实例输出结果：
        1 - 变量 a 和 b 都为 true
        2 - 变量 a 和 b 都为 true，或其中一个变量为 true
        3 - 变量 a 和 b 有一个不为 true
        4 - 变量 a 和 b 都为 true，或其中一个变量为 true
        5 - 变量 a 和 b 都为 false，或其中一个变量为 false
#####Python成员运算符
    除了以上的一些运算符之外，Python还支持成员运算符，测试实例中包含了一系列的成员，包括字符串，列表或元组。
    运算符	        描述	                                                         实例
    in	        如果在指定的序列中找到值返回 True，否则返回 False。	x 在 y 序列中 , 如果 x 在 y 序列中返回 True。
    not in	    如果在指定的序列中没有找到值返回 True，否则返回 False。	x 不在 y 序列中 , 如果 x 不在 y 序列中返回 True。
    以下实例演示了Python所有成员运算符的操作：
        a = 10
        b = 20
        list = [1, 2, 3, 4, 5 ];
        
        if ( a in list ):
           print "1 - 变量 a 在给定的列表中 list 中"
        else:
           print "1 - 变量 a 不在给定的列表中 list 中"
        
        if ( b not in list ):
           print "2 - 变量 b 不在给定的列表中 list 中"
        else:
           print "2 - 变量 b 在给定的列表中 list 中"
        
        # 修改变量 a 的值
        a = 2
        if ( a in list ):
           print "3 - 变量 a 在给定的列表中 list 中"
        else:
           print "3 - 变量 a 不在给定的列表中 list 中"
    以上实例输出结果：
        1 - 变量 a 不在给定的列表中 list 中
        2 - 变量 b 不在给定的列表中 list 中
        3 - 变量 a 在给定的列表中 list 中
#####Python身份运算符
    身份运算符用于比较两个对象的存储单元
    运算符	            描述	                                实例
    is	    is是判断两个标识符是不是引用自一个对象	x is y, 如果 id(x) 等于 id(y) , is 返回结果 1
    is not	is not是判断两个标识符是不是引用自不同对象	x is not y, 如果 id(x) 不等于 id(y). is not 返回结果 1
    以下实例演示了Python所有身份运算符的操作：
        a = 20
        b = 20
        
        if ( a is b ):
           print "1 - a 和 b 有相同的标识"
        else:
           print "1 - a 和 b 没有相同的标识"
        
        if ( id(a) == id(b) ):
           print "2 - a 和 b 有相同的标识"
        else:
           print "2 - a 和 b 没有相同的标识"
        
        # 修改变量 b 的值
        b = 30
        if ( a is b ):
           print "3 - a 和 b 有相同的标识"
        else:
           print "3 - a 和 b 没有相同的标识"
        
        if ( a is not b ):
           print "4 - a 和 b 没有相同的标识"
        else:
           print "4 - a 和 b 有相同的标识"
    以上实例输出结果：
        1 - a 和 b 有相同的标识
        2 - a 和 b 有相同的标识
        3 - a 和 b 没有相同的标识
        4 - a 和 b 没有相同的标识
#####Python运算符优先级
    以下表格列出了从最高到最低优先级的所有运算符：
    运算符	    描述
    **	    指数 (最高优先级)
    ~ + -	按位翻转, 一元加号和减号 (最后两个的方法名为 +@ 和 -@)
    * / % //	乘，除，取模和取整除
    + -	    加法减法
    >> <<	右移，左移运算符
    &	    位 'AND'
    ^ |	    位运算符
    <= < > >=	比较运算符
    <> == !=	等于运算符
    = %= /= //= -= += *= **=	赋值运算符
    is is not	身份运算符
    in not in	成员运算符
    not or and	逻辑运算符
    以下实例演示了Python所有运算符优先级的操作：

        a = 20
        b = 10
        c = 15
        d = 5
        e = 0
        
        e = (a + b) * c / d       #( 30 * 15 ) / 5
        print "(a + b) * c / d 运算结果为：",  e
        
        e = ((a + b) * c) / d     # (30 * 15 ) / 5
        print "((a + b) * c) / d 运算结果为：",  e
        
        e = (a + b) * (c / d);    # (30) * (15/5)
        print "(a + b) * (c / d) 运算结果为：",  e
        
        e = a + (b * c) / d;      #  20 + (150/5)
        print "a + (b * c) / d 运算结果为：",  e
    以上实例输出结果：
        (a + b) * c / d 运算结果为： 90
        ((a + b) * c) / d 运算结果为： 90
        (a + b) * (c / d) 运算结果为： 90
        a + (b * c) / d 运算结果为： 50  
####列表
       []内以逗号分隔，按照索引，存放各种数据类型，每个位置代表一个元素
#####列表的生成
       name = ['zhangsan','lisi']
#####列表的常用方法
       未完待续晚上写。。。。