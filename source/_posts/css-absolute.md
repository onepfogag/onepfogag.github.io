title: CSS深入理解之absolute笔记
date: 2015-10-21 10:46:05
categories: 基本技能
tags: css absolute
---
# css之absolute技巧收集

------

观看慕课网张鑫旭大神的《CSS深入理解之absolute》系列课程的笔记。
### 一、ansolute与float——鲜为人知的兄弟关系
- 两大特性
  + 包裹性
  + 破坏性

### 二、别把我和relative拴在一起——越独立越强大
- relative和absolute是分离的，对立的。绝不是兄弟关系！
- absolute越独立越强大
  + 超越overflow

### 三、无依赖的absolute定位——上辈子就是折翼的天使
- 无依赖，指不受relative限制的absolute
- 定位的行为表现
  + 脱离文档流
  + 折翼的天使，保持原有位置并处于周围元素之上
  + absolute会使浮动属性失效
  + absolute如果是inline-block会跟随之前的inline元素
  + absolute如果是block则会换行，IE7达到此效果会在外围套用一个div
  + 这种无依赖的absolute定位可称为不影响其他布局的绝对定位下的相对定位之王

### 四、强大的折翼天使——无依赖absolute相对定位实力篇
1. 图片图标来覆盖，无依赖、真不赖；
  - 父元素不使用relative，当前元素设置absolute后，使用margin控制位置。
  - 利用跟随性，调整需要绝对定位的元素的文档位置。在之前需要用<!---->注释消除换行和空格，使div和i紧密相连。
2. 如果定位下拉框，最佳实践来分析；
  - ul列表设置absolut并用margin控制。
3. 对齐居中或边缘，定位实现有脸面；
  - 父元素text-align:center、right;，当前元素前加入"```&nbsp;```"，margin-left一个值。
4. 星号时有时没有，破坏队形不用愁；
  - 对于表单必填说明的星号，可以使用absolute后margin-left负值。
5. 图文对齐兼容差，绝对定位来开挂；
  - absolute后，margin-left自身宽度负值。
6. 文字溢出不够放，不值一提就小样！
  - 超出容器宽度依然可以显示。

### 五、脱离文档流二三事——天使专属隐藏技
- 回流与重绘
- 动画尽量作用在绝对定位元素上！
- 垂直空间的层级，后来居上
- z-index无依赖
  1. 如果只有一个绝对定位元素，自然不需要z-index，自然覆盖普通元素；
  2. 如果两个绝对定位，控制DOM流的前后顺序达到需要的覆盖效果，依然无z-index；
  3. 如果多个绝对定位交错，非常少见，z-index:1控制；
  4. 如果非弹框类的绝对定位元素z-index>2，必定冗余，请优化！

### 六、天使的翅膀——top/right/bottom/left
- 受父级有定位元素的限制，通常组合使用

### 七、top/right/bottom/left与width/height——异曲同工的特殊表现
- 相互替代性，绝对定位的方向是对立(如：left vs right，top vs bottom)的时候，结果不是瞬间位移，而是身体的爆炸拉伸，例如实现遮罩
  + width:100%;height100%;
  + left:0;top:0;bottom:0;right:0;
- 差异所在-拉伸更强大，例如实现全屏自适应的容器层
  + position: absolute; left:0; right:200px;
  + position: absolutel left:0; width: calc(100%-200px);
- 相互支持性
  1. 容器无需固定的width/height值，内部元素亦可拉伸；

```css
.container{
  display: inline-block;
  position: relative;
}
.cover{
  position: absolute;
  left:0;top:0;right:0;bottom:0;
  background-color:#fff;
  opacity: .5;filter: alpha(opacity=50);
}
```

css驱动的左右半区翻图浏览效果：
```css
.prev,.next{
  width: 50%;
  *background-image: url(about:blank);
  postition: absolute; top: 0;bottom: 0;
  outline: none;
}
.prev { cursor: url(pic_prev.cur), auto;left: 0; }
.next { cursor: url(pic_next.cur), auto;right: 0; }
```

  2. 容易拉伸，内部元素支持百分比width/height值；
- 相互合作性，width/height大于天使布局。
- 当尺寸限制、拉伸以及margin:auto同时出现的时候，就会有绝对定位元素的绝对居中效果！IE8+。

### 八、absolute网页整体布局——适合移动web的布局策略
- 摆脱狭隘的定位
- absolute与整体布局
- 1.body降级，子元素升级
    - 升级子div(.page)满屏：
`.page{position: absolute; left: 0; top: 0; right: 0; bottom: 0 }`
    - 绝对定位受限于父级，因此，page需要：
`html, body{ height: 100%; }`    
- 2.各模块-头尾、侧边栏（PC端）各居其位

```css
header,footer { position: absolute; left: 0; right: 0;}
header {height: 48px; top: 0;}
footer {height: 52px; bottom: 0;}
aside {
  width: 250px;
  position: absolute; left: 0; top: 0; bottom: 0;
}
```

- 3.内容区域想象成body
  
```css
.content{
  position: absolute;
  top: 48px bottom: 52px;
  left: 250px(如果有侧边栏);
  overflow: auto;
}
```

- 4.全屏覆盖与page平级

```html
.overlay{
  position: absolute;
  top: 0; right: 0; bottom: 0; left: 0;
  background-color: rgba(0,0,0,.5);
  z-index: 9;
}
<div class="page"> </div>
<div class="overlay"> </div>
```