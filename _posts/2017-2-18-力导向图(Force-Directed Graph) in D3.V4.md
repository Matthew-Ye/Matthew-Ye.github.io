---
layout: post
title: 力导向图(Force-Directed Graph) in D3.V4
date: 2016-11-02
categories: 数据可视化
tags: [数据可视化,D3]
description: 简单介绍了第四版D3.js中力导向图的绘制方法。
---

# 力导向图(Force-Directed Graph) in D3.V4
D3.js的第4版在力导向图方面与第3版区别较大。由于发布时间尚短，笔者发现国内几乎没有这方面的总结或教程，故借着学习记录总结。更具体的说明情参考[D3技术手册](https://github.com/d3/d3/wiki/Tutorials), [力导向图部分](https://github.com/d3/d3-force)

本文将基于[https://roshansanthosh.wordpress.com/2016/09/25/forces-in-d3-js-v4/](https://roshansanthosh.wordpress.com/2016/09/25/forces-in-d3-js-v4/)并结合自己的理解，简单介绍几种基础模式，并给出简单示例，对重点进行注解。

## 1. forceX, forceY 
最简单的恢复力，使一个节点回到最稳定的位置。

[点我显示效果](https://github.com/Matthew-Ye/d3/blob/master/positioningForce.html)

```html
<html>  
  <head>  
        <meta charset="utf-8">  
        <title>Positioning Forces (forceX,forceY)</title>  
  </head> 
  <body>  
		<script src="http://d3js.org/d3.v4.min.js" charset="utf-8"></script>  
        <script>

		var width = 1000,
		    height = 600;

		var svg = d3.select("body")
					  .append("svg")
					  .attr("width",width)
		            .attr("height",height);

		var nodeData = [{'x':100,'y':100,'id':'Node'}];

		var simulation = d3.forceSimulation(nodeData); //Initializing Simulation; nodes have been assigned

		// Define the forces: method 1
		// simulation.force("xAxis",d3.forceX().strength(5).x(width/2))
				    // .force("yAxis",d3.forceY().strength(5).y(height/2));

		// Define the forces: method 2
		var xAxisForce = d3.forceX().strength(5).x(width/2);
		var yAxisForce = d3.forceY().strength(5).y(height/2);
		simulation.alphaDecay(0.01).force("xAxis",xAxisForce).force("yAxis",yAxisForce); //设置了alphaDecay()

		//Define the node
		var node = svg.selectAll("circle").data(nodeData)
		            .enter().append("circle")
		            .attr("r",30).attr("cx",width/2).attr("cy",height/2)
		            .attr("fill","black").attr("opacity",0.5)
		            .call(d3.drag()
		            .on("start",dragstarted)
		            .on("drag",dragged)
		            .on("end",dragended));

		simulation.on("tick",ticked);

		 function dragstarted(d)
		 {
		    simulation.restart();
		    simulation.alpha(1.0);
		    d.fx = d.x;
		    d.fy = d.y;
		 }

		 function dragged(d)
		 {
		    d.fx = d3.event.x;
		    d.fy = d3.event.y;
		 }

		 function dragended(d)
		 {
		    d.fx = null;
		    d.fy = null;
		    simulation.alphaTarget(0.1);
		 }

		 function ticked(){
		     node.attr("cx", function(d){ return d.x;})
		         	 .attr("cy", function(d){ return d.y;})
		 }
        </script>  
  </body>  
</html>  
```

### 定义添加SVG

```
		var width = 1000,
		    height = 600;

		var svg = d3.select("body")
					  .append("svg")
					  .attr("width",width)
		            .attr("height",height);
```
D3.v4.js允许使用svg或canvas，这里我们使用svg。

### 定义节点
`var nodeData = [{'x':100,'y':100,'id':'Node'}];`   
x,y为节点的初始位置。

### 初始化仿真
```
var simulation = d3.forceSimulation(nodeData); 
//Initializing Simulation; Nodes have been assigned
```
使用force需要创建仿真。传入参数为我们的节点数组。初始化的同时节点也进行了部署，我们可以在console中验证：

![nodeData](http://oib4fv2v2.bkt.clouddn.com/nodeData.png)

vx,vy分别为x方向和y方向的速度；x, y为节点当前的坐标。

### 定义力
```
simulation.force("xAxis",d3.forceX().strength(5).x(width/2))
		  .force("yAxis",d3.forceY().strength(5).y(height/2));
```
似乎有些混乱，那我们换一种更清楚的表达方式：   

```
var xAxisForce = d3.forceX().strength(5).x(width/2);
var yAxisForce = d3.forceY().strength(5).y(height/2);
simulation.alphaDecay(0.01).force("xAxis",xAxisForce).force("yAxis",yAxisForce); //设置了alphaDecay()
```
- .strength()定义力的大小；   
- 如果使用d3.forceX([x])能创造一种新的沿x轴的positioning force使节点到达指定的位置，缺省为0。这里我们将其至于svg中央。
- simulation.alpha(): alpha是仿真中非常重要的一个参数。它是一个0到1的值，表示仿真进行的程度。仿真开始时alpha被设置为1，随后减小，减小的速度取决于alphaDecay，当alpha小于alphaTarget时仿真结束。alphaTarget缺省为0.1。

`simulation.alphaDecay(0.01).force("xAxis",xAxisForce).force("yAxis",yAxisForce);`
我们设置了alphaDecay为0.01，作用力为xAxisForce和yAxisForce。其中"xAxis"和"yAxis"只是我们给这些力的命名，可以自己取，下同。
### 定义节点
```
		var node = svg.selectAll("circle").data(nodeData)
		            .enter().append("circle")
		            .attr("r",30).attr("cx",width/2).attr("cy",height/2)
		            .attr("fill","black").attr("opacity",0.5)
		            .call(d3.drag()
		            .on("start",dragstarted)
		            .on("drag",dragged)
		            .on("end",dragended));
```
重点是d3.drag()中添加了3个事件监听器。

- dragstarted()做一些初始化操作，其中fx, fy定义了当我们开始拖动时节点的位置。
- dragged()结合ticked()对节点位置不断更新
- dragended()将fx, fy设为null，使节点恢复状态。

### tick
最后需要用到事件tick，每个时刻都要调用它进行内容的更新。这里我们只需要更新节点坐标即可。

## 2. forceCenter

D3将所有节点视作相同的质量，Centering Forces使整个系统的质心始终保持在一个特定位置。具体代码于Positioning Forces几乎一致，只是在定义力的地方改为
`simulation.force("centeringForce",d3.forceCenter(width/2,height/2));`
显然，我们将质心固定在svg的中央。

```
<html>  
  <head>  
        <meta charset="utf-8">  
        <title>Centering Forces (forceCenter)</title>  
  </head> 

    <body>  
		<script src="http://d3js.org/d3.v4.min.js" charset="utf-8"></script>  
        <script>

		var width = 1000,
		    	height = 600;

		var svg = d3.select("body")
					  .append("svg")
					  .attr("width",width)
		            	  .attr("height",height);

		// var nodeData = [{'x':225,'y':100,'id':'First'}];
		var nodeData = [	{'x': 100,'y':100,'r':10},
							{'x': 300,'y':100,'r':10},
							{'x':50,   'y':200,'r':10},
							{'x':350,'y':200,'r':10},
							{'x':100,'y':300,'r':10},
							{'x':300,'y':300,'r':10}];

		var simulation = d3.forceSimulation(nodeData); //Initializing Simulation; nodes have been assigned

		simulation.force("centeringForce",d3.forceCenter(width/2,height/2));

		 var node = svg.selectAll("circle").data(nodeData)
		            .enter().append("circle")
		            .attr("r",function(d){ return d.r;}).attr("cx",function(d){ return d.x;}).attr("cy",function(d){ return d.y;})
		            .attr("fill","black").attr("opacity",0.5)
		            .call(d3.drag()
		            .on("start",dragstarted)
		            .on("drag",dragged)
		            .on("end",dragended));
		 
		 function dragstarted(d)
		 {
		    simulation.restart();
		    simulation.alpha(1.0);
		    d.fx = d.x;
		    d.fy = d.y;
		 }

		 function dragged(d)
		 {
		    d.fx = d3.event.x;
		    d.fy = d3.event.y;
		 }

		 function dragended(d)
		 {
		    d.fx = null;
		    d.fy = null;
		    simulation.alphaTarget(0.1);
		 }

		 function ticked(){
		     node.attr("cx", function(d){ return d.x;})
		         	 .attr("cy", function(d){ return d.y;})
		 }

		 simulation.on("tick",ticked);
		  
        </script>  
		
    </body>  
</html>  
```
[最终效果](https://github.com/Matthew-Ye/d3/blob/master/centeringForce.html)

## 3. forceManyBody
ForceManyBody可以用来仿真引力和斥力，如万有引力和静电力。所有节点相互之间都存在力。

`d3.forceManyBody()`会使用缺省的参数，如果需要自己设置可以分别定义引力和斥力：

```
		var attractForce = d3.forceManyBody().strength(200).distanceMax(400)
                     .distanceMin(60);
		var repelForce = d3.forceManyBody().strength(-140).distanceMax(50)
                   .distanceMin(10);

		var simulation = d3.forceSimulation(nodeData).alphaDecay(0.03)
                 					.force("attractForce",attractForce)
                 					.force("repelForce",repelForce);
```
- distanceMax: 考虑力的最大距离
- distanceMin: 考虑力的最小距离

[最终效果](https://github.com/Matthew-Ye/d3/blob/master/manyBodyForces.html)

## 4. forceCollide
上一节我们虽然引入了吸引力和排斥力，但是当我们快速移动节点时会穿越。一般来说，这在我们的物理世界中是不科学的。因此我们可以需要引入碰撞力。   
用碰撞力代替排斥力。碰撞力确保节点之间保持一定的距离。

```
		var attractForce = d3.forceManyBody().strength(80).distanceMax(400)
                     .distanceMin(80);
		var collisionForce = d3.forceCollide(12).strength(1).iterations(100);

		var simulation = d3.forceSimulation(nodeData).alphaDecay(0.01)
                   .force("attractForce",attractForce)
                   .force("collisionForce",collisionForce);
```
- d3.forceCollide([radius])  
给碰撞力一个特定的半径
- collide.iterations([iterations])   
iterations为迭代次数。次数越多，约束的刚性越好，节点也越不易重叠，但是相应的运行时间会增加，缺省为1。

[最终效果](https://github.com/Matthew-Ye/d3/blob/master/collisionForces.html)

## 5. forceLink
节点之间的力，因此我们需要在定义节点的同时再定义他们之间的联系。

### 定义Link
```
var nodeLinks = [{"source":0,"target":1,"distance":10},
{"source":1,"target":2,"distance":20},
{"source":2,"target":3,"distance":30},
{"source":3,"target":4,"distance":40},
{"source":4,"target":5,"distance":50},
{"source":5,"target":6,"distance":60},
{"source":6,"target":0,"distance":70}];
```
这表示了7个节点首尾相连。

### 定义Link Forces
```
var linkForce  = d3.forceLink(nodeLinks).distance(dist).strength(2);
var simulation = d3.forceSimulation(nodeData).alphaDecay(0.01).force("linkForce",linkForce);
```

### 综合其他力
我们也可以与之前所提到的力结合：

```
		var linkForce  = d3.forceLink(nodeLinks).distance(dist).strength(2);	//Define link forces
		var simulation = d3.forceSimulation(nodeData).alphaDecay(0.01).force("linkForce",linkForce)
																			.force("charge", d3.forceManyBody())
    																			.force("center", d3.forceCenter(width / 2, height / 2));
```

效果如[Link Forces1](https://github.com/Matthew-Ye/d3/blob/master/forceLink1.html)

### 引入标注
此时我们无法分辨节点的id，因此我们引入标注：

首先定义标注属性：

```
	   //Define the text
		var text = svg.selectAll("text")
				   .data(nodeData)
				   .enter()
				   .append("text")
				   .attr("font-family", "sans-serif")
				   .attr("font-size", "11px")
				   .attr("fill", "yellow");
```

由于标志也随着节点位置变化而变化，因此在ticked()函数中我们也需要对text实时更新：

```
		      text.text(function(d) {
				        return d.id;
				   })
				   .attr("x", function(d, i) {
				        return d.x;
				   })
				   .attr("dx", "-3.5")
				   .attr("dy", "2.5")
				   .attr("y", function(d) {
				        return d.y;
				   })
```
效果如[Link Forces2](https://github.com/Matthew-Ye/d3/blob/master/forceLink2.html)

### 引入线条links
虽然此时Link Forces已经存在，但是我们看不到它。因此有时我们需要把它们显示出来，因此引入Links。

```
		//Define the link
		 var link = svg.append("g")
	      		.attr("class", "links")
	    		.selectAll("line")
	   			.data(nodeLinks)
	    		.enter()
	    		.append("line");
```
同样，需要在ticked()中更新link：

```
		      text.text(function(d) {
				        return d.id;
				   })
				   .attr("x", function(d, i) {
				        return d.x;
				   })
				   .attr("dx", "-3.5")
				   .attr("dy", "2.5")
				   .attr("y", function(d) {
				        return d.y;
				   })
```
但是此时打开我们网页仍然看不到线条，我们需要对link的style进行设置。在CSS或<style></style>中设置其stroke，如：

```
<style>
.links line {
  stroke: #aaa;
}
</style>
```
我们就可以看到最终效果：
[Link Forces3](https://github.com/Matthew-Ye/d3/blob/master/forceLink3.html)

## 小结
本文仅仅简单介绍了新版D3的力导向图部分，仅冰山一角，还有很多有趣的内容大家可以自行探索。Canvas绘图可以参考[Force-Directed Tree II](https://bl.ocks.org/mbostock/9a8124ccde3a4e9625bc413b48f14b30)。文中如有错误和不足，欢迎指出。