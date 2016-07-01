#函数式编程（2）

---
##返回函数
> 高阶函数除了可以接受函数作为参数外，还可以把函数作为结果值返回。  
  
我们来实现一个可变参数的求和,但是不需要立刻求和，而是在后面的代码中根据需要再计算,可以不返回求和的结果，而是返回求和的函数：

	def lazy_sum(*args):
	    def sum():
	        ax = 0
	        for n in args:
	            ax = ax + n
	        return ax
	    return sum

当我们调用lazy_sum()时，返回的并不是求和结果，而是求和函数：  
	
	>>> f = lazy_sum(1, 3, 5, 7, 9)
	>>> f
	<function lazy_sum.<locals>.sum at 0x101c6ed90>
	
调用函数f时，才真正计算求和的结果
	
	>>> f()
	25

每个生成的函数都是独立的，及时参数一样，他们也是不同的

	>>> f1 = lazy_sum(1, 3, 5, 7, 9)
	>>> f2 = lazy_sum(1, 3, 5, 7, 9)
	>>> f1==f2
	False

###闭包
> 注意到返回的函数在其定义内部引用了局部变量args，所以，当一个函数返回了一个函数后，其内部的局部变量还被新函数引用，所以，闭包用起来简单，实现起来可不容易。  
  
另一个需要注意的问题是，返回的函数并没有立刻执行，而是直到调用了f()才执行。  

	def count():
	    fs = []
	    for i in range(1, 4):
	        def f():
	             return i*i
	        fs.append(f)
	    return fs
	
	f1, f2, f3 = count()
	
	#期望是1，4，9
	>>> f1()
	9
	>>> f2()
	9
	>>> f3()
	9
	
返回闭包时牢记的一点就是：返回函数不要引用任何循环变量，或者后续会发生变化的变量。  

如果一定要引用循环变量怎么办？方法是再创建一个函数，用该函数的参数绑定循环变量当前的值，无论该循环变量后续如何更改，已绑定到函数参数的值不变

	def count():
	    def f(j):
	        def g():
	            return j*j
	        return g
	    fs = []
	    for i in range(1, 4):
	        fs.append(f(i)) # f(i)立刻被执行，因此i的当前值被传入f()
	    return fs  
  
###小结  
一个函数可以返回一个计算结果，也可以返回一个函数。

返回一个函数时，牢记该函数并未执行，返回函数中不要引用任何可能会变化的变量。

---

##匿名函数  
当我们在传入函数时，有些时候，不需要显式地定义函数，直接传入匿名函数更方便。  
在Python中，对匿名函数提供了有限支持。还是以map()函数为例，计算f(x)=x2时，除了定义一个f(x)的函数外，还可以直接传入匿名函数：  

	>>> list(map(lambda x: x * x, [1, 2, 3, 4, 5, 6, 7, 8, 9]))
	[1, 4, 9, 16, 25, 36, 49, 64, 81]  
  
通过对比可以看出，匿名函数lambda x: x * x实际上就是：  

	def f(x):
	    return x * x  
  
关键字`lambda`表示匿名函数，冒号前面的x表示函数参数。

匿名函数有个限制，就是只能有`一个表达式`，不用写`return`，返回值就是该表达式的结果。  
  
用匿名函数有个好处，因为函数没有名字，不必担心函数名冲突。此外，匿名函数也是一个函数对象，也可以把匿名函数赋值给一个变量，再利用变量来调用该函数：  
  
	>>> f = lambda x: x * x
	>>> f
	<function <lambda> at 0x101c6ef28>
	>>> f(5)
	25  
同样，也可以把匿名函数作为返回值返回，比如： (`作用如同返回函数`)

	def build(x, y):
	    return lambda: x * x + y * y  
  
###小结

Python对匿名函数的支持有限，只有一些简单的情况下可以使用匿名函数。

---
##装饰器
>在代码运行期间动态增加功能的方式，称之为“装饰器”（Decorator）。


	def log2(func):
	    @functools.wraps(func)#然装饰完注的函数名字与装饰前相同
	    def wrapper(*args, **kw):
	        print('call %s():' % func.__name__)
	        return func(*args, **kw)
	    return wrapper
	
	
	@log2#@log2,相当于log2(f2)
	def f2():
	    print('hello')
	
	f2()

---
##偏函数

>当函数的参数个数太多，需要简化时，使用`functools.partial`可以创建一个新的函数，这个新函数可以`固定住原函数的部分参数`，从而在调用时更简单。

	例如int(self, x, base=10)字符串或数字转数字的函数
	Convert a number or string to an integer, or return 0 if no arguments
        are given.  If x is a number, return x.__int__().  For floating point
        numbers, this truncates towards zero.
        
    x为转换的字符串或数字，base为该参数的进制数
    
    int2 = functools.partial(int, base=2)
    等效于生成了一个base默认为2的函数




---