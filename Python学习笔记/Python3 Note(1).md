#Python3 笔记  (1) --基础

---
##1.基础IO:
###输出:

	print()
	变量直接放入
	字符串需要添加''或""包含  
###输入:  
	input()
	可在括号内添加字符串作为提示.
	a = input()
	可以用作赋值,返回类型是字符串

###互转字符<->编码
	ord()字符串转编码
	输入:ord('A')
	结果:65  

	chr()编码转字符串
	输入:chr(65)
	结果:A  
###字符串压缩/解压 bytes  
	encode()压缩函数,括号内添加字符串编码标准
	输入:'ABC'.encode('utf-8')  
	结果:b'ABC'
	  
	decode()解压函数,括号内添加字符串编码标准
	输入:b'ABC'.decode('utf-8')
	结果:'ABC'  
###计算长度(根据计算的内容不同,结果不同)
	len()括号内添加字符串  
	字符串->字符数
	输入:len('ABC')
	结果:3
	
	bytes类型->字节数
	输入:len('中文'.encode('utf-8'))
	结果:6
	  
###当Python解释器读取源代码时，为了让它按UTF-8编码读取，我们通常在文件开头写上这两行

	#!/usr/bin/env python3
	# -*- coding: utf-8 -*-  
  
###占位符
	常见的占位符有：
	%d	整数
	%f	浮点数
	%s	字符串
	%x	十六进制整数  
	
格式化整数和浮点数还可以指定是否补0和整数与小数的位数：

	>>> '%2d-%02d' % (3, 1)
	' 3-01'
	>>> '%.2f' % 3.1415926
	'3.14'
如果你不太确定应该用什么，%s永远起作用，它会把任何数据类型转换为字符串：
	
	>>> 'Age: %s. Gender: %s' % (25, True)
	'Age: 25. Gender: True'
有些时候，字符串里面的%是一个普通字符怎么办？这个时候就需要转义，用%%来表示一个%：
	
	>>> 'growth rate: %d %%' % 7
	'growth rate: 7 %'  

###赋值用%

	#练习
	#小明的成绩从去年的72分提升到了今年的85分，请计算小明成绩提升的百分点，并用字符串格式化显示出'xx.x%'，只保留小数点后1位：
	s1 = 72
	s2 = 85
	r = (s2-s1)/s1 * 100
	print('小明提升成绩百分点:%.1f%%' % r)

---
##2.list(数组列表)

list是一种有序的集合，可以随时添加和删除其中的元素。  

	classmates = ['Peter','Anna','Sue','Tenny']  
  
变量classmates就是一个list。用`len()`函数可以获得list元素的个数  

	len(classmates)  
  
###用索引来访问list中每一个位置的元素，记得索引是从0开始的  
	classmates[2] -> Sue
	
	-1为倒数第一位
	
	会有越界问题  
###list是一个可变的有序表，所以，可以往list中追加元素到末尾`append()`
	classmates.append('Danne')  
  
###也可以把元素插入到指定的位置`insert(index,obj)`
	比如索引号为1的位置：
	classmates.insert(1,'Lori')
	
###要删除指定位置的元素，用`pop(i)`方法，其中i是索引位置：
	classmates.pop(0) #删除0号位  
###要把某个元素替换成别的元素，可以`直接赋值`给对应的索引位置：  
	classmates[1] = 'Jack'  
###list里面的元素的数据类型也可以不同，比如：
	L = ['Apple', 123, True] #list中可以继续包含list  

---

##3.tuple 元组(指向不变的list)  

###这个tuple不能变了，它也没有`append()`,`pop()`,`insert()`这样的方法。其他获取元素的方法和list是一样的，你可以正常地使用classmates[0]，classmates[-1]，但`不能赋值`成另外的元素。
	
	classmatesB = ('Aya','Robort','Stroke')    
  
####tuple的陷阱：当你定义一个tuple时，在定义的时候，tuple的元素就必须被确定下来  
	tuple = (1,2)
####只有1个元素的tuple定义时必须加一个逗号`,`，来消除歧义
	tuple = (1,)
	

