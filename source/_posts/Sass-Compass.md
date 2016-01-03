title: 'Sass&Compass'
date: 2015-12-05 16:00:47
tags:
---
# Sass和Compass必备技能

## Sass篇

### 1.Sass，Compass与CSS三者的关系 

### 2.环境安装
- sass-lang.com
- ruby
- rvm rvm.io
- cakebrew.com
- rvm:
	+ source /Users/pangjian/.rvm/scripts/rvm 使用rvm前执行
  + rvm list known 列出所有已知的ruby版本
  + rvm list 列出通过本机安装的ruby环境
  + rvm install 2.0.0 安装ruby
  + rvm use 2.0.0 使用版本
  + rvm use --default 设置默认版本
  + rvm use system 使用系统默认ruby
  + 国内网络问题换用淘宝提供的ruby sources：gem sources --remove https://rubygems.org/ 首先移除gemorg;gem sources -a https://ruby.taobao.org/ 添加淘宝的org;gem sources -l 查看源
  + gem install sass 安装sass
  + sass -v 查看当前sass版本
  + gem update 更新所有的gem程序
  + gem install sass --version=3.3 安装特定版本的sass
  + gem list 列出本地安装的所有ruby程序包
  + gem uninstall sass --version=3.3.0 删除某版本sass
  + gem uninstall sass 删除sass
- compass-style.org 

### 3.Sass语法
#### 一、基础篇
- sass、scss转换
  + sass-convert main.scss main.sass
- 监听模式 开启这个模式才会使scss写完自动生成css并打印debug和warning等。
  + compass watch 
- 制定默认编码
  + @charset "UTF-8" 此为默认设置
- **变量**
  + $headline-ff
  + screen.scss 称为宿主文件
  + import 引入文件
  + control directive
- 基于sass的既定规则：
  1. 没有文件后缀名的时候，sass会添加.scss或者.sass的后缀。
  2. 同一目录下，局部文件和非局部文件不能重名。
- 注释：css的注释均可以使用
  + // 注释不会显示在css文件中
  + 会有默认样式可方便调试
  + /**/ 注释会显示
- 类嵌套
- & 父类选择器
- 属性嵌套

#### 二、进阶语法
- 变量操作
  1. 直接操作变量，即变量表达式
  2. 通过函数
    + 跟代码块无关的函数，多是sass内置函数，称为functions
    + 可重用代码块，称为mixin
      1. @include 的方式调用
      2. extend 的方式调用
- sass functions :http://sass-lang.com/documentation/Sass/Script/Functions.html
- mixin 一般放于页面顶部，import之后或者单独放于一个文件
  + $width 支持类似的参数
  + $width: 50% 支持类似的默认参数
- extend 继承
  + extend 继承多个选择器
  + 连续继承
  + extend 不可以继承一个选择器序列 .a .b
  + 使用%，用来构建只用来继承的选择器

#### 三、高级篇
- sass中的@media跟CSS区别：
  + sass中的media query可以内嵌在css规则中，在生产CSS的时候，media query才会被提到样式的最高级。
  + 好处，避免重复书写选择器。
- 嵌套的一些副作用：
  + 增加了样式修饰的权重
  + 制造了这种样式位置的依赖
- 最佳实践：
  + main-sec-headline
  + main-sec-detail
  + 又不如嵌套清晰
  + 这时可以使用sass的at-root指令
- sass中的if条件判断
- @each @for 
- sass 的四种输出格式

## Compass篇
- 记得开启 compass watch
### Compass核心模块
- Reset @import "compass/reset"
- Layout @import "compass/layout"
- 只有这两个模块是需要指定引入
- @import "compass" 即可默认包含其他五大模块
- CSS3 提供跨浏览器的css3能力
- Helpers 函数
- Typography 文本样式
- Utilities 没有办法放到其他模块的内容都可以放到这个模块，mixin
- browser 配置默认支持浏览器和支持到哪个版本
- 重新编译：compass compile // --force 强制编译

### Compass安装
- gem install compass-normalize

### 引入插件
- 修改config.rb
- require 'compass-normalize'

### compass/import-once的用处
- 防止sass自身的import多次引入同一个文件
- 可以使用最后加一个感叹号的方式告诉import重复引用
- 例如：@import "compass/reset!";
- 注释前加感叹号 例如 /*!  */ 可在compressed模式下保留注释

### 使用
- @import "normalize";

### Normalize核心模块
- base
- html5
- links
- typography
- embeds
- groups
- forms
- tables

### Normalize子模块方式引入
```
@import "normalize-version";
/* 分割线1 */
@import "normalize/base";
/* 分割线2 */
@import "normalize/html5";
/* 分割线3 */
@import "normalize/links";
```

### Reset核心Mixin(12个)
```
@import "compass/reset/utilities";
@include global-reset;
```
相当于
```
@import "compass/reset"
```

