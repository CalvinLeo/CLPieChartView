# CLPieChart

#### 背景
>需求：需要做一个环形图用于展示各部分之间的比例

<div align=center><img width= "371" height= "262" src="https://github.com/CalvinLeo/CLPieChart/blob/master/images/sample_image_1.png"/></div>

<div align=center>示意图</div>

#### 实现方式

##### 技术选型：


1、在UIView的drawRect方法中绘制所需要的各种视图，饼图，指示线，文字等元素绘制出页面

2、基于UIBezierPath + CAShapeLayer，UIView，UILabel等控件搭建页面

两种方式都可以实现想要的效果，相比较而言，个人觉得第二种方式更容易一些，因为不需要手动绘图，基于UI控件的熟悉可以更好地完成。

##### 实现细节：

1、圆环的实现，

* 其实这里圆环是两个视图嵌套的结果，中间的白色视图其实是在饼状图的上面，同时还有一个文本输入框可以输入文字，只不过我们的需求里没有，这里就隐藏了文本框。

相关的代码：

```
    CGFloat rate = 0.2;
    CGFloat start = -M_PI_2;
    CGFloat angle = rate * M_PI * 2;
    CGFloat end = start + angle;
    UIBezierPath *path = [UIBezierPath bezierPathWithArcCenter:CGPointMake(self.bounds.size.width * 0.5, self.bounds.size.height * 0.5)
                                                        radius:50
                                                    startAngle:start
                                                      endAngle:end
                                                     clockwise:YES];
    [path addLineToPoint:CGPointMake(self.bounds.size.width * 0.5, self.bounds.size.height * 0.5)];
    [path closePath];
    
    CAShapeLayer *slayer = [CAShapeLayer layer];
    slayer.path = path.CGPath;
    slayer.fillColor = [UIColor redColor].CGColor;
    slayer.strokeColor = [UIColor redColor].CGColor;
    slayer.lineCap = kCALineCapButt;
    [self.layer addSublayer:slayer];
```
解释一下这部分代码：

1）rate：就是扇形区域所占比
2）start：弧度起始点，end：弧度结束点 angle：角度，这里面的角度和弧度的相关概念介绍及转化就不介绍了
3） bezierPathWithArcCenter:radius:startAngle:endAngle:clockwise:这个API会根据中心点center、半径radius、起始点startAngle、结束点endAngle、是否是顺时针clockwise这五个属性绘制出一条圆弧。
4）然后将path路径闭合成一块区域，就变成了扇形区域。

<div align=center><img width="375" height="667" src="https://github.com/CalvinLeo/CLPieChart/blob/master/images/sample_image_2.png"/></div>
<div align=center>扇形图</div>


由上图可以看出红色扇形就是占比为0.2的一个扇形区域。其实我们最后做出来的效果就是由一块一块的扇形首尾拼接起来的，所以我们需要处理的就是其他path的startAngle、endAngle参数

* 完成示例图中的设计需要两个扇形区域，一个比例为0.6另一个为0.4：

<div align=center><img width="375" height="375" src="https://github.com/CalvinLeo/CLPieChart/blob/master/images/sample_image_3.png"/></div>
<div align=center>饼图</div>


2、添加中间区域的UIView，让其看似是一个圆环

中间视图其实就是一个UIView，然后设置layer.cornerRadius是宽，高的一半，再clipsToBounds 就可以实现。如果需要展示什么标题，可以加入Label。

<div align=center><img width="375" height="375" src="https://github.com/CalvinLeo/CLPieChart/blob/master/images/sample_image_4.png"/></div>
<div align=center>环形图</div>

3、