---
##4.条件判断
	# 小明身高1.75，体重80.5kg。请根据BMI公式（体重除以身高的平方）帮小明计算他的BMI指数，并根据BMI指数：
	#
	# 低于18.5：过轻
	# 18.5-25：正常
	# 25-28：过重
	# 28-32：肥胖
	# 高于32：严重肥胖
	# 用if-elif判断并打印结果：
	
	height = 1.75
	weight = 80.5
	
	BMI = weight /(height * height)
	
	if BMI < 18.5:
	    print('过轻')
	elif BMI <= 25:
	    print('正常')
	elif BMI <= 28:
	    print('过重')
	elif BMI <= 32:
	    print('肥胖')
	else:
	    print('严重肥胖')  
  
---
##5.循环
###for 循环
	names = ['Peter','Anna','Sue','Bili','Zack']
	for name in names:
	    print(name)
	
###如果要计算1-100的整数之和，从1写到100有点困难，幸好Python提供一个`range()`函数，可以生成一个整数序列，再通过list()函数可以转换为list
	numbers = list(range(1,101))
	sum = 0;
	for x in numbers:
	    sum = sum + x
	print(sum)#5050
	
###while 循环
	sum = 0
	n = 100
	while n > 0:
	    sum = sum + n
	    n = n - 1
	print(sum)#5050  
  
####练习

	#请利用循环依次对list中的每个名字打印出Hello, xxx!：
	
	L = ['Bart', 'Lisa', 'Adam']
	
	for name in L:
	    print('Hello,%s' % name)

---
##6.dict 字典
###生产(dict 字典,无序排列)
	d = {'Ann': 99,'Bane':91,'Cary':89}
	
###取值
	print(d['Ann'])
	
###通过in判断key是否存在：
	r = 'Lori' in d
	print(r)
	
###通过dict提供的`get`方法，如果key不存在，可以返回None，或者自己指定的value:
	r = d.get('Lori')
	print(r)
	r = d.get('Lori',-1)
	print(r)
	
###删除一个Key-Value
	d.pop('Ann')
	print(d)
	
###加入一个Key-Value
	d['Omiga'] = 100
	print(d)
	
###dict的key必须是不可变对象
	f = {2:'Ann',3:'Perry'}
	print(f)
---
##7.Set 集合
###set和dict类似，也是一组key的集合，但不存储value。由于key不能重复，所以，在set中，没有重复的key,且元素无序排列
	
####要创建一个set，需要提供一个list作为输入集合
	sList = list(range(1,10))
	s = set(sList)
	print(s)
	
####值不重复
	sList = [1,1,2,2,3,3,3,4,5,1,12]
	s = set(sList)
	print(s)
	
####add()添加元素,若已存在会无效
	s.add(9)
	print(s)
	
####remove()删除元素
	s.remove(1)
	print(s)
	
####两个集合之间交集
	aset = set([1,2,3,4])
	bset = set([3,4,5,6])
#####交集
	cset = aset & bset
	print(cset)
	
#####并集
	dset = aset | bset
	print(dset)

---
##8.函数
Python内置了很多有用的函数，我们可以直接调用。
要调用一个函数，需要知道函数的名称和参数，比如求绝对值的函数abs，只有一个参数。可以直接从Python的官方网站查看文档：

http://docs.python.org/3/library/functions.html#abs

也可以在交互式命令行通过help(abs)查看abs函数的帮助信息。

###8.1调用函数
####`help()`查看函数的注释
	help(abs)
	
####例如`abs()`函数的作用就是返回参数的绝对值
	print(abs(-199))
	
####`max()`的作用返回参数中最大的数值
	a = max(11,124,1231,3123,12312,1,51,21,21166,1,71,2,312,31)
	print(a)
	
####类型转换
	int('123')
	int(12.3)
	float('12.43')
	bool('1')
	str('192.2')  
  
#####练习
请利用Python内置的`hex()`函数把一个整数转换成十六进制表示的字符串：  

	n1 = 255
	n2 = 1000
	
	s1 = str(hex(n1))
	s2 = str(hex(n2))
	
	print('%s,%s'%(s1,s2))  

###8.2定义函数
####定义一个取绝对值的函数
	def my_abs(x):
	    if x > 0:
	        return x
	    else:
	        return -x
	    
###引用别的.py中的函数
	from fileName import my_abs
	用于导入fileName中的my_abs()函数（不含.py扩展名）

####空函数
	def nop():
    pass  
