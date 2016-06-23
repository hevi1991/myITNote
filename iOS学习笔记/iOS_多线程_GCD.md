#GCD学习
>GCD是苹果为开发者写的多线程开发API，原理是利用C语言底层写的
  

##1.队列：
主队列(主线程队列):  

	dispatch_get_main_queue()	
全局队列(快速获得并发子线程队列):  

	dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0)  
自定义队列(自定义队列的属性):  
	
	dispatch_queue_t queue = dispatch_queue_create("label", dispatch_queue_attr_t attr)
	dispatch_queue_attr_t attr->填写队列的类型：
	1.DISPATCH_QUEUE_SERIAL:串行队列，单个子线程顺序执行任务
	2.DISPATCH_QUEUE_CONCURRENT:并发队列，开辟多个子线程执行任务
	  
根据实际情况选择队列。

##执行

同步执行：就如同在主线程操作，没什么意义不记录

异步执行的方法（根据队列，异步同时执行任务，不需要排队）：

	dispatch_async(dispatch_queue_t queue, ^{
        
    });