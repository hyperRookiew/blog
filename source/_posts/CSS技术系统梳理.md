---
title: 大脑Cache系列--系统地梳理下CSS3技术吧
date: 2021-10-10 19:24:12
categories: 
 - [技术]
 - [大脑Cache]
tags:
- 前端
- CSS
---
### 背景
> - 视频云的开发已经进入正常的开发流程了，对前端框架层面已经有了一定的了解，需要返回补充前端核心接触知识了。
> - 在实际的开发中，调整样式总是花了我前端开发的一大半时间，主要还是对CSS不太熟悉，有必要系统学习一下。
> - 本文依然按照以往的风格，更注重CSS技术的系统性梳理，不赘述技术细节，因为Google一下就好。

### CSS基础
##### 1.  CSS的几种引入方式
- head中直接添加文档内样式代码

<!--more-->

```
<style type="text/css">
{
    body{}
}
</style>
```
- head中引入外部样式

```
<link rel="stylesheet" type="text/css" href="common.css">
```

- 元素的style样式

```
<div style="width:200px">
```

##### 2.CSS3的几种选择器
- 简单选择器（type，#id, .class）

```
每个元素的id是唯一的，class可以多个元素同时用
选择器组
ul, ol {}
```
- 属性选择器

```
.class[width = 200px]{}
```
- 上下文选择器
- 伪类选择器

```
//结构化伪类选择器
e:first-child{} //第一个子元素设置
e:last-child{}
e:nth-child(2n+1){} //奇数位置元素
//UI伪类选择器
e:visited{}
hover,focus,active, checked
```
- 伪元素选择器

##### 3. CSS的级联Casscading
- 级联：一个元素可能会被多个CSS选择器选中，就会产生层叠/级联效果。
- 优先级规则算法：

```
1. 首先会判断重要性important
    a {
        color:blue; !important
    }
2. 规则来源： 开发者设置 > 用户设置  > 浏览器预设
2. 基于明确程度，ID > Class > Type , 算法通过（I-C-T）分量来判断
3. 顺序：后添加的生效
```
##### 4. CSS的继承
- 子元素会继承来自父元素的属性
- 不是所有的属性都是可继承的

##### 5. 厂商前缀
- 为了解决不同厂商的浏览器的兼容问题
- 常见的厂商前缀

```
Chrome:     -webkit
Firefox:    -moz
Opera:      -o
Safari:     -webkit
```

### CSS布局
##### 1. float

##### 2. Postion定位

##### 3. Flex
内容

### CSS高级
##### 1. 媒体查询
- 用来实现响应式布局控制
- 类似编程语言中的if语句
- 指定一组媒体条件，在满足所有媒体条件的时候，应用新的样式规则

```
@media(max-width:600px) and (width<=600px) and (mf2) {
    prestyle{
        new style
    }
    rules{

    }
}
```
开源库：[Respond.js](https://github.com/scottjehl/Respond/)

##### 2. mobile first 的响应式设计
- 之前的实现都是宽屏优先的，初始样式是面向宽屏的。
- 移动优先的初始设计是面向手机设备，然后对逐渐宽屏进行响应式设计。
- 优点：移动端是趋势，移动设备加载更快，在移动设备上不会去下载宽屏的样式文件。

```
css rules for mobile device 
...

@media screen and (min-width:768px){
    rule1
    rule2
}
// 769px小屏， 972px中屏，1200px大屏
```

##### 3. CSS3的Transform
- 为元素添加变形和变换效果，2D和3D
- 行内元素（inline）不是transformable元素，其他常见的都是的
- Transform 对于元素的内容，边框，边距等都会生效。
- 实质上是进行矩阵变换
- 默认以元素的中心为Transform的基点，可以通过transform-origin更改
- 几个基本操作

```
transform: rotate(45deg);   //旋转45度，度数可以是负数
transform: scale(x-value,y-value); //x,y方向上的缩放
transform: translate(x-value,y-value)//在xy上移动
transform: skew(45deg,45deg)   //x和y方向上倾斜度数
transform: rotate(45deg) scale(3);  //多个transform组合使用，需要注意的是从右向左生效的
transform-origin: left top; //将Transform的基点变成左上角
```

##### 4. CSS3的Transition
- 用于给文档中的样式变化添加过渡效果
- 不是所有的CSS属性都可以添加过渡效果

```
transition-property: width, background-color;     //设置transition关注的属性值的变化
transition-property: all;       //为所有的可添加属性添加transition效果
transition-duration: 3s, 1ms;        //各个属性平滑过渡效果的持续时长
transition-delay：1s, 2ms;      //执行过渡效果动画的等待时间
transition-timing-function: linear | ease-in | ease-out | ease-in-out | ease; //设置渐变的时间函数(常用的5个)
transition: property duration func delay;       // 简写属性
```

##### 6. CSS3的Animation
- 与Transition的不同：Animation可以支持一个系列的变化，而Transition只能实现从一个状态过渡到另一个
- 实现本质方式：指定元素在动画中的关键帧状态
- 例子： 实现弹球动画

```
// 先定义了一个动画序列效果
@Keyframes bounce {     
    0% {
        left: 0px;
        top: 0px;
    }
    0% {
        left: 50px;
        top: 100px;
    }
    0% {
        left: 100px;
        top: 0px;
    }
}
// 应用动画序列效果
.box:hover .ball {
    animation: bounce 4s linear 0s;     //鼠标悬停的时候触发动画
}
```
- 动画循环次数属性：animation-iteration-count
- 动画方向属性：animation-direction


### 总结
内容