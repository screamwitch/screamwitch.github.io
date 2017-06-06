---
layout: post
title: webpack中使用ECharts
category: 学习笔记
date: 2017-06-06
description: "只有学习能让我快乐！"
tags:
    - 框架
comments: true
---


## npm安装ECharts  
>npm install echrats --save 
{: .blockquote cite="http://www.worldwildlife.org/who/index.html" }

## 引入ECharts
通过 npm 上安装的 ECharts 和 zrender 会放在node_modules目录下。可以直接在项目代码中 require('echarts') 得到 ECharts。

<!--<div class="env-header">JavaScript</div>-->
{% highlight js%}
var echarts = require('echarts');

// 基于准备好的dom，初始化echarts实例
var myChart = echarts.init(document.getElementById('main'));
// 绘制图表
myChart.setOption({
    title: { text: 'ECharts 入门示例' },
    tooltip: {},
    xAxis: {
        data: ["衬衫","羊毛衫","雪纺衫","裤子","高跟鞋","袜子"]
    },
    yAxis: {},
    series: [{
        name: '销量',
        type: 'bar',
        data: [5, 20, 36, 10, 10, 20]
    }]
});
{% endhighlight %}    

## 按需引入ECharts图标和组件
默认使用 require('echarts') 得到的是已经加载了所有图表和组件的 ECharts 包，因此体积会比较大，如果在项目中对体积要求比较苛刻，也可以只按需引入需要的模块。
例如上面示例代码中只用到了柱状图，提示框和标题组件，因此在引入的时候也只需要引入这些模块，可以有效的将打包后的体积从 400 多 KB 减小到 170 多 KB。
<!--<div class="env-header">JavaScript</div>-->
{% highlight js %}
// 引入 ECharts 主模块
var echarts = require('echarts/lib/echarts');
// 引入柱状图
require('echarts/lib/chart/bar');
// 引入提示框和标题组件
require('echarts/lib/component/tooltip');
require('echarts/lib/component/title');

// 基于准备好的dom，初始化echarts实例
var myChart = echarts.init(document.getElementById('main'));
// 绘制图表
myChart.setOption({
    title: { text: 'ECharts 入门示例' },
    tooltip: {},
    xAxis: {
        data: ["衬衫","羊毛衫","雪纺衫","裤子","高跟鞋","袜子"]
    },
    yAxis: {},
    series: [{
        name: '销量',
        type: 'bar',
        data: [5, 20, 36, 10, 10, 20]
    }]
});
{% endhighlight %}    
可以按需引入的模块列表见[https://github.com/ecomfe/echarts/blob/master/index.js](https://github.com/ecomfe/echarts/blob/master/index.js)

