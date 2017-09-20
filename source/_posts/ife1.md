---
title: 2016百度IFE前端技术学院Task01-07
categories: IFE
tags:   
 - IFE
---
### [任务要求](https://github.com/leeween/lfe/tree/master/task001/mission07)  
### img底部留白以及解决办法  
如图![img底部留白](/imgs/03-15a.png)
可以看到img与div之间有留白。    
解决办法：  
1.给div赋值高度，强行让其没有留白；  
2.给div设置`div {line-height: 0;}`或者`div {font-size: 0;}`同样可以解决问题；  
3.给img设置`vertical-align: middle`属性。  
每种方法各有利弊，需要在实际中自行选择。  
产生原因以及更多精彩分析见张鑫旭大神的博客[CSS深入理解vertical-align和line-height的基友关系](http://www.zhangxinxu.com/wordpress/2015/08/css-deep-understand-vertical-align-and-line-height/)   
`input`紧邻的`button`在相同高度下也会出现`button`比`input`高1px的情况，同样使用`vertical-align: middle`可以解决。
### 使用css3 `:not`选择器与父级元素`margin`负值的选择  
经常碰到如图所示情形![选择](/imgs/03-15b.png)  
即在同一行中相同元素两边有空隙或者竖线，而两边是没有的。当然此图可以设置`margin-left`。在不考虑ie9以下浏览器时应该使用css3中的`:not`选择器，而需要兼容低级IE浏览器时只需包裹一层父元素，令父元素的`margin`为负值。  
### textarea相关
`textarea`不继承`body`的字体属性，需要自行设定，(⊙﹏⊙)以前真不知道；  
禁止`textarea`缩放，只需添加`textarea {resize: none;}`。
### 题外话  
第一次使用[fecs](http://fecs.baidu.com/)检查代码，吓死了，还好全是缩进的问题。不过对于代码的整洁以及风格比较有帮助。