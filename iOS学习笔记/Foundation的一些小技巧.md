#Foundation的一些小技巧
	
####取得以下结构体的字符串
	NSStringFromRect
	NSStringFromSize
	NSStringFromRange
	NSStringFromPoint
	...
###合并字符串数组	NSString *str = [arrs2 componentsJoinedByString:@"|"]; NSLog(@"%@",str); //obj|obj1|obj2|obj4|obj5
###字符串切割成数组	NSArray *arr3=[str componentsSeparatedByString:@"|"]; NSLog(@"%@",arr3);
###数组排序	array = [array sortedArrayUsingSelector:@selector(compare:)];
###如果要添加空的话可以用以下方法 
	[arrs addObject:[NSNull null]];	  
  
###NSSet 用法:####set 集合:	NSSet * set= [[NSSet alloc] initWithObjects:@"one",@"two",@"three",@"four", nil];#####set2 集合:	NSSet * set2 = [[NSSet alloc] initWithObjects: @"one",@"two",@"three",@"four", nil];#####(1)返回集合中对象的个数 
	[set count];#####(2)判断集合中是否拥有某个元素 判断集合中是否拥有@”two”:	BOOL ret = [set containsObject:@"two"];
#####(3)判断两个集合是否相等	BOOL ret = [set isEqualToSet:set2];#####(4)判断 set 是否是 set2 的子集合	BOOL ret = [set isSubsetOfSet:set2];#####(5)集合也可以用枚举器来遍历	NSEnumerator * enumerator = [set objectEnumerator]; NSString *str;	while(str =[enumerator nextObject])	{	......	}#####(6)数组转换为集合	NSArray * array = [[NSArray alloc] initWithObjects: @"one",@"two",@"three",@"four", nil];	NSSet * set= [[NSSet alloc] initWithArray:array];#####(7)集合转换为数组	NSArray * array2 = [set allObjects];####3、可变集合 NSMutableSet NSMutableSet 用法:NSMutableSet * set=[[NSMutableSet alloc] init];#####(1)添加元素 [setaddObject:@"one"];	[set addObject:@"two"];	[set addObject:@"two"];#####(2)如果添加的元素有重复,实际只保留一个删除元素#####(3)删除元素 ######(a)删除指定元素:	[set removeObject:@"two"];######(b)删除所有元素:	[set removeAllObjects];#####(3)将 set2 中的元素添加到 set 中#####将 set2 中的元素添加到 set 中来,如果有重复,只保留一个
	NSSet * set2 = [[NSSet alloc] initWithObjects:@"two",@"three",@"four", nil]; 
	[set unionSet:set2];#####(4)删除 set 中与 set2 相同的元素	[set minusSet:set2];###4、指数集合(索引集合)NSIndexSet	NSIndexSet * indexSet = [[NSIndexSet alloc] initWithIndexesInRange: NSMakeRange(1, 3)];####(1)根据集合提取数组中指定位置的元素	NSArray * array = [[NSArray alloc] initWithObjects: @"one",@"two",@"three",@"four", nil];	NSArray * newArray = [array objectsAtIndexes:indexSet];	返回@"two",@"three",@"four"###5、可变指数集合 NSMutableIndexSet	NSMutableIndexSet *indexSet =[[NSMutableIndexSet alloc] init]; [indexSet addIndex:0]	[indexSet addIndex:3];	[indexSet addIndex:5];####通过集合获取数组中指定的元素	NSArray *array = [[NSArray alloc] initWithObjects: @"one",@"two",@"three",@"four",@"five",@"six", nil];	NSArray *newArray = [array objectsAtIndexes:indexSet];	返回@"one",@"four",@"six"  
  
###枚举
	通过枚举类型枚举	NSEnumerator *enumerator = [dic keyEnumerator]; 	id key = [enumerator nextObject];	while (key) {	id obj = [dic objectForKey:key]; NSLog(@"%@", obj);	key = [enumerator nextObject];	}  
  
###NSDate取本地时区时间
	NSDate *date = [NSDate date];
    NSTimeZone *zone =[NSTimeZone localTimeZone];
    NSInteger interval = [zone secondsFromGMTForDate:date];
    date = [date dateByAddingTimeInterval:interval];
    NSLog(@"date:%@",date);  
  
###NSFileManager的使用
####创建一个文件并写入内容
	BOOL isWritSuccess = [str writeToFile:@”/User/LFF/Desktop/lff.txt” atomically:YES encoding:NSUTF8StringEncoding error:&error];
####通过上面代码成功创建了一个文件,下面步入正题:#####打印文件创建日期与大小 	NSFileManager * fileManager = [NSFileManager defaultManager];//启动文件管理器	NSString * path = @"/ User/LFF/Desktop/lff.txt "; //路径	NSError * error;	NSDictionary * dic =	[fileManager attributesOfItemAtPath:path error:&error];	NSLog(@"dic %@",dic); 
	
	//取得文件创建时间	if (error == nil)	{	NSDate * date = [dic objectForKey:NSFileCreationDate]; NSString * size = [dic objectForKey:NSFileSize]; NSLog(@" date = %@, size = %@",date,size);
	}
#####(1)创建目录
	NSString * path = @"/Users/LFF/Desktop/myfolder/aaa";	NSError * error;	/*withIntermediateDirectories 参数:	YES 逐级创建文件夹	NO 表示只能够创建一级目录*/	BOOL isCreateSuccess = [fileManager createDirectoryAtPath:path withIntermediateDirectories:YES attributes:nil error:&error];  
#####(2)移动目录 移动就是剪切操作  
	BOOL isMoveSuccess = [fileManager moveItemAtPath:path toPath:pathTo error:&error];   
#####(3)删除  
	BOOL isRemoveSuccess = [fileManager removeItemAtPath:path error:&error];   
#####(4)拷贝文件  
	BOOL isCopySuccess = [fileManager copyItemAtPath:path toPath:pathTo error:&error];    
	  
###深浅拷贝  
* 在非集合类对象中:对 不可变对象进行 copy 操作,是指针复制,mutableCopy 操作时内容复制;    
* 对 mutable 对象进行 copy 和 mutableCopy 都是内容复制。
* 在集合类对象中,对 immutable 对象进行 copy,是指针复制,mutableCopy 是内容复制;对 mutable 对象进 行 copy 和 mutableCopy 都是内容复制。但是:`集合对象的内容复制仅限于对象本身,对象元素仍然是指针复制`。  


