# async_coroutine
### 划重点
    今天聊聊协程+异步，基于asyncio/await 实现的异步协程

### 1 了解一下
1. 阻塞状态：指程序未得到所需计算资源时被挂起的状态，如网络 I/O 阻塞、磁盘 I/O 阻塞、用户输入阻塞等
* 非阻塞：程序等待某操作过程中，自身不被阻塞，可以继续做其他事情
* 同步：做完一件事再去做另外一件
* 异步：无需等待，一件事卡住了可以先去做另外一件事
* 关系：进程（CPU 的多核优势） --> 主线程 --> 协程（用户态轻量级线程）
* 协程：
     * 拥有自己的寄存器上下文及栈，切换自如
     * 本质是单进程，相比多进程，无需线程上下文切换的开销，无需原子操作锁定及同步的开销
* 协程应用：使用协程来实现异步操作，网络请求等待时，可以同时处理其他事情，得到响应后切换回来继续处理，充分利用CPU和其他资源，凸显异步协程的优势

### 2 基本概念
* asyncio：基本库
* aiohttp：协助库
* event_loop: 事件循环
* coroutine： 协程，用async关键字定义方法调用时即可返回一个协程对象
* task：对协程对象的进一步封装，包含了任务的各个状态（pending/准备、running、finished）
* future：将来执行或没有执行的任务结果，同task

简单认识：

	    async def excute(x):
		     print('x: ', x)
		     return x

	    coroutine = excute(100)
	    loop = asyncio.get_event_loop()       # 创建事件循环loop
	    # loop.run_until_complete(coroutine)  # 将协程注册到事件循环，封装成task并执行 (这句可以代替后面两句)
	    task = loop.create_task(coroutine)    # 显式声明task，相当于上面一句（此时task是pending态）  
	    loop.run_until_complete(task)         # 此时task finished态
	    print(task.result())                  # 获取 task 的结果，finished态才有结果

        async func --> coroutine --> task --> event_loop

### 3 运用
	import asyncio 
	import requests
	import aiohttp   # aiohttp 是一个支持异步请求的库，利用它和 asyncio 配合我们可以非常方便地实现异步请求操作
	import time
		
	start = time.time()


        async def request():                        -----> f1
	       url = "http://localhost:8080/"
	       status = requests.get(url)
       	    return status                     


        async def request():                        -----> f2
		url = "http://localhost:8080/"
                session = aiohttp.ClientSession()
	        response = await session.get(url)   # 点睛之笔，耗时转用其他协程。
	        return result.status
      
         # 如果遇到了await，那么就会将当前协程挂起，转而去执行其他的协程,直到其他的协程也挂起或执行完毕，再进行下一个协程的执行
	   有空可以看看await的说明，不是简单声明就可以使用了，所以这里才会借助aiohttp


        loop = asyncio.get_event_loop()   # 创建事件循环loop
	# mutiltask
	tasks = [loop.create_task(request()) for _ in range(5)]  # 5个task
	loop.run_until_complete(asyncio.wait(tasks))
	for task in tasks:
             print(task.result())
		
	end = time.time()
	print('time:', end - start)     
        
        # f1 --->   time: 15.053324222564697 
        # f2 --->   time:  3.051652431488037
        



        说明： "http://localhost:8080/"是本地beego框架起的服务，特意将对应的处理函数睡眠了3秒
              func (c *MainController) Get() {
			time.Sleep(time.Duration(3)*time.Second)
			c.TplName = "index.html"
              }
