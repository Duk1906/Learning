## 装饰器
 1. key：用来装饰函数，不修改函数的情况下，执行一些额外的动作
 2. 举个例子，一个简单的函数如eg1，现在我们想在调用summary前检测x与y值的类型，如果非int类型就报错。如果不想修改summary，则可以用装饰器来实现，看eg2。

eg1：

     def summary(x, y):
         return x+y
eg2:

     def decoration(fn):
	    def check(x, y):      # 这里x,y参数是被装饰函数传递的
	        if not isinstance(x, int):
	            print('x非int类型')
	            return
	        if not isinstance(y, int):
	            print('y非int类型')
	            return
	        return fn(x, y)
	    return check


	@decoration
	def summary(x, y):       # 这里x,y对应传递到check(x, y)
	    return x+y


    if __name__ == '__main__':
	    s = summary('k', 5)
	    print(s)

    输出结果：
      x非int类型
      None
