# Draw_in_fanned_1
绘制扇形

原理：1.使用贝塞尔曲线决定绘制圆的路径和角度； 
      2.用CAShapeLayer来重合贝塞尔曲线的路径; 
      3.用基础动画来实现绘制的动态性。


1.使用贝塞尔曲线决定绘制圆的路径和角度

      //用贝塞尔曲线来画扇形，实际上画的是圆，因为半径为边线中间到原点的距离，所以设置半径为线宽的一半，
      这样线宽就覆盖了中间填充部分的颜色，形成扇形。
      UIBezierPath *bezierPath=[UIBezierPath bezierPathWithArcCenter:CGPointMake((self.view.frame.size.width)/2, (self.view.frame.size.height)/2) radius:60 startAngle:0 endAngle:M_PI/3*4 clockwise:YES];


2.用CAShapeLayer来重合贝塞尔曲线的路径

        CAShapeLayer *shanxingLayer = [CAShapeLayer layer];
        //这里设置填充线的宽度
        shanxingLayer.lineWidth = 120;
        //设置拐角样式
        shanxingLayer.lineCap = kCALineCapButt;
        //绘制的线的颜色
        shanxingLayer.strokeColor = [[UIColor redColor] CGColor];
        //填充色设置为nil是为了防止填充色混淆，因为这个本身实际上是画圆，第一步有解释
        shanxingLayer.fillColor = nil;
        //路径
        shanxingLayer.path = bezierPath.CGPath;   
        [self.view.layer addSublayer:shanxingLayer];
        

3.用基础动画来实现绘制的动态性

        CABasicAnimation *animation = [CABasicAnimation animationWithKeyPath:@"strokeEnd"];
        //设置绘制动画持续的时间
        animation.duration = 1;
        //速度控制函数
        animation.timingFunction = [CAMediaTimingFunction functionWithName:kCAMediaTimingFunctionEaseOut];
        //起始值
        animation.fromValue = [NSNumber numberWithFloat:0.0f];
        //结束值
        animation.toValue = [NSNumber numberWithFloat:1.0f];
        //是否翻转绘制
        animation.autoreverses = NO;
        //当前对象在非active时间段的行为
        animation.fillMode = kCAFillModeForwards;
        //重复次数
        animation.repeatCount = 1;
        [shanxingLayer addAnimation:animation forKey:@"strokeEnd"];
        //表示绘制到百分比多少就停止，这个我们用1表示完全绘制
        shanxingLayer.strokeEnd=1.0f;
