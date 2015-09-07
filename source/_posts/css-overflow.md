title: CSS深入理解之overflow笔记
date: 2015-09-07 09:31:46
categories: 基本技能
tags: css overflow
---
观看慕课网张鑫旭大神的《CSS深入理解之overflow》系列课程的笔记。
### 一、overflow基本属性——基本表现，包括兼容性等
- 基本属性
  + visible（默认） 超出部分依然显示
  + hidden 
  + scroll 右下加入滚动条
  + auto 自动
  + inherit 
- 如果overflow-x和overflow-y值相同，等同于overflow，如果overflow-x和overflow-y值不同，且一个值为visible另一个为hidden、scroll、auto，那么这个visible会被重置为auto
- 作用的前提
  + 非`display: inline`水平~
  + 对应方位的尺寸限制。width/height/max-width/max-height/absplute拉伸
  + 对于单元格`td`等。还需要`table`为`table=layout: fixed`
- 给按钮加样式`overflow: visible`可完善ie7下按钮样式

### 二、overflow与滚动条——滚或不滚，我就在那里
- 滚动条出现的条件
  + overflow:auto/overflow:scroll `<html>/<textarea>`
  + 内容尺寸超出容器尺寸限制
- body/html与滚动条
  + 无论什么浏览器，默认滚动条均来自`<html>`!而不是`<body>`标签
  + `<body>`默认`.5em``margin`值
  + ie6/7 `html { overflow: scroll }`
  + ie8+ `html { overflow: auto }`
  + 去除页面默认滚动条，只需要：`html { overflow: hidden;}`
- body/html与滚动条-JS与滚动高度
  + Chrome浏览器是：`document.body.scrollTop`;
  + 其他浏览器是：`document.documentElement.scrollTop`;  
  + 目前两者不会同时存在，建议写法：
  + `var st = document.document.documentElement.scrollTop || document.body.scrollTop;`
- overflow的padding-bottom确实现象
  + `.box { width: 400px; height: 100px; padding: 100px 0; overflow: auto; }`
  + 导致不一样的`scrollHeight`（元素内容高度）
- 滚动条的宽度机制
  + 滚动条会占用容器的可用宽度或高度
- 滚动栏的宽度
```
.box { width: 400px; overflow: scroll;}
.in { *zoom: 1; }

<div class="box">
	<div id="in" class="in"></div>
</div>

console.log(400 - document.getElementById("in").clientWidth);
```
  + 结果均是17px  

- 水平居中跳动问题的修复
  + `html { overflow-y: scroll; }` 默认显示滚动栏，不好
  + `.container { padding-left: calc(100vw - 100%); }` 100vw为浏览器宽度，100%为可用内容宽度

- 自定义滚动条
  + **整体部分** ::-webkit-scrollbar
  + **两端按钮** ::-webkit-button
  + **外层轨道** ::-webkit-scrollbar-track
  + **内层轨道** ::-webkit-scrollbar-track-piece
  + **滚动滑块** ::-webkit-scrollbar-thumb
  + **边角** ::-webkit-scrollbar-corner
  + 实际开发常用的属性：
    - 血槽宽度：`::-webkit-scrollbar { width: 8px; height: 8px;} `
    - 拖动条：`::webkit-scrollbar-thumb { background-color: rgba(0,0,0,.3); border-radius: 6px;}`
    - 背景槽：`::webkit-scrollbar-track { background-color: #ddd; border-radius: 6px; }`
  + jquery滚动条自定义插件
    - [项目页面](http://manos.malihu.gr/jquery-custom-content-scroller/) 
    - [Github地址](https://github.com/malihu/malihu-custom-scrollbar-plugin) 
- iOS原声滚动回调效果
  + -webkit-overflow-scrlling : touch;

### 三、overflow与BFC——清除浮动、自适应布局等
- BFC(Block formatting context) 块级格式化上下文
- 结界元素，内部元素怎么翻云覆雨都不会影响外边
- 出发BFC的值为：auto scroll hidden
- 清楚浮动影响
- 避免margin穿透问题
- 两栏自适应布局
- **广为流传**清楚浮动写法，兼容IE6：
```
.clearfix { *zoom: 1; }
.clearfix:after { content: ''; display: table; clear: both; }
```
- BFC属性表现：
+ `overflow: hidden;` 自适应，但“溢出不可见”
+ `float + float` 包裹性 + 破坏性，无法自适应，块状浮动布局
+ `position:absolute` 脱离文档流，自娱自乐
+ `display: inline-block` 包裹性，无法自适应；IE6/7block水平不相识
+ `display: table-cell` 包裹性，但天生无溢出特性，绝对宽度也能自适应
- **广为流传**的两栏自适应布局写法：
```
.cell { 
	display: table-cell; width: 2000px; // IE8+ BFC特性
	*display: inline-block; *width: auto; // IE7- 伪BFC特性
}    
```

### 四、overflow与绝对定位——隐藏失效与滚动固定
- 失效原因：
> 绝对定位元素不总是被父级元素overflow属性剪裁，尤其当overflow在绝对定位元素及其包含块之间的时候。
> 包含块指“含position:relative/absolute/fixed声明的父级元素，没有则body元素”
- 避免失效：
+ overflow元素自身为包含块
+ overflow元素的子元素为包含块
+ 任意合法transform声明当做包含块（兼容性问题）
- overflow失效的妙用：
```
<div class="h0 ovh tr">
	&nbsp;<img src="fixed-right.png;" class="abs ml10 mt30">
</div>
.h0 { height: 0;}
.ovh { overflow: hidden;}
.tr { text-align: right;}
.abs { position: absolute;}
```

###　五、依赖overflow的样式表现——大腿抱起来
- resize拉伸（**overflow属性值不能为visible**）
  + resize:both 水平垂直拉伸
  + resize:horizontal 水平拉伸
  + resize:vertical 垂直拉伸
  + resize 17*17像素
- text-overflow: ellipsis文字溢出点点点省略（**必须overflow:hidden;**）

### 六、overflow与锚点技术
- 锚点定位的本质**改变容器的滚动高度**
- 触发方法：
  + url地址中的锚链与锚点元素
  + 可focus的锚点元素处于focus态
- overflow选项卡技术（**适用于单页应用**）
```
<div class="box">
	<div class="list" id="one">1</div>
	<div class="list" id="two">2</div>
	<div class="list" id="three">3</div>
	<div class="list" id="four">4</div>
</div>
<div class="link">
	<a class="click" href="#one">1</a>
	<a class="click" href="#two">2</a>
	<a class="click" href="#three">3</a>
	<a class="click" href="#four">4</a>
</div>
```