# 基于golang语言web框架gin搭建应用-01

## 简介

因为工作上的需求，由于两个项目都是管理类型的，故在项目开始之处就考虑做一个能够快速开发应用的框架，考虑到时间紧，任务重，同时市面上很多优秀的基于golang的web框架，综合考虑，不重复造轮子，站在巨人的肩膀上，使用了golang的web框架gin来搭建基础框架，在该框架中使用了很多由于的模块，在此感谢作者的付出。

因为是写一个系列文章， 故文章会从零开始来搭建这个框架，大部分代码会从原有的项目copy过来。主要目的记录该框架是如何搭建的，以及在上面如果快速开发一个后台应用。

gin框架地址:[https://github.com/gin-gonic/gin](https://github.com/gin-gonic/gin)

## 开发环境

* PC: Mac
* IDE: Goland 2018.3 EAP
* GO:go1.10 darwin/amd64 
* 环境变量`GOPATH:/Users/cloudy/Documents/go`  
* 包依赖管理：govendor

## 新建项目

### 新建项目

项目名称为base-framework,如果其他项目使用或者修改项目名称，需要修改代码中包引入涉及到的项目名，可以批量替换。

![](/assets/01-create-project.png)

### 依赖初始化

项目目录下执行`govendor init`，在项目目录下将会出现`vendor`文件夹

添加项目需要使用到的包,后面章节若使用了新的包将会更新下面的目录

```

```



