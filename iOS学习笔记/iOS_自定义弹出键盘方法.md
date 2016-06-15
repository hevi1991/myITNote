#自定义弹出的键盘    
###随便定义一个textfield看效果.
	UITextField *tf = [[UITextField alloc]initWithFrame:CGRectMake(0, 0, 100, 30)];
    tf.backgroundColor = [UIColor groupTableViewBackgroundColor];
    tf.center = self.view.center;
    [self.view addSubview:tf];
    
###inputView只看高度

    UIView *aView = [[UIView alloc]initWithFrame:CGRectMake(0, 0, 0, 250)];
    aView.backgroundColor = [UIColor redColor];
    tf.inputView = aView;
    //tf.inputAccessoryView = aView;//这个也可以  
  
###API说明
// Presented when object becomes first responder.  If set to nil, reverts to following responder chain.   

//  If set while first responder, will not take effect until reloadInputViews is called.

	@property (nullable, readwrite, strong) UIView *inputView;             
	@property (nullable, readwrite, strong) UIView *inputAccessoryView;