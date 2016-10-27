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
        >>> user_dict = dict.fromkeys(['a','b','c'],1)
        >>> user_dict
        {'c': 1, 'a': 1, 'b': 1}
    4、get(self, k, d=None)
        '''
            key –> 字典中要查找的键
            default –> 如果指定键的值不存在时，返回该默认值值
        '''
        #返回指定键的值，如果值不在字典中返回默认值
        >>> user_dict = {'name':'zhangsan','age':18}
        >>> user_dict.get('name')
        'zhangsan'
        >>> print(user_dict.get('name1'))
        None
        >>> print(user_dict.get('name1','没有'))
        没有
    5、has_key(self, k)
        #
        
    
    
