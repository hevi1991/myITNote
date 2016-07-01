#Python 函数式编程（1）
---
##高阶函数
核心概念：函数可以作为变量传递

	def add(x, y, f):
	    return f(x) + f(y)  
    
---  
##map/reduce  
    
###1.map  
	map函数接收两个参数，作用让迭代的每个元素调用函数，然后返回每个结果的Iterator（迭代器对象）
	第一个参数:函数名
	第二个参数:iterable（可迭代的）
	
###2.reduce
	reduce函数必须接收两个参数，reduce把结果继续和序列的下一个元素做累积计算  
	
	reduce(f, [x1, x2, x3, x4]) == f(f(f(x1, x2), x3), x4)
	
	例子：
	r = reduce(f,[1,2,3,4,5,6,7,8,9])

	print(r)>>45
	    
###3.filter
	接收一个函数和一个序列，把传入的函数依次作用于每个元素，然后根据返回值是True还是False决定保留还是丢弃该元素。(Ture留,False弃)
	
	def is_odder(x):
    	return x % 2 == 1

	print(list(filter(is_odder,[1,2,3,4,5,6,7])))  
	>>>[1, 3, 5, 7]
	  
#####练习：
	# 回数是指从左向右读和从右向左读都是一样的数，例如12321，909。请利用filter()滤掉非回数：
	# 测试:

	def is_palindrome(n):
	    return str(n) == str(n)[::-1]
	
	output = filter(is_palindrome, range(1, 1000))
	print(list(output))
  
###4.sorted
	排序的核心是比较两个元素的大小，作用进行排序（3个参数，返回可迭代类型）
	一个必选参数：可迭代的类型
	key=：添加一个函数，根据返回值来排序可迭代类型
	reverse=：True反向排列，False默认为正向排列
	
	例子：
	LX1 = ['King','Quan','Alaxi','Misaya','aBo','b19']

	L3 = sorted(LX1,key=str.lower)
	
	L4 = sorted(LX1,key=str.lower,reverse=True)  
  


  
