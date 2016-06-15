#iOS transform-变形,使用方法

##位移
	//tx是x轴位移， ty是y轴位移
	//使用带有Make的方法，只会移动一次50，50
	self.imageView.transform = CGAffineTransformMakeTranslation(50, 50);
	
	//使用不带有make的方法，每次点击都会移动
	self.imageView.transform = CGAffineTransformTranslate(self.imageView.transform, 50, 50);
	    
##缩放
	//sx x轴放大倍数, sy y轴的放大倍数
	self.imageView.transform = CGAffineTransformMakeScale(1.5, 1.5);
	
    self.imageView.transform = CGAffineTransformScale(self.imageView.transform, 1.5, 1.5);

##旋转
    self.imageView.transform = CGAffineTransformMakeRotation(M_PI_4);
    
    self.imageView.transform = CGAffineTransformRotate(self.imageView.transform, M_PI_4);

##还原
    self.imageView.transform = CGAffineTransformIdentity;
