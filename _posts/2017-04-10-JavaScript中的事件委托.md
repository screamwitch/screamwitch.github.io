---
layout: post
title: JavaScript中的事件委托
date: 2017-04-10
excerpt: "懒癌晚期患者的第一篇——事件委托"
tags: [ JavaScript ]
comments: true
---

## 事件委托

&nbsp;&nbsp;&nbsp;&nbsp;事件，就是文档与浏览器窗口发生的一些特定的交互瞬间，像是点击（click）、加载（load）、鼠标进入（mouseenter）等。  
&nbsp;&nbsp;&nbsp;&nbsp;委托，就是这件事我不做，我让别人帮我做。当然了，委托这种事除了自己的亲爹亲妈，别人才不会管你呢，所以事件委托必须是委托给父级元素。

## 为什么要用事件委托

&nbsp;&nbsp;&nbsp;&nbsp;那么各位老铁们就要问了，明明自己就能干的事，为啥要委托给别人？  
&nbsp;&nbsp;&nbsp;&nbsp;且听我细细道来~  
&nbsp;&nbsp;&nbsp;&nbsp;先看下面这段代码：
<p data-height="265" data-theme-id="0" data-slug-hash="BWXmNY" data-default-tab="js,result" data-user="Conycony" data-embed-version="2" data-pen-title="未委托" class="codepen">See the Pen <a href="http://codepen.io/Conycony/pen/BWXmNY/">未委托</a> by Cony (<a href="http://codepen.io/Conycony">@Conycony</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

&nbsp;&nbsp;&nbsp;&nbsp;在上面的代码中，点击li标签，分别会执行不同的逻辑，分别用各自的点击事件来完成的。  
&nbsp;&nbsp;&nbsp;&nbsp;在JavaScript中，每一个函数都是对象，都会占用内存；内存中的对象越多，页面的性能就越差。在实际开发中，可能会有人给每个按钮都添加一个onclick事件，如果像这样不分青红皂白的向页面中添加大量的处理程序，就会影响页面的性能。  
&nbsp;&nbsp;&nbsp;&nbsp;那么，类似的逻辑要怎么提升页面的性能呢？对“事件处理程序过多”问题的解决方案，就是**事件委托**。

## 原生js中的事件委托
&nbsp;&nbsp;&nbsp;&nbsp;上面的代码可以利用事件委托来进行修改：
<p data-height="265" data-theme-id="0" data-slug-hash="YZmrNz" data-default-tab="js,result" data-user="Conycony" data-embed-version="2" data-pen-title="原生js事件委托" class="codepen">See the Pen <a href="http://codepen.io/Conycony/pen/YZmrNz/">原生js事件委托</a> by Cony (<a href="http://codepen.io/Conycony">@Conycony</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>
&nbsp;&nbsp;&nbsp;&nbsp;在这段代码里，我们将事件委托给了li的父级元素ul，也就是#list，由于所有的li元素都是#list的子节点，而且它们的事件会**冒泡**（肯定有人会问什么是冒泡，好的，下一篇就写事件冒泡），所以每个li元素的点击事件最终都会被#list的点击函数处理。在这个函数中，我们通过获取**事件源e.target**的id，并用switch语句检测事件源id，来决定当前的点击事件要执行什么逻辑。以上就构成了原生js的事件委托。

## 事件委托更为重要的作用
&nbsp;&nbsp;&nbsp;&nbsp;肯定有老铁发现过以下问题： 

1. js动态生成的元素绑定事件无效；
2. 元素的class是js动态设置的，通过这个class选择器获取到这个元素后绑定事件失效。

&nbsp;&nbsp;&nbsp;&nbsp;遇到上面的问题，有些老铁可能就会懵了，这是为什么呢？  
&nbsp;&nbsp;&nbsp;&nbsp;这是因为事件绑定只对dom中存在的元素有效，对于我们后来动态增加的元素是监测不到，所以绑定不了。  
&nbsp;&nbsp;&nbsp;&nbsp;同样，当你使用var aa = document.getElementsByTagName("动态生成的元素");来获取动态生成的元素的时候也是获取不到的，因为网页只会执行一次初始化绑定，之后动态生成的dom元素也是监测不到的。  
&nbsp;&nbsp;&nbsp;&nbsp;针对这个问题，解决的办法有以下两种：  

1. 生成元素的同时就绑定事件：
<p data-height="265" data-theme-id="0" data-slug-hash="WpVamM" data-default-tab="result" data-user="Conycony" data-embed-version="2" data-pen-title="未来元素绑定事件" class="codepen">See the Pen <a href="http://codepen.io/Conycony/pen/WpVamM/">未来元素绑定事件</a> by Cony (<a href="http://codepen.io/Conycony">@Conycony</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>
2. 使用事件委托  
jquery中使用on方法进行事件绑定时，接收的第一个参数event是事件，第二个是可选参数selector，如果添加了selector这个参数，就构成了事件委托，如下：  
{% highlight js %}
$('非动态父级元素').on("click","动态元素",function(){});
//给“动态元素”绑定事件，事件委托给非动态生成的父级元素
{% endhighlight %}  
  
  
&nbsp;&nbsp;&nbsp;&nbsp;最后再放一个例子吧，这个例子中的元素class是js动态改变的，改变后绑定事件使用了事件委托。
[点我，我就是例子](http://codepen.io/Conycony/pen/rywOze)


[回到拉萨](https://screamwitch.github.io)
