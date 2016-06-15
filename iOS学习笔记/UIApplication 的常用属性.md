#UIApplication 的常用功能  
##1,设置应用程序图标右上角的红色提醒数字
	@property(nonatomic) NSInteger applicationIconBadgeNumber;  
##2,设置联网指示器的可见性	@property(nonatomic,getter=isNetworkActivityIndicatorVisible) BOOL networkActivityIndicatorVisible;
##3,openURL  
###UIApplication 有个功能十分强大的 openURL:方法
	- (BOOL)openURL:(NSURL*)url;###openURL:方法的部分功能:  
(1)打电话
	UIApplication *app = [UIApplication sharedApplication];	[app openURL:[NSURL URLWithString:@"tel://10086"]];	(2)发短信
	[app openURL:[NSURL URLWithString:@"sms://10086"]];	 
(3)发邮件
	[app openURL:[NSURL URLWithString:@"mailto://12345@qq.com"]]; 
(4)打开一个网页资源(自动跳转到浏览器打开)
	[app openURL:[NSURL URLWithString:@"http://ios.itcast.cn"]];
(5)打开其他 app 程序
[跳转OSChina查看详情](http://my.oschina.net/u/1440723/blog/361939)
