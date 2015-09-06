title: CSS深入理解之line-height笔记
date: 2015-08-31 11:53:46
categories: 基本技能
tags: css line-height
---
观看慕课网张鑫旭大神的《CSS深入理解之line-height》系列课程的笔记。
### 一、line-height的定义
- line-height: 两行基线(baseline)之间的距离
- 不同文字，基线的位置不同

### 二、line-height与行内框盒子模型——css进阶必备知识
    <p>这是一行普通的文字，这里有个<em>em</em>标签。</p>
1. “内容区域”（content area），是一种围绕文字看不见的盒子。大小与font-size大小有关；
2. “内联盒子”（inline boxes），其不会让内容成块显示，而是排成一行。如果外部含`inline`水平标签（`span`，`a`，`em`等），则属于“内联盒子”。如果是个光秃秃的文字，则属于“匿名内联盒子”；
3. “行框盒子”（line boxes），每一行就是一个“行框盒子”，每个“行框盒子”又是由一个一个“内联盒子”组成；
4. `<P>`标签所在的“包含盒子”（containing box），此盒子由一行一行的“行框盒子”组成。

### 三、line-height的高度机理——深入理解内联元素的高度表现
	<p>这是一行普通的文字，这里有个<em>em</em>标签。</p>
	console.log(document.queryselector("p").clientHeight);
- 此时p的高度是：36px
- 高度不是由文字撑开，而是由line-height决定的。
- **内联元素的高度是由行高决定的！**
- 需要知道的：
  * 行高由于其继承性，影响无处不在，即使单行文本也不例外；
  * 行高只是幕后黑手，**高度的表现不是行高**，而是<b style="color:red;">内容区域</b>和<b style="color:red;">行间距</b>。
- **内容区域高度（content area）+ 行间距（vertical spacing）= 行高（line-height）**
  + <span style="color:red;">内容区域</span>（content area）高度只与<span style="color:red;">字号</span>（font-size）及<span style="color:red;">字体</span>（font-family）有关，与line-height没有任何关系；
  + 在simsun字体（宋体）下，内容区域高度等于文字大小值；
  + simsun字体下：font-size + 行间距 = line-height。
- 总结：行高决定内联盒子高度；行间距墙头草，可大可小（甚至负值），保证高度正好等同于行高。

### 四、line-height各类属性值——深入理解line-height不同类别值的不同表现
- line-height支持的属性值
  + normal——默认属性值
   - 跟着用户的浏览器走，且与元素字体关联。
  + number——使用数值作为行高值
  	- 例如：`line-height: 1.5;`
  	- 根据当前元素的font-size大小计算。
  	- 假设字体大小20像素，则实际的行高像素值是：`line-height = 1.5 * 20px = 30px;`
  + length——使用具体长度值作为行高值
  	- `line-height: 1.5em;`
  	- `line-height: 1.5rem;`
  	- `line-height: 1.5px;`
  	- `line-height: 1.5pt;`
  + percent——使用百分比值作为行高值
    - 例如：`line-height: 150%;`
    - 相当于设置了该`line-height`属性的元素的`font-size`大小计算。
    - 假设字体大小20像素，则实际的行高像素值是：`line-height = 150% * 20px = 30px;`
  + inherit——行高继承
    - IE8+ `input{ line-height: inherit;}`
    - `input`框等元素默认行高是`normal`，使用`inherit`可以让文本框样式可控性更强。
  + 应用元素有差别
    - `line-height: 1.5`所有可继承元素根据font-size重计算行高；
    - `line-height: 150%/1.5em`当前元素根据font-size计算行高，继承给下面的元素；
- **body全局数值行高使用经验**
  - `body{ font-size: 14px; line-height: ？ }`
  - blog 1.5/1.6
  - web 匹配20像素的使用经验——方便心算。
  - line-height = 20px/14px = 1.42857
  - `body{ font-size: 14px; line-height: 1.4286;}`
  - `body{ font: 14px/1.4286 'microsoft yahei';}`

### 五、line-height与图片的表现——深入探讨行高和图片的样式表现
  - 行高不会影响图片实际占据的高度！
  - 消除图片底部间隙：
  	+ `img { display: block;}`
  	+ `img { vertical-align: bottom;}`
  	+ `.box{ ling-height: 0;}`
    
### 六、line-height的实际应用
  - 实现大小不固定的图片、多行文字垂直居中：
  	`.box { line-heght: 300px; text-align: center;}`
  	`.box > img { vertical-align: middle;}`
  - 多行文本水平垂直居中
    `.box { line-height: 250px; text-align: center;}`
    `.box > .text { display: inline-block; line-height: normal; text-align: left; vertical-align: middle;}`
  - {height: 36px; line-height: 36px;}此时`height:36px`是多余的可以删掉
