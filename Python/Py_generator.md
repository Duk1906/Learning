## 生成器
### 1 key
   > 生成一个可迭代的对象（具有g.next()/g.__next__()方法）
### 2 介绍两种方式
* 列表推倒式的 [] 改成 () 即可

eg1：

    lst = [1, 2, 3, 4, 5, 6, 7]
    g = (x*x for x in lst)
    print(g)
    输出结果：
    C:\Users\P\AppData\Local\conda\conda\envs\xp35\python.exe D:/00_LeetCode/Py_generator.py
    <generator object <genexpr> at 0x000001CCF4C1F990>

* 使用yield（用到yield的函数是生成器函数）

eg2：

    def generator():
	    for i in range(10):
	        yield i

	gen = generator()
	for j in gen:           //推荐使用for循环遍历生成器
	    print(j)
	print(gen.__next__())   //生成器里的元素是一次性的，用完后再调用next()方法时会报"StopIteration"错误

    tips：可以考虑将generator转换成list以实现持久使用，如 lst = list(gen)

### 3 斐波那契数列（Fibonacci sequence）
      fbnc = [0, 1, 1, 2, 3, 5, 8, 13, 21, 34, ......]
      题目：返回斐波那契数列数列的第 n 个元素,n从0数起
      input: n
      output: 第 n 个元素

* 递归实现

eg3:

	  def recursion(n):         # 递归
	     if n == 0:
	        return 0
	     elif n == 1:
	        return 1
	     else:
	        return recursion(n-1) + recursion(n-2)


	  if __name__ == '__main__':
	      begin = time.time()
	      print(recursion(35))
	      end = time.time()
	      print('耗时：', end - begin)

      输出结果：
          9227465
          耗时： 9.520537614822388
      可见，当n=35的时候，就要等好几秒才可以出结果了

* 生成器实现

eg4：
 
     def fib_generator(n):
	     x, a, b = 0, 0, 1
	     while x <= n:
	         yield a
	         a, b = b, a+b
	         x += 1


	 if __name__ == '__main__':
	     begin = time.time()
	     print(list(fib_generator(100000))[-1])
	     end = time.time()
	     print('耗时：', end - begin)

     输出结果：
        25974069.....
        耗时： 0.6815938949584961
     可见，100k级别妙算。将n调整到200k，耗时： 16.25570511817932


* 普通for循环实现

eg5：

    def fib_for(n):
    x, a, b = 0, 0, 1
    while x < n:
        a, b = b, a+b
        x += 1
    return a


	if __name__ == '__main__':
	    begin = time.time()
        print(fib_for(100000))
        end = time.time()
        print('耗时：', end - begin)

    输出结果：
        25974069.....
        耗时： 0.2928318977355957
     可见，100k级别也是妙算，似乎比生成器实现的更快。现在将n调整到200k，耗时：0.8576185703277588，
     这时候可以确定比生成器实现的方案更快，下面列一下其他n对应的耗时：
        n              耗时
        500k           5.800682067871094
        700k           9.689306497573853     10s 分界点
        800k           12.973601341247559
        900k           16.46160387992859     生成器200k的耗时
        1000k          19.848633289337158

        