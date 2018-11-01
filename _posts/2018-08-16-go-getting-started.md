---
ID: 378
post_title: Go入门指南
post_name: go-getting-started
author: 邓冰寒
post_date: 2018-08-16 12:48:00
layout: post
link: >
  https://devops.vpclub.cn/go-getting-started/
published: true
tags:
  - golang
categories:
  - Go
---
# Go入门指南

## 起源与发展

Go 语言起源 2007 年，并于 2009 年正式对外发布。它从 2009 年 9 月 21 日开始作为谷歌公司 20% 兼职项目，即相关员工利用 20% 的空余时间来参与 Go 语言的研发工作。该项目的三位领导者均是著名的 IT 工程师：Robert Griesemer，参与开发 Java HotSpot 虚拟机；Rob Pike，Go 语言项目总负责人，贝尔实验室 Unix 团队成员，参与的项目包括 Plan 9，Inferno 操作系统和 Limbo 编程语言；Ken Thompson，贝尔实验室 Unix 团队成员，C 语言、Unix 和 Plan 9 的创始人之一，与 Rob Pike 共同开发了 UTF-8 字符集规范。自 2008 年 1 月起，Ken Thompson 就开始研发一款以 C 语言为目标结果的编译器来拓展 Go 语言的设计思想。

在 2008 年年中，Go 语言的设计工作接近尾声，一些员工开始以全职工作状态投入到这个项目的编译器和运行实现上。Ian Lance Taylor 也加入到了开发团队中，并于 2008 年 5 月创建了一个 gcc 前端。

Russ Cox 加入开发团队后着手语言和类库方面的开发，也就是 Go 语言的标准包。在 2009 年 10 月 30 日，Rob Pike 以 Google Techtalk 的形式第一次向人们宣告了 Go 语言的存在。

直到 2009 年 11 月 10 日，开发团队将 Go 语言项目以 BSD-style 授权（完全开源）正式公布了 Linux 和 Mac OS X 平台上的版本。Hector Chu 于同年 11 月 22 日公布了 Windows 版本。

作为一个开源项目，Go 语言借助开源社区的有生力量达到快速地发展，并吸引更多的开发者来使用并改善它。自该开源项目发布以来，超过 200 名非谷歌员工的贡献者对 Go 语言核心部分提交了超过 1000 个修改建议。在过去的 18 个月里，又有 150 开发者贡献了新的核心代码。这俨然形成了世界上最大的开源团队，并使该项目跻身 Ohloh 前 2% 的行列。大约在 2011 年 4 月 10 日，谷歌开始抽调员工进入全职开发 Go 语言项目。开源化的语言显然能够让更多的开发者参与其中并加速它的发展速度。Andrew Gerrand 在 2010 年加入到开发团队中成为共同开发者与支持者。

在 Go 语言在 2010 年 1 月 8 日被 Tiobe（闻名于它的编程语言流行程度排名）宣布为 “2009 年年度语言” 后，引起各界很大的反响。目前 Go 语言在这项排名中的最高记录是在 2017 年 1 月创下的第13名，流行程度 2.325%。

## Go发展历程

* 2007 年 9 月 21 日：雏形设计
* 2009 年 11 月 10日：首次公开发布
* 2010 年 1 月 8 日：当选 2009 年年度语言
* 2010 年 5 月：谷歌投入使用
* 2011 年 5 月 5 日：Google App Engine 支持 Go 语言
  
从 2010 年 5 月起，谷歌开始将 Go 语言投入到后端基础设施的实际开发中，例如开发用于管理后端复杂环境的项目。有句话叫 “吃你自己的狗食”，这也体现了谷歌确实想要投资这门语言，并认为它是有生产价值的。

Go 语言的官方网站是 golang.org，这个站点采用 Python 作为前端，并且使用 Go 语言自带的工具 godoc 运行在 Google App Engine 上来作为 Web 服务器提供文本内容。在官网的首页有一个功能叫做 Go Playground，是一个 Go 代码的简单编辑器的沙盒，它可以在没有安装 Go 语言的情况下在你的浏览器中编译并运行 Go，它提供了一些示例，其中包括国际惯例 “Hello, World!”。

更多的信息详见 github.com/golang/go，Go 项目 Bug 追踪和功能预期详见 github.com/golang/go/issues。

Go 通过以下的 Logo 来展示它的速度，并以囊地鼠（Gopher）作为它的吉祥物。

![go-log](/images/go-getting-started/go-logo.jpg)

谷歌邮件列表 golang-nuts 非常活跃，每天的讨论和问题解答数以百计。

关于 Go 语言在 Google App Engine 的应用，这里有一个单独的邮件列表 google-appengine-go，不过 2 个邮件列表的讨论内容并不是分得很清楚，都会涉及到相关的话题。go-lang.cat-v.org/ 是 Go 语言开发社区的资源站，irc.freenode.net 的#go-nuts 是官方的 Go IRC 频道。

@golang 是 Go 语言在 Twitter 的官方帐号，大家一般使用 #golang 作为话题标签。

