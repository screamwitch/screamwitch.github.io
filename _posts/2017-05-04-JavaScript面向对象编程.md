---
layout: post
title: JavaScript面向对象编程
category: 技术文档
date: 2017-05-04
description: "来自五四青年节的问候——少年，唯有学习与爱不可辜负！"
tags:
    - JavaScript
comments: true
---

## OOP——面向对象编程
面向对象（Object-Oriented，OO）的语言有一个标志：它们都有类的概念，通过类可以创建任意多个具有相同属性和方法的对象。ECMAScript中没有类的概念，是通过 原型（prototype）的方式来实现面向对象编程的。  
ECMA-262把对象定义为：“无序属性的集合，其属性可以包含基本值、对象或者函数。”严格来讲，这就相当于说对象是一组没有特定顺序的值。对象的每个属性或方法都有一个名字，而每个名字都映射到一个值。正因为这样，我们可以把ECMAScript的对象想象成散列表：无非就是一组键值对，其中值可以是数据或函数。

## 构造函数
构造函数也是普通的函数，但是在其内部使用了this变量，对构造函数使用new运算符就能生成实例，并且this变量会绑定到实例对象上：
<div class="env-header">JavaScript</div>
{% highlight js linenos %}
function lineFamily(name,color){
    this.name=name;
    this.color=color;
};
var Brown=new lineFamily("Brown","brown");
var Cony=new lineFamily("Cony","white");
console.log(Brown.name); //Brown
console.log(Cony.color); //white
{% endhighlight %}  
此时“Brown”和“Cony”这两个“lineFamily”的实例对象，都会自动含有一个constructor属性，指向它们的构造函数，也就是“lineFamily”：
<div class="env-header">JavaScript</div>
{% highlight js linenos %}
console.log(Cony.constructor==lineFamily); //true
{% endhighlight %}   
JavaScript提供了instanceof运算符，用来验证原型对象与实例对象之间的关系：
<div class="env-header">JavaScript</div>
{% highlight js linenos %}
console.log(Cony instanceof lineFamily); //true
{% endhighlight %}   

### 构造函数的问题：浪费内存
如果为“lineFamily”添加几个不变的属性，就变成了这样：
<div class="env-header">JavaScript</div>
{% highlight js linenos %}
function lineFamily(name,color){
    this.name=name;
    this.color=color;
    this.company="line";
    this.eat=function(){
        console.log("什么都吃");
    };
};
//生成实例
var Brown=new lineFamily("Brown","brown");
var Cony=new lineFamily("Cony","white");
console.log(Brown.company); //line
Cony.eat(); //什么都吃
console.log(Brown.eat==Cony.eat);//false
{% endhighlight %}  
表面上看没什么问题，但实际上，没生成一个实例，company属性和eat()方法都是一样的，每一次生成实例，都是重复的内容，就会多占用内存，这样既不环保，也缺乏效率。
### 解决办法：让不变的属性在内存中只生成一次
通过把不变的属性和方法直接定义在构造函数的prototype对象上，让不变的属性在内存中只生成一次。
#### prototype模式
JavaScript规定，每个构造函数都有一个prototype属性，指向另一个对象，这个对象的所有属性和方法都会被构造函数的实例继承。
<div class="env-header">JavaScript</div>
{% highlight js linenos %}
function lineFamily(name,color){
    this.name=name;
    this.color=color;
};
lineFamily.prototype.company="line";
lineFamily.prototype.eat=function(){
    console.log("什么都吃");
};
//生成实例
var Brown=new lineFamily("Brown","brown");
var Cony=new lineFamily("Cony","white");
console.log(Brown.company); //line
Cony.eat(); //什么都吃
{% endhighlight %}  
这时所有lineFamily的实例的company和eat()，其实都是一个内存地址，指向prototype对象，因此就提高了执行效率。
<div class="env-header">JavaScript</div>
{% highlight js linenos %}
console.log(Brown.eat==Cony.eat);//true
{% endhighlight %}   
#### prototype相关方法
1.isPrototypeOf()：判断prototype对象和实例之间的关系，如果实例对象的原型链中具有prototype对象，则返回true；否则返回false：
<div class="env-header">JavaScript</div>
{% highlight js linenos %}
console.log(lineFamily.isPrototypeOf(Brown));//true
{% endhighlight %}  
2.hasOwnProperty()：判断属性是否为本地属性（私有属性），如果是本地属性，则返回true；如果是继承自prototype对象的属性或不具有该属性，则返回false：
<div class="env-header">JavaScript</div>
{% highlight js linenos %}
console.log(Brown.hasOwnProperty("name"));//true
console.log(Cony.hasOwnProperty("company"));//false
{% endhighlight %}     
3.in运算符：判断实例是否具有某个属性（不论是本地属性还是继承自prototype对象的属性），也可以遍历对象的所有属性：
<div class="env-header">JavaScript</div>
{% highlight js linenos %}
console.log("color" in Brown);//true
console.log("eat" in Cony);//true
for(var prop in Brown){
    console.log("Brown["+prop+"]="+Brown[prop]);
};
//Brown[name]=Brown
//Brown[color]=brown
//Brown[company]=line
//Brown[eat]=function (){
//    console.log("什么都吃");
//}
{% endhighlight %}   



[回到拉萨](https://screamwitch.github.io)
