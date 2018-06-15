---
ID: 228
post_title: Github贡献代码及提交PR流程
post_name: how-to-create-pr
author: 邓冰寒
post_date: 2018-06-15 10:00:00
layout: post
link: >
  https://devops.vpclub.cn/how-to-create-pr/
published: true
tags:
  - "PR"
  - Pull Request
categories:
  - 通用
---

# 在Github贡献代码及的提交PR的流程

Github 是一个家喻户晓的代码托管平台，对于大部分爱好编程的程序员来说，提交PR(Pull Request)并不陌生，本文将针对初学者来说明，如何一步一步的来提交PR。

## Fork 项目

我们以项目 https://github.com/john-deng/hello-world为例，如果你在github的用户名是 zhang-san, 那么你fork后项目地址是 https://github.com/zhang-san/hello-world.

* 在浏览器打开https://github.com/john-deng/hello-world
* 点击 Fork 按钮, 如下图所示

![fork](/images/pull-request/fork.png)

## 克隆项目到你的电脑

在命令行下面转到你的工作目录执行如下命令：

```bash
git clone https://github.com/zhang-san/hello-world.git
```

## 切换目录

切换到你刚刚克隆的文件夹下面：

```bash
cd hello-world
```

## 添加上游项目

添加上游项目，即你fork的来源：

```bash
git remote add upstream https://github.com/zhang-san/hello-world.git
git config --global --add http.followRedirects 1
```

## 创建新的分支并更改代码

```bash
git checkout -b my-feature

# 更改你的代码

```

## 保持你的fork同步

上游项目持续更新，你可以使用下面命令来保持你的fork始终和上游项目同步更新：

```bash
git fetch upstream
git rebase upstream/master
```

## 提交PR

如你已经完成上述过程，更改并提交了你的更改到你fork的github项目后，接下来就可以创建PR来贡献你的代码到上游项目中了。

![create-pr](/images/pull-request/create-pr.png)

如上图，点击 Pull Request 链接，跳转到PR提交页面，点击Create pull request 按钮即可。

![create-pr](/images/pull-request/submit-pr.png)

恭喜你，你已经完成了提交PR的所有步骤。