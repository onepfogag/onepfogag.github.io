title: CSS深入理解之float笔记
date: 2015-10-22 11:23:28
categories: 基本技能
tags: css float
---
# css之float技巧收集

------

观看慕课网张鑫旭大神的《CSS深入理解之float》系列课程的笔记。

### 一、Float的历史——知其历史而更知当下
- 为了实现文字环绕效果

### 二、包裹与破坏——增强浮动的感性认知
- 包裹：1.收缩；2.坚挺；3.隔绝，及BFC，块级格式化上下文
- 具有包裹性的其他属性：
    + display: inlie-block/table-cell/...
    + position: absolute(近亲)/fixed/sticky
    + overflow: hidden/scroll
- 破坏：容器被破坏，父元素高度塌陷
- 具有破坏性的其他属性：
    + display: none
    + position: absolute(近亲)/fixed/sticky
- 浮动是魔鬼，新三无准则：无宽度、无图片、无浮动

### 三、被误解的float——是魔鬼还是情非得已
- 浮动使父元素高度塌陷是标准
- 只是单纯为了实现文字环绕效果

### 四、清除浮动——清除浮动面面观
- 清除浮动带来的影响
- 方法1：脚底插入`clear:both`
    + html
    + css
- 方法2：父元素BFC(IE8+)或haslayout(IE6/IE7)
    + float:left/right
    + position:absolute/fixed
    + overflow:hidden/scroll(IE7+)
    + display:inlie-block/table-cell(IE8+)
    + width/height/zoom:1/...(IE6/IE&)
- 最终方法为方法1、2的结合
```
.clearfix:after { content: ''; display: block; height: 0;overflow: hidden; clear: both; }
.clearfix { *zoom: 1;}
```
更好的方法
```
.clearfix:after { content: ''; display: table; clear: both; }
.clearfix { *zoom: 1;}
```
- clearfix应用在包含float的父元素上

### 五、float的滥用——不在其职而谋其政
- 浮动有两大基本特性：
  1. 元素的block块状化(转头化)；
  2. 破坏性造成的紧密排列特性(去空格化)；

### 六、浮动与流体布局——更多技能更多选择
- float + follow（inline）
- 左浮动 + text-align:center； + 右浮动
- 文字环绕的衍生——单侧固定
    标签元素：width+float
    后边的整体：padding-left/margin-left
- DOm与显示位置匹配的单侧固定布局，右侧宽度固定左侧自适应
    外层：width100% + float
    里层：padding-left/margin-left
    固定元素：width + float + margin负值
- 高级进化——只能自适应布局，左右都自适应
    一侧：float
    另一侧：display:table-cell(IE8+)
            display:inline-block(IE7)
            
### 七、浮动与兼容性——不同老爸不同命
#### IE7问题汇总：
- 含clear的浮动元素包裹不正确
- 浮动元素倒数2个莫名垂直间距问题
- 浮动元素最后一个字符重复问题
- 浮动元素楼体排列问题
- 浮动元素和文本不同一行的问题