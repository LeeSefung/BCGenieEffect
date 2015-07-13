# BCGenieEffect

####效果图:

 ![image](http://github.com/LeeSefung/BCGenieEffect/raw/master/BCGenieEffect/1.png) ![image](http://github.com/LeeSefung/BCGenieEffect/raw/master/BCGenieEffect/2.png) 
 ![image](http://github.com/LeeSefung/BCGenieEffect/raw/master/BCGenieEffect/3.png) ![image](http://github.com/LeeSefung/BCGenieEffect/raw/master/BCGenieEffect/4.png)

####来源：<https://github.com/Ciechan/BCGenieEffect.git>
####本文：<https://github.com/LeeSefung/BCGenieEffect.git>

####代码示例：

//
//  ViewController.m
//  BCGenieEffect
//
//  Created by rimi on 15/7/10.
//  Copyright (c) 2015年 LeeSefung. All rights reserved.
//  https://github.com/Ciechan/BCGenieEffect.git
//  https://github.com/LeeSefung/BCGenieEffect.git
//

#####import "ViewController.h"
#####import "UIView+Genie.h"

@interface ViewController () {
    
    UIView *_view;
    UIButton *_button;
    NSTimer *_timer;
}

@end

@implementation ViewController

-- (void)viewDidLoad {
    [super viewDidLoad];
    
    //设置背景色
    self.view.backgroundColor = [UIColor whiteColor];
    
    //创建VIEW
    _view = [[UIView alloc] initWithFrame:CGRectMake(150, 100, 200, 200)];
    _view.backgroundColor = [UIColor orangeColor];
    [self.view addSubview:_view];
    
    //创建按钮
    _button = [[UIButton alloc] initWithFrame:CGRectMake(20, 300, 20, 20)];
    _button.backgroundColor = [UIColor redColor];
    [_button addTarget:self action:@selector(genieOutTransition) forControlEvents:UIControlEventTouchUpInside];
    [self.view addSubview:_button];
    
    UIView *view2 = [[UIView alloc] initWithFrame:CGRectMake(100, 510, 20, 20)];
    view2.backgroundColor = [UIColor redColor];
    [self.view addSubview:view2];
    
    //添加计时器
    _timer = [NSTimer scheduledTimerWithTimeInterval:3.9f target:self selector:@selector(genieOutTransition) userInfo:nil repeats:YES];
}

//动画触发时必须保证动画正常结束，即动画时间小于动画执行时间间隔，如此处按钮取消交互的时间大于动画时间，计时器周期大于动画时间,以避免出现程序错误。
//离开动画<p>
-- (void)genieOutTransition {

    //在使用按钮点击触发时间时要取消按钮的交互，在动画结束后再打开按钮的交互
    //使用计时器时必须保证计时器的循环周期时间大于动画时间
    _button.userInteractionEnabled = NO;
    //设置开始时视图的位置及大小，结束时的位置为视图的原始位置，startEdge代表移动方向。
    CGRect startRect = CGRectMake(20, 300, 20, 20);
    //开始离开动画
    //打印视图中心点
    NSLog(@"%lf-%lf",_view.center.x,_view.center.y);
    [_view genieOutTransitionWithDuration:1.8f
                               startRect:startRect
                               startEdge:BCRectEdgeRight
                              completion:^{
                                  [self genieInTransition];
                                  //打印视图中心点
                                  NSLog(@"%lf-%lf",_view.center.x,_view.center.y);
                              }];
}

//进入动画<p>
-- (void)genieInTransition {
    
    //设置结束时视图的位置及大小，startEdge代表移动方向。
    CGRect endRect = CGRectMake(100, 510, 20, 20);
    //动画开始会从起始点开始。注意动画过程中视图的中心点始终不变
    [_view genieInTransitionWithDuration:1.8f
                              destinationRect:endRect
                              destinationEdge:BCRectEdgeTop
                                   completion:^{
                                       
                                       //动画结束打开按钮交互
                                       _button.userInteractionEnabled = YES;
                                       //打印视图中心点
                                       NSLog(@"%lf-%lf",_view.center.x,_view.center.y);
    }];
}

@end
