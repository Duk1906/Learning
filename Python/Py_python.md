### python点滴
##### 1. 常用模块
   1. dis:反汇编工具,将Python代码翻译成字节码指令
      1. dis.dis(fn)
   2. sys： 
      1. 获取命令行参数 sys.argv[0]
      2. 获取引用次数 sys.getrefcount(x)
   3. time
      1. time.strptime(str,fmt='%a %b %d %H:%M:%S %Y')
      2. time.sleep(1)
   4. os
      1. os.getpid()
      2. os.open()
      3. os.close()
   5. random
      1. 随机整数 random.randint(a,b)
      2. 随机小数 numpy.random.randn(5)
   6. copy 下面 第8点
           
##### 2. is 和 == 的区别
    is 是检查两个对象是否指向同一块内存空间，而 == 是检查他们的值是否相等。
    
##### 3. 可变对象 vs 不可变对象
    dict、list是可变对象，str、int、tuple、float是不可变对象
    延申：
		 eg1：          a = [1, 2]
		                b = a         // a,b指向了同一块内存
		                b.append(3)
		                print(a, b, id(a), id(b))   //  [1, 2, 3] [1, 2, 3] 2310587398280 2310587398280

		 eg2：          a = 'hello'
			        b = a
		         	print(id(a), id(b), a, b)   // 2618086222624 2618086222624 hello hello
				a += 'lxp'
			        print(id(a), id(b), a, b)   //  2618088668592 2618086222624 hellolxp hello     
                        key：b = a的时候，b指向了和a一样的内容为'hello'的内存地址2618086222624；字符串是不可变对象，
			        执行a += 'lxp'时，系统分配了新的内存块2618088668592去存储新生成的字符串'hellolxp',并将变量a指向这个新分配的地址

             eg3：          a = 'hello'   
				b = 'hello'   
				print(id(a), id(b))    //  2298988751648 2298988751648 

				c = []  
				d = []
				print(id(c), id(d))   //  1843333085320 1843333083208
			    key:系统会对小对象进行缓存，接下来的引用会指向同一内存，如 'hello', 1,1.11; 但是 [],{},(1,),'hello,world'*2 就不会。

          
             eg4:         def test(lst=[]):    
				   lst.append(1)
				   print(id(lst))   // 1688817744008
				   return lst
			       x = test()
			       print(id(x), x)      // 1688817744008 [1]
			       y = test([1, 2])
			       print(id(x), id(y), x, y)  // 1688817744008 1688817741896 [1] [1, 2, 1]
			       z = test()
			       print(id(x), id(y), id(z), x, y, z)  // 1688817744008 1688817741896 1688817744008 [1, 1] [1, 2, 1] [1, 1]
##### 4. join vs +
      ''.join(strlist) --> 快
      for str in strlist:
          result += str  --> 慢，且浪费空间，美加一次分配一块新内存
##### 5. _ _new_ _(cls)和 _ _init_ _(self)
      __new__：新建类，以类为首参，会返回一个类实例，伴随__init__
      __init__:初始化类，以类实例为首参，无返回
      
##### 6. with 上下文管理,帮忙关闭文件==
    with open('output', 'w') as f:     //output没有会自动新建
        f.write('hi,lxp')
    上面代码等价于：
    try:
        f = open('output', 'w')
        f.write('hi,lxp')
    finally:
        f.close()
	
##### 7. 异常捕捉_断言
    无论异常是否发生，在程序结束前，finally中的语句都会被执行。
    try:
        a = 1/0
    except ZeroDivisionError:          // 捕捉指定错误，可省略，捕捉全部错误
        print('ZeroDivisionError')
    else:           // 可省略，无exceptoin 会走 else
        print('fine')
    finally:
        print('whatever happened, I still excute')
    assert
       1. 判断assert后面紧跟的语句是True还是False
       2. 如果是True则继续执行
       3. 如果是False则中断程序，调用默认的异常处理器，同时输出assert语句逗号后面的提示信息

