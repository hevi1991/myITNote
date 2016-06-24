#单例模式

>单例模式是一种常用的软件设计模式,通过单例模式可以保证系统中一个类只有一个实例而且该实例易于外 界访问,从而方便对实例个数的控制并节约系统资源。 如果希望系统中某个类的对象只能存在一个,单例模式是最好的解决方案,iOS 中最常见的单例就是 UIApplication 。

---

##第一步:重写 allocWithZone 方法allocWithZone 方法是对象分配内存空间时,最终会调用的方法,重写该方法,保证只会分配一个内存空间 
	+ (id)allocWithZone:(struct _NSZone *)zone{	static Ticket *instance;	static dispatch_once_t onceToken;//用来检测是否只被执行一次	//dispatch_once 是线程安全的,能够做到在多线程的环境下 Block 中的代码只会被执行一次 	dispatch_once(&onceToken, ^{		instance = [super allocWithZone:zone];		});	return instance; 	}
##第二步:建立 sharedXXX 类方法,便于其他类访问 

	+ (instancetype)sharedTicket{		return [[self alloc] init]; 	}