---
title: Relative与Absolute组合使用
date: 2018-05-20
tags: 
- HTML
- CSS
categories:
- HTML/CSS
---
使用position:absolute可以实现被设置元素==相对于浏览器(body)== 设置定位以后，大家有没有想过可不可以相对于其它元素进行定位呢？答案是肯定的，当然可以。使用position:relative来帮忙，但是必须遵守下面规范：

1、参照定位的元素必须是相对定位元素的前辈元素：

    <div id="box1"><!--参照定位的元素-->
        <div id="box2">相对参照元素进行定位</div><!--相对定位元素-->
    </div>

从上面代码可以看出box1是box2的父元素（父元素当然也是前辈元素了）。

2、参照定位的元素必须加入position:relative;

    #box1{
        width:200px;
        height:200px;
        position:relative;        
    }

3、定位元素加入position:absolute，便可以使用top、bottom、left、right来进行偏移定位了。

    #box2{
        position:absolute;
        top:20px;
        left:30px;         
    }

这样box2就可以相对于父元素box1定位了（这里注意参照物就可以不是浏览器了，而可以自由设置了）。