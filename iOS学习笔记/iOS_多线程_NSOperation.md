#NSOperation，多线程操作学习
>NSOperation是苹果在GCD为基础上再抽象的一个多线API，最大特点可以设置线程并发数，各任务之间的依赖，在面对大型操作中为了保障GPU性能稳定，常用。


##流程
	自定义队列->定义任务->队列添加任务--(设置好依赖，并发数等等)->自动执行  
---

###自定义队列--（均为并发队列）

    NSOperationQueue *queue = [[NSOperationQueue alloc]init];
    
###设置最大并发数

    [queue setMaxConcurrentOperationCount:2];
    
###创建操作(NSBlockOperation或NSInvocationOperation)
    
    NSBlockOperation *blockOp1 = [NSBlockOperation blockOperationWithBlock:^{
        NSLog(@"hello1,%@",[NSThread currentThread]);
    }];
    NSBlockOperation *blockOp2 = [NSBlockOperation blockOperationWithBlock:^{
        NSLog(@"hello2,%@",[NSThread currentThread]);
    }];
    NSBlockOperation *blockOp3 = [NSBlockOperation blockOperationWithBlock:^{
        NSLog(@"hello3,%@",[NSThread currentThread]);
    }];
    
###添加依赖，必须被依赖的操作完成后再执行依赖的操作
    [blockOp3 addDependency:blockOp1];
    [blockOp2 addDependency:blockOp3];
    
###在队列中添加操作
    [queue addOperation:blockOp1];
    [queue addOperation:blockOp2];
    [queue addOperation:blockOp3];
    
###设置主线程操作
    NSBlockOperation *blockOp4 = [NSBlockOperation blockOperationWithBlock:^{
        NSLog(@"刷新UI,%@",[NSThread currentThread]);
    }];
    [blockOp4 addDependency:blockOp2];
    [[NSOperationQueue mainQueue] addOperation:blockOp4];