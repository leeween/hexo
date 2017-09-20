---
title: input的border-color属性
categories: css
tags: [css]
---
在做freecodecamp项目时，为了美观改变input的边框颜色，为了偷懒就直接写了一句

	border-color: red;
![input1](/imgs/0619input/input1.png)
发现颜色有点不对，上边框和左边框颜色更为深一点，作为一个新时代的切图仔，这当然不能容忍。想到是不是`box-shadow`的问题，于是，将边框的宽度设置为：

	border-width: 20px;
![input2](/imgs/0619input/input2.png)
很显然，`box-shadow`直接排除掉了。百般思索，不得其解，然后网上搜索，似乎没有人遇到跟我一样的问题。  
然后思索着，如果单独设置每个边框的`border-color`会不会好
![input3](/imgs/0619input/input3.png)
结果发现还是这样子。最后决定打破一下当前思路，想一下以前是怎样设置input的边框的
![input4](/imgs/0619input/input4.png)
似乎，有些不同！在这里面有设置了border的样式类型为solid，再次测试
![input5](/imgs/0619input/input5.png)
果然，是因为浏览器默认的`border-style: inset;`的原因。一个`boder-style`竟然有如此之多的属性，需要学习和实践的东西还很多！
