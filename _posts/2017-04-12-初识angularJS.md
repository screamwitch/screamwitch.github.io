---
layout: post
title: 初识angularJS-学习笔记
category: 学习笔记
comments: true
date: 2017-04-10
description: "一起学习MVC框架angularJS"
tags:
    - angular
---

## angular四大核心特性
1. MVC  
&nbsp;&nbsp;&nbsp;&nbsp;M——Modle：数据模型层  
&nbsp;&nbsp;&nbsp;&nbsp;V——View：视图层，负责展示  
&nbsp;&nbsp;&nbsp;&nbsp;C——Controller：业务逻辑和控制逻辑  
2. 模块化
3. 指令系统
4. 双向数据绑定  

## Controller使用中的注意点
1. 不要试图去服用Controller，一个控制器一般只负责一小块视图；
2. 不要在Controller中操作DOM，这不是控制器的职责；
3. 不要在Controller里面做数据格式化，ng有很好用的表单控件；
4. 不要在Controller里面做数据过滤操作，ng有$filter服务；
5. 一般来说，Controller是不会相互调用的，控制器之间的交互会通过事件进行。 


## angular表达式
angularJS表达式写在双大括号{{}}内
<p data-height="265" data-theme-id="0" data-slug-hash="NjWqWq" data-default-tab="html,result" data-user="Conycony" data-embed-version="2" data-pen-title="angularJS表达式" class="codepen">See the Pen <a href="http://codepen.io/Conycony/pen/NjWqWq/">angularJS表达式</a> by Cony (<a href="http://codepen.io/Conycony">@Conycony</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

<div class="note alert">
*注：ng-init初始化应用程序数据，并不常使用，通常用控制器或模块代替。
</div>

## angular指令


> ng-app：定义一个angularJS应用程序的边界范围；  
> ng-init：初始化应用程序数据；  
> ng-model： 把HTML元素值绑定到应用程序；  
> ng-bind：把应用程序数据绑定到HTML视图；  
> ng-repeat：重复HTML元素（应用于数组）；  
> 自定义指令：使用.directive函数来添加自定义的指令。
> 
{: .blockquote}  

使用双花括号{{}}在页面绑定的数据，在页面加载不完全的时候会暴露在页面中，这看起来一点都不高级，可以使用ng-bind指令来修改上面的代码。
{% highlight html %}
<div ng-app>
  <p ng-bind="5+3"></p><!--表达式-->
  <p ng-init="quantity=4;price=5">
  总价：<span ng-bind="quantity*price"></span>
  </p><!--数字-->
  <p ng-init="firstName='Jim';lastName='Green'">
  姓名：<span ng-bind="firstName+' '+lastName"></span>
  </p><!--字符串-->
  <p ng-init="arr=[10,20,30]">
  序号：<sapn ng-bind="arr[1]"></span>
  </p><!--数组-->
  <p ng-init="person={firstName:'John',lastName:'Doe'}">
  名字：<span ng-bind="person.firstName"></span>
  </p><!--对象-->
</div>
{% endhighlight %}