##### 8. 深浅copy  
    import copy
    浅：copy.copy()
    深：copy.deepcopy()
    1. 对于不可变类型的赋值、浅拷贝、深拷贝在内存当中用的都是同一块地址
    2. 对于浅拷贝，字典、列表等可变类型，它们只拷贝第一层地址
    3. 对于深拷贝，字典、列表等可变类型，它里面嵌套多少层，就会拷贝多少层出来，但是最底层的不可变类型地址不变
    4. 应用：用deepcopy拷贝数组的值就不用共享内存了
            a = [1]
	    b = copy.deepcopy(a)
	    print(id(a), id(b))   // 2766068833544 2766068822216
	    
##### 9. 设计模式
    1. 单例模式
       某个类只有一个实例存在，适用于 系统的配置文件等
       思路：设置一个类属性，调用类的__new__方法时先判断为空才实例化类，确保类只有一个实例
            class SingleTon(object):
	          __instance = None
	          age = None
	          name = None
			
		  def __new__(cls, age, name):
		      if not cls.__instance:
			   cls.__instance = object.__new__(cls)
			   cls.age = age
			   cls.name = name
		           return cls.__instance
			
	    lxp = SingleTon(21, "lxp")
	    czq = SingleTon(21, "czq")  // 结果表明，这个实例化并没有生效
			
	    print(id(lxp), id(czq))   // 3068614188168 3068614188168
	    print(lxp.name, czq.name)   // lxp lxp
    2. 工厂模式（工厂：流水线批量生产）
       按需生产对象，一类多实例
    ![](https://www.cnblogs.com/tangkaishou/p/9246353.html)

##### 10. 内存管理 
    1. 引用计数：+-
    2. 分代回收：0、1、2
    3. 孤立引用环: 遍历每个变量，谁引用了我，谁减1，最后为0的回收

##### 11. 多进程 + 多线程 （windows）
     1   from multiprocessing import Process，Pool
		
         p = Process(target=function, args=())
	 p.start()
         p.join()
		
         pool = Pool()
	 pool.map(func, lst)   # map 第二个参数是任务列表
	 pool.close()
	 pool.join()
		     
     2   import threading
         threadLock = threading.Lock()
         
   * 进程：
     1. 操作系统进行资源分配和调度的基本单位，多个进程之间相互独立
     2. 稳定性好，如果一个进程崩溃，不影响其他进程，但是进程消耗资源大，开启的进程数量有限制
   * 线程：
     1. CPU进行资源分配和调度的基本单位，线程是进程的一部分，是比进程更小的能独立运行的基本单位，一个进程下的多个线程可以共享该进程的所有资源
     2. 如果IO操作密集，则可以多线程运行效率高，缺点是如果一个线程崩溃，都会造成进程的崩溃
     3. 每个线程都有他自己的一组CPU寄存器，称为线程的上下文，该上下文反映了线程上次运行该线程的CPU寄存器的状态。
     4. 指令指针和堆栈指针寄存器是线程上下文中两个最重要的寄存器，线程总是在进程得到上下文中运行的，这些地址都用于标志拥有线程的进程地址空间中的内存。
     5. 线程可以被抢占（中断）。
     6. 在其他线程正在运行时，线程可以暂时搁置（也称为睡眠） -- 这就是线程的退让
   * 应用：
     1. IO密集的用多线程，在用户输入，sleep 时候，可以切换到其他线程执行，减少等待的时间
     2. CPU密集的用多进程，因为假如IO操作少，用多线程的话，因为线程共享一个全局解释器锁，当前运行的线程会霸占GIL，其他线程没有GIL，就不能充分利用多核CPU的优势

##### 12. 进程通信 Queue、Pipes
                from multiprocessing import Process, Queue
                import os, time, random
		
		# 写数据进程执行的代码:
		def write(q):
		    for value in ['A', 'B', 'C']:
		        print 'Put %s to queue...' % value
		        q.put(value)
		        time.sleep(random.random())
		
		# 读数据进程执行的代码:
		def read(q):
		    while True:
		        value = q.get(True)
		        print 'Get %s from queue.' % value
		
		if __name__=='__main__':
		    # 父进程创建Queue，并传给各个子进程：
		    q = Queue()
		    pw = Process(target=write, args=(q,))
		    pr = Process(target=read, args=(q,))
		    # 启动子进程pw，写入:
		    pw.start()
		    # 启动子进程pr，读取:
		    pr.start()
		    # 等待pw结束:
		    pw.join()
		    # pr进程里是死循环，无法等待其结束，只能强行终止:
		    pr.terminate()
      
##### 13. 协程/gevent 为何比线程/threading还快
   > 当一个greenlet遇到IO操作时，比如访问网络，就自动切换到其他的greenlet，等到IO操作完成，再在适当的时候切换回来继续执行。由于IO操作非常耗时，经常使程序处于等待状态，有了gevent为我们自动切换协程，就保证总有greenlet在运行，而不是等待IO
   
   > 由于gevent是基于IO切换的协程，所以最神奇的是，我们编写的Web App代码，不需要引入gevent的包，也不需要改任何代码，仅仅在部署的时候，用一个支持gevent的WSGI服务器，立刻就获得了数倍的性能提升

##### 14. fliter(), map()
      def func(x):
	      return x % 20 == 0
      lst = filter(func, range(100))    // filter 的首参func返回的是筛选条件  
      print(list(lst), type(lst))  // [0, 20, 40, 60, 80] <class 'filter'>

      def func(x):
          return x - 20  
      lst = map(func, range(10))   // map 的func可以返回操作后的x，也可以是x的操作
      print(list(lst), type(lst))

##### 15. 匿名函数lambda
      sum = lambda a,b:a+b
      print(sum(3, 5))

##### 16. Python搜索变量的顺序
      本地作用域（Local）→当前作用域被嵌入的本地作用域（Enclosing locals）
      →全局/模块作用域（Global）→内置作用域（Built-in）
      
##### 17. *args 与 **kwargs
      *args : []
      **kwargs: key-value

##### 18. 简述同源策略

	 同源策略需要同时满足以下三点要求： 

	 1）协议相同 
	 2）域名相同 
	 3）端口相同 
	
	 http:www.test.com与https:www.test.com 不同源——协议不同 
	 http:www.test.com与http:www.admin.com 不同源——域名不同 
	 http:www.test.com与http:www.test.com:8081 不同源——端口不同

	 只要不满足其中任意一个要求，就不符合同源策略，就会出现“跨域”

##### 19. 简述cookie和session的区别

	1. session 在服务器端，cookie 在客户端（浏览器）	
	2. session 的运行依赖 session id，而 session id 是存在 cookie 中的，也就是说，如果浏览器禁用了 cookie ，同时 session 也会失效，存储Session时，键与Cookie中的sessionid相同，值是开发人员设置的键值对信息，进行了base64编码，过期时间由开发人员设置
	3. cookie安全性比session差
	
##### 20. python中什么元素为假
        0，空字符串，空列表、空字典、空元组、None, False
##### 21. python异常
	IOError：输入输出异常

	AttributeError：试图访问一个对象没有的属性

	ImportError：无法引入模块或包，基本是路径问题

	IndentationError：语法错误，代码没有正确的对齐

	IndexError：下标索引超出序列边界

	KeyError:试图访问你字典里不存在的键

	SyntaxError:Python代码逻辑语法出错，不能执行

	NameError:使用一个还未赋予对象的变量
##### 22.列出几种魔法方法并简要介绍用途
	
	__init__:对象初始化方法
	
	__new__:创建对象时候执行的方法，单列模式会用到
	
	__str__:当使用print输出对象的时候，只要自己定义了__str__(self)方法，那么就会打印从在这个方法中return的数据
	
	__del__:删除对象执行的方法

##### 23. sys.argv
    >python 1.py 22 33
    >print(sys.argv)   // ['1.py', 22, 33] 

##### 24. 去除字符串空格
      方法1：  string = ' fg fxd '
	      lst = string.split(' ')
	      res = ''.join(lst)
	      print(res)

     方法2：  stri = string.replace(' ', '')   // 首选推荐
	      print(stri)

##### 25.举例sort和sorted对列表排序，list=[0,-1,3,-10,5,9]
     lst = [0, -1, 3, -10, 5, 9]
     lst.sort()
     res = sorted(lst, reverse=False)  // sorted()会生成新list
    
     延申：1 列表嵌套字典的排序
             foo = [{"name": "zs", "age": 19}, {"name": "ll", "age": 54},
                    {"name": "wa", "age": 17}, {"name": "df", "age": 23}]
             a = sorted(foo, key=lambda x: x['age']) // x代表foo中的每个元素
             print(a)  // [{'age': 17, 'name': 'wa'}, {'age': 19, 'name': 'zs'}, {'age': 23, 'name': 'df'}, {'age': 54, 'name': 'll'}]

          2 列表嵌套元组/列表
             foo = [(20, 'lxp'), (21, 'czq')]
	     a = sorted(foo, key=lambda x: x[1])  // 按列表的字符串排序
             #  a = sorted(foo, key=lambda x: (x[1],x[0]))  // 先按名字后按年龄
	     print(a)  // [(21, 'czq'), (20, 'lxp')]

          3 对字典的键/值排序（关键是键/值是可比较的）
             foo = {"name": 'lxp', "age": 19}
	     print(foo.items())
	     a = sorted(foo.items(), key=lambda x: x[0], reverse=False)
	     new_dict = {x[0]: x[1] for x in a}
             print(new_dict)

          4 列表嵌套字符串，根据字符串长度排序，改变列表本身/无额外内存消耗
            foo = ['1', '12', '14', '1234']
	    foo.sort(key=lambda x: len(x), reverse=False)
	    print(foo)  
            // ['1', '12', '14', '1234']
##### 26. dict and json字符串
        dic = {'xp': 21, 'aq': 21}
        jsn = json.dumps(dic)
        print(jsn, type(jsn))    // {"aq": 21, "xp": 21} <class 'str'>
        jsn_dic = json.loads(jsn)
        print(jsn_dic, type(jsn_dic))   // {'aq': 21, 'xp': 21} <class 'dict'>

##### 27. 类型转换
        a = int(1.11)
	print(a)    // 1
	b = float('1.11')
	print(b)    // 1.11
	c = int('1.11')
	print(c)    // 报错

##### 28. PEP8
	1. import置顶，顶级定义之间空两行，比如函数或者类定义。
	2. 方法定义、类定义与第一个方法之间，都应该空一行
	3. 三引号进行注释
        4. 使用Pycharm一般使用4个空格来缩进代码

##### 29. 乐观锁和悲观锁
    乐观锁：乐观态度不上锁
    悲观锁：悲观怕取数据时出错(别人会修改之类的)，要上锁

##### 30. python参数传递
    def self_add(z):
	    z += z
	    print('里面的：', id(z), z)

    x = 10
    self_add(x)                   //   里面的： 1427862768 20 ，里面的x是新建的局部变量
    print('外面的：', id(x), x)    //   外面的： 1427862448 10
	
    y = [1] 
    self_add(y)                   //   里面的： 2123441735176 [1, 1]
    print('外面的：', id(y), y)    //   外面的： 2123441735176 [1, 1]

##### 31. HTTP请求中get和post区别

1. GET请求是通过URL直接请求数据，数据信息可以在URL中直接看到，比如浏览器访问；而POST请求是放在请求头中的，我们是无法直接看到的；

2. GET提交有数据大小的限制，一般是不超过1024个字节，而这种说法也不完全准确，HTTP协议并没有设定URL字节长度的上限，而是浏览器做了些处理，所以长度依据浏览器的不同有所不同；POST请求在HTTP协议中也没有做说明，一般来说是没有设置限制的，但是实际上浏览器也有默认值。总体来说，少量的数据使用GET，大量的数据使用POST。

3. GET请求因为数据参数是暴露在URL中的，所以安全性比较低，比如密码是不能暴露的，就不能使用GET请求；POST请求中，请求参数信息是放在请求头的，所以安全性较高，可以使用。在实际中，涉及到登录操作的时候，尽量使用HTTPS请求，安全性更好。

