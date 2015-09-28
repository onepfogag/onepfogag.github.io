title: 移动web相关技巧
date: 2015-09-28 10:22:46
categories: 基本技能
tags: mobile web viewport meta rem media
---
观看慕课网碧仔（叶琳）大神的《Hello，移动WEB》的笔记
### 一、Pixel移动开发像素知识
- px：CSS pixels 逻辑像素，浏览器使用的抽象单位
- dp,pt：device independent pixels 设备无关像素
- dpr：devicePixelRatio 设备像素缩放比
- 计算公式：1px = (dpr)^2\*dp
- DPI：打印机每英寸可以喷的墨汁点（印刷行业）
- PPI：屏幕每英寸的像素数量，即单位英寸内的像素密度
- 计算公式：以iPhone5为例子
  + ppi = √(1136^2 + 640^2)/4 = 326ppi（视网膜Retina屏）

|	         | ldpi    | mdpi | hdpi | xhdpi |
| ---------- | ------- | ---- | ---- | ----- |
| ppi	     | 120     | 160  | 240  | 320   |
| 默认缩放比 | 0.75    | 1.0  | 1.5  | 2.0   |

- 设备分辨率640\*1136 dp -> √(1136^2 + 640^2)/4 = 326ppi -> dpr=2 -> 1px = dpr^2 \* dp -> iPhone5的屏幕为320\*568 px

### 二、Viewport 
- 手机浏览器默认：
  + 页面渲染在一个980px(ios)的viewport
  + 缩放
- visual viewport ：视口viewport 窗口缩放
- layout viewport ：布局viewport 整个页面

### 三、Meta标签
- `<meta name="viewport" content="name=value,name=value">`
  + width：设置布局viewpost的特定值（“device-width”）
  + initial-scale： 设置页面的初始缩放
  + minimum-sacle： 最小缩放
  + maximum-scale： 最大缩放
  + user-scalable： 用户能否缩放
- 布局viewport：document.body.clientWidth = 980
- 度量viewport：window.innerWidth = 1030
- [布局viewport] = [设备宽度] = [度量viewport]
  + `<meta name="viewport" content="width=device-width,initial-scale=1,user-scalable=no">`


### 四、flex布局
- 传统的不定宽高的水平垂直居中
```
.myoff-wrapper{
	postion: absolute;
	top: 50%;
	left: 50%;
	z-index: 3;
	-webkit-transform: translate(-50%,-50%);
	border-radius: 6px;
	background: #fff;
}
```
- [flexbox版]不定宽高的水平垂直居中
```
.parent{
	justify-content: center; //子元素水平居中
	align-items: center;     //子元素垂直居中
	display: -webkit-flex;
}
```
- 更多示例请参考：testpages/web_flex.html

### 五、响应式设计
- 媒体查询：
```
@media screen and (max-width: 1024px) {
	#pagewrap{
		width: 95.5%;
	}
	#content {
		width: 62%;
	}
	#content .article .hr{
		width: 66%;
		margin-left: 34%;
	}
}
```
- 媒体类型：
  + screen（屏幕）
  + print（打印机）
  + handheld（手持设备）
  + all（通用）
- 常用媒体查询参数：
  + width——视口宽高
  + height——视口宽高
  + device-width——设备宽高
  + device-height——设备宽高
  + orientation——检查设备处于横向(landscape)还是竖屏(portrait)
- 设计点一：百分比布局
- 设计点二：弹性图片 `img{max-width:100%;}`
- 设计点三：重新布局，显示和隐藏
  + **经常需要切换位置元素使用【绝对定位】，减少重绘提高渲染性能**
- 权衡利弊

