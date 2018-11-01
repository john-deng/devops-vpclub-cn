---
ID: 309
post_title: 如何在OpenShift查看应用日志
post_name: operation-openshift-logging
author: 邓冰寒
post_date: 2018-07-17 20:11:11
layout: post
link: >
  https://devops.vpclub.cn/operation-openshift-logging/
published: true
tags:
  - Loging
  - OpenShift
  - 日志
categories:
  - 运维
  - 运维
---

# 如何在OpenShift查看应用日志

## 登录OpenShift

![openshift](/images/common/openshift-logo.png)

首先登录OpenShift控制台，在浏览器地址栏输入：

```browser
https://{{openshift-web-console}}:8443/
```

>注：替换 {{openshift-web-console}} 为你要访问的正确域名。

![login](/images/operation-openshift-logging/login-openshift.png)

选择 Log in with GitLab， 第一次登录会跳转到GitLab请求授权来实现单点登录。

## 查找项目

使用导航菜单先找到自己负责的项目

![view-all](/images/operation-openshift-logging/view-all-projects.png)

使用关键字查找项目

![find-project](/images/operation-openshift-logging/find-project.png)

## 查找应用

然后通过关键字查找到应用

![find-app](/images/operation-openshift-logging/find-app.png)

## 通过OpenShift控制台查看日志

接下来进入到 Logs, 可以看的上面提示

>Only the previous 5000 log lines and new log messages will be displayed because of the large log size.

那么历史日志怎么查看呢？进入Pod页面后选择 Logs -> View Archive:

![view-archive](/images/operation-openshift-logging/view-archive.png)

## 通过Kibana查看日志

### 查看当前日志

我们可以通过Kibana来查询历史日志，点击 View Archive 链接进入到Kibana。

![pod-log](/images/operation-openshift-logging/pod-log.png)

你所看的默认到日志为所选择到Pod日志，你也可以在Kibana搜索栏直接搜索你想要到应用，比如查看 order-query 的日志, 可以输入以下表达式查找

```browser
kubernetes.namespace_name:"moses-prod" AND kubernetes.labels.app:"order-query"
```

如果想进一步过滤查询条件，如查询所有 ERROR 级别日志

```browser
kubernetes.namespace_name:"moses-prod" AND kubernetes.labels.app:"order-query" AND message: *ERROR*
```

如查询某个id为 100360681064038401 的日志

```browser
kubernetes.namespace_name:"moses-prod" AND kubernetes.labels.app:"order-query" AND message: *100360681064038401*
```

>如欲知更高级查询，请查看[官方文档](https://www.elastic.co/guide/en/beats/packetbeat/current/kibana-queries-filters.html)

![app-log](/images/operation-openshift-logging/app-log.png)

### 查看历史日志

你还可以通过过滤不同时间段来查找历史日志

![historical-log](/images/operation-openshift-logging/historical-log.png)

选择你要查看的时间段

![abs-log](/images/operation-openshift-logging/historical-abs-log.png)

## 问题及解决办法

如果你打开Kibana后出现错误提示，如下图所示，表示你的浏览器有脏数据，请清除浏览器缓存再次尝试。

![trouble-shot](/images/operation-openshift-logging/trouble-shot.png)

如果清除浏览器缓存？我们以Chrome为例

![clear-browsing-data](/images/operation-openshift-logging/clear-browsing-data.png)

![clear-browsing-data-confirm](/images/operation-openshift-logging/clear-browsing-data-confirm.png)