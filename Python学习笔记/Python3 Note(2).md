#Python3 笔记  (2) --高级特性

---
##切片
给list或者tuple指定范围筛选结果    
格式：[开始下标:结束下标:遍历规则]    

遍历规则：  
例如2即为每两位取值，-1即为逆序取值

	nums = list(range(15))
	
	
	print(nums[:10])
	>>>[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
	
	print(nums[5:10])
	>>>[5, 6, 7, 8, 9]
	
	print(nums[::2])
	>>>[0, 2, 4, 6, 8, 10, 12, 14]
	
	print(nums[::-1])
	>>>[14, 13, 12, 11, 10, 9, 8, 7, 6, 5, 4, 3, 2, 1, 0]
	
---  
	
##迭代类型-(遍历)

	d = {'a' : 1,'b' : 2,'c' : 3}
###字典遍历
	for key in d.keys():
	    print(key)
	
	for value in d.values():
	    print(value)
	
	for k,y in d.items():
	    print(k,':',y)

###枚举器
	n = ['A','B','C','D','E']
	for i,value in enumerate(n):
	    print(i,value)

###判断是否为迭代类型  
	#导入头文件
	from collections import Iterable
	print(isinstance(d,Iterable))
	>>>True
	isinstance(123, Iterable) # 整数是否可迭代
	>>>False 
Iterable是判断的类型，这里是迭代类型，可替换成其他类型 
  
---
##列表生成器
	
定义特定条件的单元，生成列表

	nums = list(range(100))
	#从0~20位，取x%2为0的每个元素，并平方后，保存在nums2中
	nums2 = [x * x for x in nums[:20] if x % 2 == 0]

格式：[每个元素格式 for 每个参考list的元素 in 参考list if 参考list的条件]，返回符合条件的list

---
##生成器--generator
generator有点类似于对象

通过列表生成式，我们可以直接创建一个列表。但是，受到内存限制，列表容量肯定是有限的。而且，创建一个包含100万个元素的列表，不仅占用很大的存储空间，如果我们仅仅需要访问前面几个元素，那后面绝大多数元素占用的空间都白白浪费了。

所以，如果列表元素可以按照某种算法推算出来，那我们是否可以在循环的过程中不断推算出后续的元素呢？这样就不必创建完整的list，从而节省大量的空间。在Python中，这种一边循环一边计算的机制，称为生成器：generator。

###要创建一个generator，有很多种方法。第一种方法很简单，只要把一个列表生成式的[]改成()，就创建了一个generator：  
  
	>>> L = [x * x for x in range(10)]
	>>> L
	[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
	>>> g = (x * x for x in range(10))
	>>> g
	<generator object <genexpr> at 0x1022ef630>  
  
创建L和g的区别仅在于最外层的`[]`和`()`，L是一个list，而g是一个generator。

我们可以直接打印出list的每一个元素，但我们怎么打印出generator的每一个元素呢？

如果要一个一个打印出来，可以通过`next()`函数获得generator的下一个返回值：

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

当然，上面这种不断调用next(g)实在是太变态了，正确的方法是使用for循环，因为generator也是可`迭代对象`  

	著名的斐波拉契数列（Fibonacci），除第一个和第二个数外，任意一个数都可由前两个数相加得到：
	
	1, 1, 2, 3, 5, 8, 13, 21, 34, ...
	
	斐波拉契数列用列表生成式写不出来，但是，用函数把它打印出来却很容易：
	
	def fib(max):
	    n, a, b = 0, 0, 1
	    while n < max:
	        print(b)
	        a, b = b, a + b
	        n = n + 1
	    return 'done'
	上面的函数可以输出斐波那契数列的前N个数：
	
	>>> fib(6)
	1
	1
	2
	3
	5
	8
	'done'
	仔细观察，可以看出，fib函数实际上是定义了斐波拉契数列的推算规则，可以从第一个元素开始，推算出后续任意的元素，这种逻辑其实非常类似generator。
	
	也就是说，上面的函数和generator仅一步之遥。要把fib函数变成generator，只需要把print(b)改为yield b就可以了：
	
	def fib(max):
	    n, a, b = 0, 0, 1
	    while n < max:
	        yield b
	        a, b = b, a + b
	        n = n + 1
	    return 'done'
	这就是定义generator的另一种方法。如果一个函数定义中包含yield关键字，那么这个函数就不再是一个普通函数，而是一个generator    

####生成器同样是可迭代对象，但是你只能读取一次，因为它并没有把所有值存放内存中，它动态的生成值

###通过`yield`,可以将这个函数变成generator  
  
###小结

generator是非常强大的工具，在Python中，可以简单地把列表生成式改成generator，也可以通过函数实现复杂逻辑的generator。

要理解generator的工作原理，它是在for循环的过程中不断计算出下一个元素，并在适当的条件结束for循环。对于函数改成的generator来说，遇到return语句或者执行到函数体最后一行语句，就是结束generator的指令，for循环随之结束。

请注意区分普通函数和generator函数，普通函数调用直接返回结果：

	>>> r = abs(6)
	>>> r
	6
generator函数的“调用”实际返回一个generator对象：

	>>> g = fib(6)
	>>> g
	<generator object fib at 0x1022ef948>

---
##迭代器 
凡是可作用于for循环的对象都是`Iterable`类型；

凡是可作用于`next()`函数的对象都是`Iterator`类型，它们表示一个惰性计算的序列；

集合数据类型如list、dict、str等是`Iterable`但不是`Iterator`，不过可以通过`iter()`函数获得一个`Iterator对象`。