这里还有一个在 Linked-in 的小组：www.linkedin.com/groups?gid=2524765&trk=myg_ugrp_ovr。

Go 编程语言的维基百科：en.wikipedia.org/wiki/Go_(programming_language)

Go 语言相关资源的搜索引擎页面：gowalker.org

Go 语言还有一个运行在 Google App Engine 上的 Go Tour，你也可以通过执行命令 go install go-tour.googlecode.com/hg/gotour 安装到你的本地机器上。对于中文读者，可以访问该指南的 中文版本，或通过命令 go install https://bitbucket.org/mikespook/go-tour-zh/gotour 进行安装。

## 搭建开发环境

`重要提示： 由于众所周知的原因，在安装前请自行准备梯子(关键字：shadowsocks, proxifier)，否则下载不到安装包`

Go 的环境安装和配置并不复杂，这是[官方入门指南](https://golang.org/doc/install)，我们推荐安装GVM - Go 语言多版本安装及管理利器。

* 管理 Go 的多个版本，包括安装、卸载和指定使用 Go 的某个版本
* 查看官方所有可用的 Go 版本，同时可以查看本地已安装和默认使用的 Go 版本
* 管理多个 GOPATH，并可编辑 Go 的环境变量
* 可将当前目录关联到 GOPATH
* 可以查看 GOROOT 下的文件差异

### 安装 GVM

```bash
bash <<(curl -s -S -L https://raw.githubusercontent.com/moovweb/gvm/master/binscripts/gvm-installer)  
```

### 使用 GVM

直接输入 gvm，查看使用帮助

```bash
Usage: gvm [command]

Description:
  GVM is the Go Version Manager

Commands:
  version    - print the gvm version number
  get        - gets the latest code (for debugging)
  use        - select a go version to use (--default to set permanently)
  diff       - view changes to Go root
  help       - display this usage text
  implode    - completely remove gvm
  install    - install go versions
  uninstall  - uninstall go versions
  cross      - install go cross compilers
  linkthis   - link this directory into GOPATH
  list       - list installed go versions
  listall    - list available versions
  alias      - manage go version aliases
  pkgset     - manage go packages sets
  pkgenv     - edit the environment for a package set
```

### 安装指定的 Go 版本

```bash
gvm install go1.10
```

它背后做的事情是先把源码下载下来，再用 C 做编译。安装好之后，指定默认使用这个版本，加上 --default 即可，省去每次敲 gvm use

```bash
gvm use go1.10 --default  
```

这个时候查看 go 环境变量

```bash
go env
```

应该显示正确的GOROOT，GOPATH等

```bash
GOARCH="amd64"
GOBIN=""
GOCACHE="/Users/johnd/Library/Caches/go-build"
GOEXE=""
GOHOSTARCH="amd64"
GOHOSTOS="darwin"
GOOS="darwin"
GOPATH="/Users/johnd/.gvm/pkgsets/go1.10/hidevops:/Users/johnd/go:/Users/johnd/.gvm/pkgsets/go1.10/global"
GORACE=""
GOROOT="/Users/johnd/.gvm/gos/go1.10"
GOTMPDIR=""
GOTOOLDIR="/Users/johnd/.gvm/gos/go1.10/pkg/tool/darwin_amd64"
GCCGO="gccgo"
CC="clang"
CXX="clang++"
CGO_ENABLED="1"
CGO_CFLAGS="-g -O2"
CGO_CPPFLAGS=""
CGO_CXXFLAGS="-g -O2"
CGO_FFLAGS="-g -O2"
CGO_LDFLAGS="-g -O2"
PKG_CONFIG="pkg-config"
GOGCCFLAGS="-fPIC -m64 -pthread -fno-caret-diagnostics -Qunused-arguments -fmessage-length=0 -fdebug-prefix-map=/var/folders/zw/ynx11jnx6j5gwbhkqqdqf0wm0000gn/T/go-build844998127=/tmp/go-build -gno-record-gcc-switches -fno-common"
```

### 管理多个 GOPATH

GVM 还一个很实用的功能，可以管理多个 GOPATH（Go package），通过 gvm pkgset 命令, 可以查看使用帮助。

```bash
gvm pkgset
```

```bash
= gvm pkgset

* http://github.com/moovweb/gvm

== DESCRIPTION:

GVM pkgset is used to manage various Go packages

== Usage

  gvm pkgset Command

== Command

  create     - create a new package set
  delete     - delete a package set
  use        - select where gb and goinstall target and link
  empty      - remove all code and compiled binaries from package set
  list       - list installed go packages
```

### 安装IDE

Go语言开发推荐使用[Visual Studio Code](https://code.visualstudio.com/)或[Goland](https://www.jetbrains.com/go/)，具体安装方法可参考对应官方指南。

如果你选择Visual Studio Code，需要额外安装支持Go语言等相关插件

![go-plugins](/images/go-getting-started/go-vscode-plugins.png)

## 重要参考书籍资料

[Effective Go中文版](https://www.kancloud.cn/kancloud/effective/72199)

以上就绪，接下来就可以写Go的代码了。