### 六、移动web特别样式处理
- 一像素边框，根本原因：1px使用2dp渲染
```
.sidebar .folder li{
	padding: 8px 0 8px 15px;
	color:#7c7c7c;
	cursor: pointer;
	position: relative;
}
.folder li + li.before{
	position: absolute;
	top: -1px;
	left: 0;
	content: '';
	width: 100%;
	height: 1px;
	border-top: 1px solid #ddd;
	-weblit-transform: scaleY(0.5);
}
```
- 相对单位rem，基值rem = screen.width/10
- 文字不使用rem：font-size
- 单行文本溢出
```
.inaline {
	overflow: hidden;
	white-space: nowrap;
	text-overflow: ellipsis;
}
```
- 多行文本溢出
```
.intwoline {
	display: -webkit-box !important;
	overflow: hidden;

	text-overflow: ellipsis;
	word-break: break-all;

	-webkit-box-orient: vertical;
	-webkit-line-clamp: 2;
}
```

### 七、Tap基础事件
- 300毫秒的故事
  + 移动web页面的click事件响应都要慢上300ms
  + 300ms延迟怎么破？使用Tap事件代替click事件
  + Tap“点透”的bug
    - 使用缓动动画，过渡300ms延迟
    - 等，个人觉得缓动动画的解决方式最好所以不记录了
- Touch基础事件
- 触摸事件：touchstart、touchmove、touchend、touchcancel
- 触摸事件属性：
  + touches：跟踪触摸操作的touch对象数组
  + targetTouches：特定事件目标的touch对象数组
  + changeTouches：上次触摸改变的touch对象数组
- 每个touch对象包含属性：
  + clientX：触摸目标在视口中的x坐标。
  + clientY：触摸目标在视口中的y坐标。
  + identifier：标识触摸的唯一ID。
  + pageX：触摸目标在页面中的x坐标（包含滚动）。
  + pageY：触摸目标在页面中的y坐标（包含滚动）。
  + screenX：触摸目标在屏幕中的x坐标。
  + screenY：触摸目标在屏幕中的y坐标。
  + target：触摸的DOM节点目标。
- 局部滚动开启弹性滚动：Android不支持原生的弹性滚动！但可以借助三方库iScroll来实现
```
body{
	overflow: scroll;
	-webkit-overflow-scrolling: touch;
}
```
- 下拉刷新：在document上绑定事件，判断开始和结束是否都在顶层，并有一定的区域。例：兴趣部落，网易新闻
- 上拉加载：使用scroll事件，而不是touch事件（android有bug）
- 以下为两个例子


```
<!doctype html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1,user-scalable=no">
<title>Test Tap</title>
<style>
	.touchpad{
		width: 100%;
		height: 200px;
		font-size: 40px;
		text-align: center;
		line-height: 200px;
		background: rgba(0,0,0,0.5);
		position: relative;
		color: #ddd;
	}

	.ball{
		display: none;
		position: absolute;
		width: 25px;
		height: 25px;
		border-radius: 15px;
		background-color: #7AE6FF;
		top: 0;
		left: 0;
	}
	p{
		height: 30px;
	}

</style>
</head>
<body>

<p id="desc"></p>
<div id="touchPad" class="touchpad">触摸板</div>
<div id="ball" class="ball"></div>

<script src="../js/zepto.js"></script>
<script type="text/javascript">
	var touchpad = document.querySelector('#touchPad'),
		ball = document.querySelector('#ball'),
		desc = document.querySelector('#desc');

	//简单的touch事件（废弃）
	var touchHandler = function(e){
		var type = e.type;


		//注意touchend的touches和targetTouches为空，只偶有changeTouches获取上次一的touchTlist
		if(type != 'touchend'){
			var pos = {
				x : e.touches[0].clientX,
				y : e.touches[0].clientY
			}

			ball.style.left = pos.x + 'px';
			ball.style.top = pos.y + 'px';
			desc.innerHTML = e.type + '(clienX:'+pos.x+', clientY:'+ pos.y+')';
		}else{
			desc.innerHTML = e.type ;
		}
		
		switch(type){
			case 'touchstart':
				ball.style.display = 'block'; 
				break;
			case 'touchmove': 
				//android的4.4,4.0的bug：只触发touchstart，和一次的touchmove，touchend不触发
				//加入evnt.preventDefault
				event.preventDefault();
				break;
			case 'touchend': 
				ball.style.display = 'none';
				break;
		}
	}

	touchpad.addEventListener('touchstart',touchHandler);
	touchpad.addEventListener('touchmove', touchHandler);
	touchpad.addEventListener('touchend', touchHandler);


</script>

</body>
</html>
```

