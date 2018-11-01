---
ID: 271
post_title: hicicd应用部署操作手册
post_name: operation-hicicd-user-manual
author: 邓冰寒
post_date: 2018-06-19 01:00:00
layout: post
link: >
  https://devops.vpclub.cn/operation-hicicd-user-manual/
published: true
tags:
  - User Menual
categories:
  - 运维
  - 运维
---
# hicicd应用部署操作手册

>通过运维开发团队的努力，我们的CI/CD自动化部署应用hicicd今天上线运行啦，我们将以往需要手动操作到部分尽量交给机器去执行，以提升工作效率。目前发布的是第一个版本。如果你在使用过程中遇到任何问题，请在下面留言（留言支持使用markdown语法），我们会在第一时间去完善。

## hicicd简介

我们在过去使用的是命令行工具来部署应用，大部分的业务逻辑写在客户端，现在改用微信小程序作为客户端，服务端部署了hicicd服务，既提高了部署的性能，又方便了部署。

下面让我们来教你如何使用hicicd。

## 打开微信小程序

你可以直接识别下面二维码进入小程序

![xw-devops](/images/operation-hicicd-user-manual/xw-devops-qrcode.jpg)

或者进入微信->发现->小程序

![wechat-sp](/images/operation-hicicd-user-manual/wechat-sp.png)

搜索“小微DevOps”

![serch-xw](/images/operation-hicicd-user-manual/search-xw.png)

## 登录控制台

进入小程序

![console](/images/operation-hicicd-user-manual/xw-home.png)

在菜单项选择控制台,如果需要部署dev、test环境，则选择控制台下的 开发/测试，如果需要部署stage、prod环境，则需要部署控制台下面的预言环境/生产环境

![menu](/images/operation-hicicd-user-manual/xw-menu.png)

使用你的gitlab账号登陆小微DevOps控制台(账户名使用名字拼音，不要用邮箱，例如chulei)

![login](/images/operation-hicicd-user-manual/login.png)

## 项目应用列表


选择你需要部署的项目

![list-project](/images/operation-hicicd-user-manual/project-list.png)

或者使用搜索来找到你的项目


![search-project](/images/operation-hicicd-user-manual/search.png)

## 部署方法

部署非常简单，从列表页面选择进入应用详情，按提示选择相应选项即可。

![select-options](/images/operation-hicicd-user-manual/deploy-project.png)


### 部署选项详细说明

| 选项       | 描述     | 示例       |
|------------|----------|------------|
|语言类型|java项目 选择java，cqrs项目 选择java-cqrs，nodejs项目 选择nodejs| java |
|环境| 选择项目需要部署到哪个环境上面，例如：gitlab上面的demo 如果选择dev，则项目部署到openshift 对应的namespace是demo-dev 依此类推。| dev |
|分支| 部署dev、test环境默认选择的是development分支上的代码，见图描述后面的分支名称development，当需要部署stage、prod则部署的是gitlab上的master分支的代码。 | master |
|路径| 当前路径是默认配置kong网关地址，如需变化则自行更改。|/namsepace/project|
|版本号 | 部署的版本号, 灰度发布需要使用到的。 这里只需要写大版本即可，注意是小写的v，如 v1 | v1 |
|构建| 是否强制编译和构建docker镜像|是|
|部署| 是否强制部署应用 如果更新了代码，则必须选择构建和部署。 | 是 |
|网关| 是否添加kong网关，首次部署项目时，若没有网关则需选择网关选项，若存在反之不选择。 | 是 |


![confirm](/images/operation-hicicd-user-manual/confirm-deployment.png)

部署中可以到OpenShift控制台查看日志

![os-view](/images/operation-hicicd-user-manual/start-build.png)

在build的过程中会出现Build Successful.后面会自动的push  images  当push成功后会自动部署

![built](/images/operation-hicicd-user-manual/build-success.png)

当容器build Complete后，就会启动pod，查看日志是否正常。

![app-log](/images/operation-hicicd-user-manual/app-log.png)

到这里我们的应用就部署成功了，构建Java应用大概需要一分钟左右。

![build-time](/images/operation-hicicd-user-manual/build-result.png)

如果你对本应用有任何疑问或建议，请在下方留言，谢谢！