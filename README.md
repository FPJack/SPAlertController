
# SPAlertController
[![Build Status](http://img.shields.io/travis/SPStore/SPAlertController.svg?style=flat)](https://travis-ci.org/SPStore/SPAlertController)
[![Pod Version](http://img.shields.io/cocoapods/v/SPAlertController.svg?style=flat)](http://cocoadocs.org/docsets/SPAlertController/)
[![Pod Platform](http://img.shields.io/cocoapods/p/SPAlertController.svg?style=flat)](http://cocoadocs.org/docsets/SPAlertController/)
[![Pod License](http://img.shields.io/cocoapods/l/SPAlertController.svg?style=flat)](https://www.apache.org/licenses/LICENSE-2.0.html)
[![Carthage compatible](https://img.shields.io/badge/Carthage-compatible-4BC51D.svg?style=flat)](https://github.com/SPStore/SPAlertController)
[![codecov](https://codecov.io/gh/SPStore/SPAlertController/branch/master/graph/badge.svg)](https://codecov.io/gh/SPStore/SPAlertController)
# 目录
* [如何导入](#如何导入) 
* [如何使用](#如何使用)
* [效果图](#效果图) 

## 如何导入
##### 版本1.0.1
```
platform:ios,'8.0'
target 'MyApp' do
  pod 'SPAlertController', '~> 1.0.1'
end
```
##### 版本1.7.0
```
platform:ios,'8.0'
target 'MyApp' do
  pod 'SPAlertController', '~> 1.7.0'
end
```
##### 版本2.0
```
platform:ios,'8.0'
target 'MyApp' do
  pod 'SPAlertController', '~> 2.0'
end
```
然后在终端输入命令:pod install 

## 如何使用
* 第一步：创建SPAlertController
```
SPAlertController *alertController = [SPAlertController alertControllerWithTitle:@"这是大标题" message:@"这是小标题" preferredStyle:SPAlertControllerStyleActionSheet animationType:SPAlertAnimationTypeDefault];

我想对你说:
preferredStyle是提醒对话框的弹出样式，SPAlertControllerStyleActionSheet是从底部或者顶部弹出（顶部还是底部取决于animationType），SPAlertControllerStyleAlert从中间弹出，animationType是动画类型，有从底部往上弹出动画，从顶部往下弹出动画，从中间渐变弹出动画，缩放弹出动画等

```
* 第二步：创建action
```
SPAlertAction *actionOK = [SPAlertAction actionWithTitle:@"OK" style:SPAlertActionStyleDefault handler:^(SPAlertAction * _Nonnull action) {
        
    }];
    
我想对你说:
方法参数中的style是action的样式，这里跟系统的一致，共有SPAlertActionStyleDefault、SPAlertActionStyleCancel(取消)、SPAlertActionStyleDestructive(默认红色)这3种样式，跟系统不一样的是，SPAlertController可以自定义action的相关属性，如文本颜色、字体等;
block块:当点击action的时候回调
```
* 第三步：添加action
```
[alertController addAction:actionOK];
```
* 第四步：modal出alertController
```
[self presentViewController:alertController animated:YES completion:nil];
```
*以上这就是最基本的四步操作，当然你可以中间再设置alertController的属性或者action的属性，至于具体哪些属性干什么，示例程序中有非常详细的注释.*

### 还可以做什么?
* 添加文本输入框
```
[alertController addTextFieldWithConfigurationHandler:^(UITextField * _Nonnull textField) {
        // 这个block只会回调一次，因此可以在这里自由定制textFiled，如设置textField的相关属性，设置代理，添加addTarget，监听通知等
}];
```
* 自定义整个弹出视图
```
MyView *myView = [MyView shareMyView];
    
SPAlertController *alertController = [SPAlertController alertControllerWithTitle:@"这是大标题" message:@"这是小标题" preferredStyle:SPAlertControllerStyleAlert animationType:SPAlertAnimationTypeAlpha customView:myView];
[self presentViewController:alertController animated:YES completion:nil];
    
我想对你说:
自定义整个弹出视图时,添加action或textField没有任何作用，因为已经自定义了整个视图，自带的内部布局将不起作用
```
* 自定义titleView
```
MyTitleView *myTitleView = [[MyTitleView alloc] initWithFrame:CGRectMake(0, 0, 0, 200)];
SPAlertController *alertController = [SPAlertController alertControllerWithPreferredStyle:SPAlertControllerStyleAlert animationType:SPAlertAnimationTypeDefault customTitleView:myTitleView];
SPAlertAction *action1 = [SPAlertAction actionWithTitle:@"第1个" style:SPAlertActionStyleDefault handler:^(SPAlertAction * _Nonnull action) {
     NSLog(@"点击了第1个");
}];
    
SPAlertAction *action2 = [SPAlertAction actionWithTitle:@"第2个" style:SPAlertActionStyleDestructive handler:^(SPAlertAction * _Nonnull action) {
    NSLog(@"点击了第2个");
}];
[alertController addAction:action1];
[alertController addAction:action2];
[self presentViewController:alertController animated:YES completion:nil];

我想对你说:
为什么要自定义titleView？有这样的的需求吗？答案是肯定的，因为SPAlertController自带的titleView只能显示文本且居中，如果想往里面添加图片则自带的就不管用了，这时便可以在外界自定义好一个titlView，创建时当参数传进去，SPAlertController便会帮你显示在对话框的顶部
```
* 自定义centerView
```
MyCenterView *centerView = [[MyCenterView alloc] initWithFrame:CGRectMake(0, 0, 0, 200)];
    
SPAlertController *alertController = [SPAlertController alertControllerWithTitle:@"这是大标题" message:@"这是小标题" preferredStyle:SPAlertControllerStyleAlert animationType:SPAlertAnimationTypeDefault customCenterView:centerView];

SPAlertAction *action1 = [SPAlertAction actionWithTitle:@"第1个" style:SPAlertActionStyleDefault handler:^(SPAlertAction * _Nonnull action) {
    NSLog(@"点击了第1个");
}];
// 设置第1个action的颜色
action1.titleColor = [UIColor blueColor];

// SPAlertActionStyleDestructive默认文字为红色(可修改)
SPAlertAction *action2 = [SPAlertAction actionWithTitle:@"第2个" style:SPAlertActionStyleDestructive handler:^(SPAlertAction * _Nonnull action) {
    NSLog(@"点击了第2个");
}];
// 设置第2个action的颜色
action2.titleColor = [UIColor redColor];
[alertController addAction:action1];
[alertController addAction:action2];
[self presentViewController:alertController animated:YES completion:nil];

我想对你说:
有时看见过这样的对话框，顶部是一个title，最底部有2个action，中间是一个tableView；这里自定义centerView就是解决这个需求。
当自定义centerView时，如果是SPAlertControllerStyleAlert样式，action的个数最多只能是2个，超过2个将不显示，只显示最前面2个添加的；如果是SPAlertControllerStyleActionSheet样式，只有取消（SPAlertActionStyleCancel）样式才会显示，其余样式的action均不会显示
```
*当你自定义view时，无论是customView、customTitleView还是customCenterView，外界对这些自定义的view设置的x、y、宽均无效，只有高度有效，SPAlertController内部会自动修改x、y、宽，所以，当你自定义这些view时，这些view的子控件的布局尽量写在layoutSubViews里，或者用autoLayout，这样子控件才能适应SPAlertController所做的改变。如果你想改变这些自定义view的宽度，或者垂直位置，你可以设置相关边距(margin)，SPAlertController提供了几个边距属性和offsetY，offsetY指的是垂直中心的偏移*

## 效果图
![image](https://github.com/SPStore/SPAlertController/blob/master/PreImages/F6C0259AFBAD7E5F651CB1FD41796DEF.jpg)....................![image](https://github.com/SPStore/SPAlertController/blob/master/PreImages/06A97B9DBDE3F07D2207BAA2085D25C6.jpg)  

![image](https://github.com/SPStore/SPAlertController/blob/master/PreImages/A76E51AC536052790CD80C184E803432.jpg)....................![image](https://github.com/SPStore/SPAlertController/blob/master/PreImages/9C6F94D5ECF90CE6A94D90D507DB18EC.jpg)  

![image](https://github.com/SPStore/SPAlertController/blob/master/PreImages/4DB2CAFA218FEE08E36578C94F2A5B71.jpg)....................![image](https://github.com/SPStore/SPAlertController/blob/master/PreImages/03A721F9F6A4F39346134F7EEE49FA2E.jpg)

![image](https://github.com/SPStore/SPAlertController/blob/master/PreImages/3DB1CF20C14DFE9103B827F06BE5ACE5.jpg)....................![image](https://github.com/SPStore/SPAlertController/blob/master/PreImages/1A20B204D250B3DBFE973A4EC0C5209F.jpg)  

![image](https://github.com/SPStore/SPAlertController/blob/master/PreImages/57DDD7273486D292452471FAFDDC9F18.jpg)....................![image](https://github.com/SPStore/SPAlertController/blob/master/PreImages/CDD1F2ADE694932980AB1509921FB628.jpg) 

![image](https://github.com/SPStore/SPAlertController/blob/master/PreImages/EAEFB2EAA7932E456E7D85238E2C73C7.jpg)....................![image](https://github.com/SPStore/SPAlertController/blob/master/PreImages/2DB32E8A0446B214C5EDC97998B127BA.jpg) 

![image](https://github.com/SPStore/SPAlertController/blob/master/PreImages/19E17ECCD0C1A8CE7D0499CD8AF06A2F.jpg)....................![image](https://github.com/SPStore/SPAlertController/blob/master/PreImages/30D02B422FC9CA27F5CBA37FF85BADE6.jpg)  

![image](https://github.com/SPStore/SPAlertController/blob/master/PreImages/36186FEFDD89D2356EFEF140093A28A7.jpg)....................![image](https://github.com/SPStore/SPAlertController/blob/master/PreImages/3E8019BA57F447785BD2561ECF95D234.jpg)

![image](https://github.com/SPStore/SPAlertController/blob/master/PreImages/91B2F76B900C0EA9B5E34B8AE64CAB36.jpg)....................![image](https://github.com/SPStore/SPAlertController/blob/master/PreImages/1776B8F36B3C3051C2C2FE3AE12D843F.jpg)

![image](https://github.com/SPStore/SPAlertController/blob/master/PreImages/1FE9B512B50D7E139B30E0BDB5B3FF6E.jpg)....................![image](https://github.com/SPStore/SPAlertController/blob/master/PreImages/86C4035CB7097B99FA89706E3668055E.jpg)



[回到顶部](#目录) 
