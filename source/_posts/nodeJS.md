title: nodeJS
date: 2015-10-27 22:26:56
tags:
---
观看慕课网nondeJS教程笔记

### 一、前言
- 可以解析JS代码（没有浏览器安全级的限制）
- 提供系统级别的API：
  + 文件的读写
  + 进程的管理
  + 网络通信
- 为什么学习nodeJS
  + 很火
  + 很强！
- 推荐网站
  + nodejs.org 官网
  + npmjs.com 编写新项目时使用
  + github.com 阅读优秀源码
  + stackoverflow.com 技术问答社区

### 二、安装node.js 0.10.33
- Node.js 偶数为稳定版本，奇数为非稳定版本
- 输入node -v 查看当前版本号
  npm -v 查看npm版本号 
- Linux下
  + cat /etc/redhat-release
  + rpm -q gcc rpm -q gcc-c++
  + yum -y install gcc gcc-c++ kernel-devel
  + python -V
  + yum -y update && yum -y grounpinstall "Development Tools"
- Mac下
  + 安装homebrew，官网brew.sh，找到：ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)" 复制到终端下
  + 通过homebrew安装nodeJS及mongodb：brew install git mongodb node ...
  + 更新node，切换不同版本 安装n模块：npm install -g n，n 0.10.22切换到10.22版本

### 三、等不及了来尝鲜
- 起一个web服务器

### 四、模块与包管理工具
- commonJs规范：包括了模块，包，系统二进制，控制台编码，套接字，单元测试。。。。来约定js的组织及编写，大部分标准在拟定和讨论之中，首先把执行不同任务的特定的代码块或者代码文件看做是独立的模块，每一个模块还看做是一个独立的作用域，存在依赖关系，对一个模块来说，分成三个关键部分：模块的定义、模块的标识、模块的引用。
- nodeJs/Couchdb都是对这个规范的实现。
- nodeJs接见并实现了commonJs，模块管理系统。通过依赖引入形成功能包。安装npm包管理工具，通过npm引入模块，每个模块都是独立完整的。
- nodeJs中文件与模块是一一对应的。模块分为：核心模块，文件模块，第三方模块。
- 模块的一个流程：
  + 创建模块 teacher.js
  + 导出模块 exports.add = function(){}
  + 加载模块 var teacher = require('./teacher.js')
  + 使用模块 teacher.add('Scott')