`pass`语句什么都不做，那有什么用？实际上`pass`可以用来作为占位符，比如现在还没想好怎么写函数的代码，就可以先放一个`pass`，让代码能运行起来。  

####参数检查  
假如x的类型不匹配为int或者float`isinstance(x,type)`,则报错`raise TypeError()`

	def my_abs(x):
    if not isinstance(x ,(int,float)):
        raise TypeError('bad type')
    if x > 0:
        return x
    else:
        return -x

####返回多个值  
返回值是一个tuple！但是，在语法上，返回一个tuple可以省略括号，而多个变量可以同时接收一个tuple，按位置赋给对应的值，所以，Python的函数返回多值其实就是返回一个tuple，但写起来更方便。

	#导入一个数学函数库
	import math
	
	def move(x,y,step,angle=0):
	    nx = x + step * math.cos(angle)
	    ny = y - step * math.sin(angle)
	    return nx,ny
	    
	#可以这样赋值
	x,y = move(100,100,60,math.pi/6)
	
	print(x,y)
	
#### 练习

[韦达定理](http://baike.baidu.com/link?url=rXjlSHM4q9gMCLOJqz7GDhXt-SWwSwiWZ0epQ6DK4uZk2yfTH01aSwWyuWVrx6mWZ3o6fU4uiydPHCBY1_LIEq)

	#
	# 请定义一个函数quadratic(a, b, c)，接收3个参数，返回一元二次方程：
	#
	# ax^2 + bx + c = 0
	#
	# 的两个解。
	#
	# 提示：计算平方根可以调用math.sqrt()函数：
	
	import math
	
	def quadratic(a,b,c):
	    if a == 0:
	        if b != 0:
	            x = - (c / b)
	            return x
	        else:
	            return '无解'
	    else:
	        derta = b*b - 4*a*c
	        print(derta)
	        if derta > 0:
	            x1 = (-b + math.sqrt(factor)) / (2*a)
	            x2 = (-b - math.sqrt(factor)) / (2*a)
	            return x1,x2
	        elif derta == 0:
	            x = -(b / 2*a)
	            return x
	        else:
	            return '无解'
	
	print(quadratic(9,5,1))  
  
###8.3函数的参数
####位置参数
如果我们要计算x3怎么办？可以再定义一个power3函数，但是如果要计算x4、x5……怎么办？我们不可能定义无限多个函数。

	def power(x, n):
    s = 1
    while n > 0:
        n = n - 1
        s = s * x
    return s  
  
####默认参数  
新的power(x, n)函数定义没有问题，但是，旧的调用代码失败了，原因是我们增加了一个参数，导致旧的代码因为缺少一个参数而无法正常调用：  

	>>> power(5)
	Traceback (most recent call last):
	  File "<stdin>", line 1, in <module>
	TypeError: power() missing 1 required positional argument: 'n'  
	
这个时候，默认参数就排上用场了。由于我们经常计算x2，所以，完全可以把第二个参数n的默认值设定为2：  
	
	#默认n = 2,在调用的时候n不填,即为平方
	def power(x, n=2):
	    s = 1
	    while n > 0:
	        n = n - 1
	        s = s * x
	    return s  
  
#####默认参数的坑
先定义一个函数，传入一个list，添加一个END再返回：
	
	def add_end(L=[]):
	    L.append('END')
	    return L
当你正常调用时，结果似乎不错：
	
	>>> add_end([1, 2, 3])
	[1, 2, 3, 'END']
	>>> add_end(['x', 'y', 'z'])
	['x', 'y', 'z', 'END']
当你使用默认参数调用时，一开始结果也是对的：
	
	>>> add_end()
	['END']
但是，再次调用add_end()时，结果就不对了：
	
	>>> add_end()
	['END', 'END']
	>>> add_end()
	['END', 'END', 'END']
很多初学者很疑惑，默认参数是[]，但是函数似乎每次都“记住了”上次添加了'END'后的list。

原因解释如下：

Python函数在定义的时候，默认参数L的值就被计算出来了，即[]，因为默认参数L也是一个变量，它指向对象[]，每次调用该函数，如果改变了L的内容，则下次调用时，默认参数的内容就变了，不再是函数定义时的[]了。

所以，定义默认参数要牢记一点：默认参数`必须指向不变对象！`

要修改上面的例子，我们可以用`None`这个不变对象来实现：

	def add_end(L=None):
	    if L is None:
	        L = []
	    L.append('END')
	    return L
现在，无论调用多少次，都不会有问题：
	
	>>> add_end()
	['END']
	>>> add_end()
	['END']  
  
####可变参数  
在Python函数中，还可以定义可变参数。顾名思义，可变参数就是传入的参数个数是可变的，可以是1个、2个到任意个，还可以是0个。

我们以数学题为例子，给定一组数字a，b，c……，请计算a2 + b2 + c2 + ……。

要定义出这个函数，我们必须确定输入的参数。由于参数个数不确定，我们首先想到可以把a，b，c……作为一个list或tuple传进来，这样，函数可以定义如下：  

	def calc(numbers):
    sum = 0
    for n in numbers:
        sum = sum + n * n
    return sum  
  
但是调用的时候，需要先组装出一个list或tuple：  

	>>> calc([1, 2, 3])
	14
	>>> calc((1, 3, 5, 7))
	84  
  
所以，我们把函数的参数改为可变参数：  

	def calc(*numbers):
	    sum = 0
	    for n in numbers:
	        sum = sum + n * n
	    return sum  
定义可变参数和定义一个list或tuple参数相比，仅仅在参数前面加了一个*号。在函数内部，参数numbers接收到的是一个tuple，因此，函数代码完全不变。但是，调用该函数时，可以传入任意个参数，包括0个参数：   

	>>> calc(1, 2)
	5
	>>> calc()
	0  
  
#####Python允许你在list或tuple前面加一个*号，把list或tuple的元素变成可变参数传进去：  
	
	>>> nums = [1, 2, 3]
	>>> calc(*nums)
	14
	
#####`*`nums表示把nums这个list的所有元素作为可变参数传进去。这种写法相当有用，而且很常见。

####关键字参数
	
可变参数允许你传入0个或任意个参数，这些可变参数在函数调用时`自动组装为一个tuple`。而关键字参数允许你传入0个或任意个含参数名的参数，这些关键字参数在函数内部自动组装为一个dict。请看示例：  

	def person(name, age, **kw):
    	print('name:', name, 'age:', age, 'other:', kw)  
  
函数person除了必选参数name和age外，还接受关键字参数kw。在调用该函数时，可以只传入必选参数：

	>>> person('Michael', 30)
	name: Michael age: 30 other: {}  
  
也可以传入任意个数的关键字参数：

	>>> person('Bob', 35, city='Beijing')
	name: Bob age: 35 other: {'city': 'Beijing'}
	>>> person('Adam', 45, gender='M', job='Engineer')
	name: Adam age: 45 other: {'gender': 'M', 'job': 'Engineer'}  

和可变参数类似，也可以先组装出一个dict，然后，把该dict转换为关键字参数传进去：

	>>> extra = {'city': 'Beijing', 'job': 'Engineer'}
	>>> person('Jack', 24, city=extra['city'], job=extra['job'])
	name: Jack age: 24 other: {'city': 'Beijing', 'job': 'Engineer'}
#####当然，上面复杂的调用可以用简化的写法：
	
	>>> extra = {'city': 'Beijing', 'job': 'Engineer'}
	>>> person('Jack', 24, **extra)
	name: Jack age: 24 other: {'city': 'Beijing', 'job': 'Engineer'}  
 
`**extra`表示把extra这个dict的所有key-value用关键字参数传入到函数的**kw参数，kw将获得一个dict，注意kw获得的dict是extra的`一份拷贝`，对kw的改动不会影响到函数外的extra。 

####命名关键字参数  
  
要限制关键字参数的名字，就可以用命名关键字参数，例如，只接收city和job作为关键字参数。这种方式定义的函数如下：  

	def person(name, age, *, city, job):
    print(name, age, city, job)  
  
和关键字参数`**kw`不同，命名关键字参数需要一个特殊分隔符`*,`*后面的参数被视为命名关键字参数。

调用方式如下：

	>>> person('Jack', 24, city='Beijing', job='Engineer')
	Jack 24 Beijing Engineer  
  
####参数组合  
在Python中定义函数，可以用必选参数、默认参数、可变参数、关键字参数和命名关键字参数，这5种参数都可以组合使用。但是请注意，参数定义的顺序必须是：必选参数、默认参数、可变参数、命名关键字参数和关键字参数。  
  