```
<!doctype html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1,user-scalable=no">
<title>Test Tap</title>
<style>
	.touchpad{
		width: 100%;
		height: 200px;
		font-size: 40px;
		text-align: center;
		line-height: 200px;
		background: rgba(0,0,0,0.5);
		position: relative;
		color: #ddd;
	}

	.ball{
		display: none;
		position: absolute;
		width: 25px;
		height: 25px;
		border-radius: 15px;
		background-color: #7AE6FF;
		top: 0;
		left: 0;
	}
	p{
		height: 30px;
	}

</style>
</head>
<body>

<p id="desc"></p>
<div id="touchPad" class="touchpad">触摸板</div>
<div id="ball" class="ball"></div>

<script src="../js/zepto.js"></script>
<script type="text/javascript">
	var touchpad = document.querySelector('#touchPad'),
		ball = document.querySelector('#ball'),
		desc = document.querySelector('#desc');


	//获取touch的点(兼容mouse事件)
	var getTouchPos = function(e){
        var touches = e.touches;
        if(touches && touches[0]) {
            return { x : touches[0].clientX ,
            		 y : touches[0].clientY };
        }
        return { x : e.clientX , y: e.clientY };
    }

    //计算两点之间距离
    var getDist = function(p1 , p2){
        if(!p1 || !p2) return 0;
        return Math.sqrt((p1.x - p2.x) * (p1.x - p2.x) + (p1.y - p2.y) * (p1.y - p2.y));
    }
    //计算两点之间所成角度
    var getAngle = function(p1 , p2){
        var r = Math.atan2(p2.y - p1.y, p2.x - p1.x);
        var a = r * 180 / Math.PI;
        return a;
    };
     //获取swipe的方向
    var getSwipeDirection = function(p2,p1){
        var dx = p2.x - p1.x;
        var dy = -p2.y + p1.y;    
        var angle = Math.atan2(dy , dx) * 180 / Math.PI;

        if(angle < 45 && angle > -45) return "right";
        if(angle >= 45 && angle < 135) return "top";
        if(angle >= 135 || angle < -135) return "left";
        if(angle >= -135 && angle <= -45) return "bottom";

    }


	var startEvtHandler = function(e){
		var pos = getTouchPos(e);
		ball.style.left = pos.x + 'px';
		ball.style.top = pos.y + 'px';
		ball.style.display = 'block';

		var touches = e.touches; 
        if( !touches || touches.length == 1 ){ //touches为空，代表鼠标点击
            point_start = getTouchPos(e);
            time_start = Date.now();
        }
	}

	var moveEvtHandler = function(e){
		var pos = getTouchPos(e);
		ball.style.left = pos.x + 'px';
		ball.style.top = pos.y + 'px';


		point_end = getTouchPos(e);
		e.preventDefault();
	}

	//touchend的touches和targetTouches没有对象，只有changeTouches才有
	var endEvtHandler = function(e){
		ball.style.display = 'none';

		var time_end = Date.now();

		//距离和时间都符合
        if(getDist(point_start,point_end) > SWIPE_DISTANCE && time_start- time_end < SWIPE_TIME){
           
           var dir = getSwipeDirection(point_end,point_start);
           touchPad.innerHTML = 'swipe'+dir;
        }
	}

   
    var SWIPE_DISTANCE = 30;  //移动30px之后才认为swipe事件
    var SWIPE_TIME = 500;     //swipe最大经历时间
    var point_start,
    	point_end,
    	time_start,
    	time_end;

    //判断是PC或者移动设备
	var startEvt, moveEvt, endEvt;
	if("ontouchstart" in window){
        startEvt="touchstart";
        moveEvt="touchmove";
        endEvt="touchend";
    }else{
        startEvt="mousedown";
        moveEvt="mousemove";
        endEvt="mouseup";            
    }

	touchpad.addEventListener(startEvt, startEvtHandler);
	touchpad.addEventListener(moveEvt, moveEvtHandler);
	touchpad.addEventListener(endEvt, endEvtHandler);



</script>

</body>
</html>
```