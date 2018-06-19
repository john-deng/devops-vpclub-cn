---
ID: 67
post_title: 如何撰写文档
post_name: general-how-to-write-doc
author: 邓冰寒
post_date: 2018-06-12 17:11:47
layout: post
link: >
  https://devops.vpclub.cn/general-how-to-write-doc/
published: true
tags:
  - "markdown"
  - doc
  - 文档
categories:
  - 通用
---



# hicicd项目部署操作手册

## 项目登陆

使用gitlab登陆该控制台(账户名使用名字拼音，不要邮箱 例如chulei)


## 项目列表

选择你需要部署的项目
![](/images/hicicd-operation-manual/listProject.png)
## 项目部署
![](/images/hicicd-operation-manual/build.png)
语言类型： 
java项目 选择java 、
cqrs项目 选择java-cqrs、
nodejs项目 选择nodejs

环境：

选择项目需要部署到哪个环境上面，例如：gitlab上面的demo 如果选择dev，则项目部署到openshift 对应的namespace是demo-dev 依此类推。

分支：

需要你们部署项目对应的gitlab分支

镜像标签来源：
dev、test  来源：dev
stage、prod 来源：stage


版本号：
部署账号的版本号。

服务网格：
是否需要istio监控信息 true -> 需要 

默认强制构建、强制部署勾选

部署后查看容器编译的日志情况
![](/images/hicicd-operation-manual/hicicdBuilds.png)

![](/images/hicicd-operation-manual/hicicd_logs.png)
在build的过程中会出现Build Successful.后面会自动的push  images  当push成功后会自动创建pod
![](/images/hicicd-operation-manual/build-success.png)

当容器Complete后 就会启动pod，查看日志是否正常。