- global-reset
- nested-reset
- reset-box-model
- reset-font
- reset-body
- reset-html5
- reset-list-style
- reset-table
- reset-table-cell
- reset-quotation
- reset-image-anchor-border
- reset-display($selector,$important)
- reset-focus

### Layout模块 提供页面布局的控制能力 使用度最低的模块
- @import "compass/layout"
- @import "compass/layout/grid-background";
- @import "compass/layout/sticky-footer"; 提供页面的页脚始终置于页脚的能力
- @import "compass/layout/stretching"; 用于将一个子元素拉伸填满整个父元素的能力
- @include stretch();
- @include sticky-footer(30px);

### CSS3模块 跨浏览器的css3能力
- @import "compass/css3" 引入模块
- 调用@include box-shadow(1px, 1px, 3px, 2px)

### browser support 模块 更改浏览器支持情况，影响其他6个模块的输出
- @import "compass/support" 引入模块
- 引入css3模块时可不写此模块，因为css3模块会引用此模块
- compass的另一个模式：compass interactive
- ctrl + c 退出某一个模式
- browsers() 查看浏览器
- browser-versions(chrome) 查看支持的浏览器的版本
- $supported-browsers: chrome, firefox; 设置支持的浏览器
- $browser-minimum-versons:("ie:" "8"); 设置支持浏览器的版本

### tpography模块
- 分别引入模块或全部引入，一共四个模块
- @import "compass/typography"
- Links 链接修饰模块 
- Lists 列表修饰模块
- Text 文本修饰模块
- Vertical Rhythm 垂直韵律修饰模块

#### links模块
- @import "compass/typography/links"
- 超链接样式修改一般为两种
  + 平时去掉下划线，hover时出现
  @include hover-link();
  + 修改不同状态下超链接的颜色
	@include link-colors(n$normal, $hover, $active, $visited, $focus)
- @include unstyled-link(); 抹平超链接样式，使其与父容器一致

#### lists模块
- @import "compass/typography/lists"
- 去掉li默认的点
  @include no-bullets();
- 列表横向展示 inline
  @include inline-list();
- 浮动list
  @include horizontal-list();
- inline-block
  @include inline-block-list(padding);

#### text模块
- @import "compass/typography/text"
- 强制换行
  @include force-wrap();
- 不换行
  @include nowrap();
- 超出部分省略号
  @include ellipsis();
- 按钮的隐藏文本方式
  @include hide-text();
  @include squish-text();
- 引用图片当按钮
  @include replace-text($url);

#### vertical thythm 模块
- @import "compass/vertical_rhythm"
- 大段文字布局时候使用
- 浏览器默认16px字体大小
- 1.4/1.5倍作为基准高
- $base-font-size: 16px;
- $base-line-height: 24px;
- 初始化字体和行高
  @include establish-baseline()；
- @include debug-vertical-alignment();
- h1{font-size: 3em; /* 48 / 16 = 3*/}
- @include adjust-font-size-to(48px);想让h1的字体变为多大
- @include leader()和trailer()指定元素的margin-top和margin-bottom

### Helpers模块
- 建立images文件夹
- 名字需要跟config.rb中的images_dir配置项一致
- inline-image 用于生产base64格式的图片
- 例如： background-image: inline-image
('xxx.png'); //目录下的文件名
- cache buster :
background-image: url('../images/xxx.png?t=234521'); //麻烦，易出错
- image-url 解决了问题
image-url('xxx.png') compass根据图片生成时间会生成cache buster
- 更改```http_path = "/"```配置项修改路径
- 配置项```relative_assets = true``` 修改为相对路径
- font-face mixin：
@include font-face("FontAwesome", font-files("xx.otf","xx.eto","xx.svg","xx.ttf","xx.woff"));

### Utilities模块
- @import "compass/utilities"

#### color 模块
- brightness(#000) 查看亮度值
- contrasted()

#### print 模块
- 需跟print.css模块协同使用

#### tables 模块
- table-borders 边框
- table-scaffolding 脚手架
- alternating-rows-and-columns 样式

#### general 模块
- @include clearfix();
- **```caniuse.com```** 查看浏览器支持情况

##### hacks 模块
- *zoom: 1; //只有ie67能识别带`*`的css
- _display: inline; //只有ie6能识别下划线的css

##### minimums 模块
- min-height

##### tag cloud 标签云模块
- @include tag-cloud(24px);

#### sprites 精灵图合图模块
- 只需要将所有图片放入一个文件中，并在scss中使用
- 新建一个scss文件，`_icons.scss`
- 在screen.scss中引入，`@import "icons";`
- 在icons模块中单独引入，`@import "compass/utilities/sprites";`
- images下建立logo子目录
- 在icons中引入所有的png图片，`@import "logo/*.png";`
- 与此同时，logo的同级目录会生成一个形如“logo-s1ecd90d7e9.png”的合图
- 使用只需要在icons中调用，`@include all-logo-sprites();` all 和 sprites 中间是我们的目录名