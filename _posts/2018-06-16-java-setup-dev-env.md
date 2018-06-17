---
ID: 254
post_title: Java开发环境搭建
post_name: java-setup-dev-env
author: 邓冰寒
post_date: 2018-06-16 22:00:00
layout: post
link: >
  https://devops.vpclub.cn/java-setup-dev-env/
published: true
tags:
  - Development Environment
  - Java
categories:
  - Java
---

# Java开发环境搭建

俗话说的好，工欲善其事，必先利其器，一个好的工具是Java开发的一大利器，我们选择社区使用最广泛的代码管理工具Git和集成开发工具IDEA IDE。

## 安装JDK

* [下载 JDK 1.8](https://stage.vpclub.cn/file/jdk/jdk-8u152-windows-x64.exe) （我们采用Spring Boot来开发微服务，JDK最低要求是1.8版本）
* 安装JDK 安装在安装官方指引安装即可，安装完毕确认以下几点:
    * 设置环境变量JAVA_HOME
    * 在终端下确认Java的版本

```bash
java -version
  
java version "1.8.0_31"
Java(TM) SE Runtime Environment (build 1.8.0_31-b13)
Java HotSpot(TM) 64-Bit Server VM (build 25.31-b07, mixed mode)
```  

## 安装GIT
  
* Windows用户选择Windows版本，假设你的Windows是64位的，那么[点击下载 -> Git for Windows SDK](https://stage.vpclub.cn/file/git/git-sdk-installer-1.0.6-64.7z.exe)

    下载好Git后，运行安装包，设置Git安装路径为：C:\git， 等待Git自动安装完毕。

* Mac用户可以使用Homebrew在终端下安装：

1. 先安装 [Homebrew](http://brew.sh/index_zh-cn.html)
```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

2. 再安装 git

```bash
brew install git
```

* Linux用户：

* Ubuntu:

```bash
sudo apt-get install git
```

* Fedora:

```bash
sudo yum -y install git
```

## 安装集成开发环境 IDE

* [下载 IDEA IDE](https://www.jetbrains.com/idea/download)
  ![downloads](/images/java-setup-dev-env/idea-downloads.png) (要求 IDEA IDE 2016.3.0 或以上版本)
* 安装

IDEA IDE 的安装较为简单，确实安装就行了。

* 配置
  
IDEA IDE 安装完毕后，你需要配置以下环境，(Windows和Linux用户打开下拉菜单 File -> Settings, Mac 用户打开下拉菜单 IntelliJ IDEA -> Preferences)

Maven, IDEA IDE安装成功后，已经包含了Maven，可以直接使用，也可以自己[下载 Maven](apache-maven-3.5.2-bin.zip)自定义安装。

![idea-maven](/images/java-setup-dev-env/idea-maven.png)

安装插件 (**protobuf**和**Lombok** 必须安装)

* 在IDEA系统设置搜索框输入Pluggins打开插件管理

![idea-plugins](/images/java-setup-dev-env/idea-plugins.png)

* 在IDEA插件管理搜索框输入**protobuf**, 并点击右边的 Install 按钮

![idea-protobuf](/images/java-setup-dev-env/idea-protobuf.png)

* 重复上面的步骤，搜索SonarLint、**Lombok**并安装。
* 安装完毕按提示重启IDEA IDE即可生效。

## 配置Maven仓库

配置Maven仓库为公司私有仓库. 首先查看Maven工具安装路径

```bash
mvn -v
```

输出结果如下：

```bash

Apache Maven 3.3.9 (bb52d8502b132ec0a5a3f4c09453c07478323dc5; 2015-11-11T00:41:47+08:00)
Maven home: /usr/local/maven
Java version: 1.8.0_31, vendor: Oracle Corporation
Java home: /Library/Java/JavaVirtualMachines/jdk1.8.0_31.jdk/Contents/Home/jre
Default locale: en_US, platform encoding: UTF-8
OS name: "mac os x", version: "10.12.4", arch: "x86_64", family: "mac"
```

那么Maven配置文件的位置在 /usr/local/maven/conf

```bash
ls /usr/local/maven/conf
```

查看到 /usr/local/maven/conf下面有 settings.xml

```bash

logging              settings-default.xml settings.xml         toolchains.xml
```

从上面输出可以看出maven的配置文件settings.xml在/usr/local/maven/conf/下面

```bash
# 可选： 如果你没有仓库的账户，下面两项可以不设置
export MAVEN_NEXUS_USERNAME=your-nexus-username
export MAVEN_NEXUS_PASSWORD=your-nexus-password

# 必填：设置私有仓库URL
export MAVEN_PUBLIC_URL=https://nexus.example.com/repository/maven-public/
export MAVEN_RELEASES_URL=https://nexus.example.com/repository/maven-releases/
export MAVEN_SNAPSHOTS_URL=https://nexus.example.com/repository/maven-snapshots/
```

配置 

```xml
<settings>
    <servers>
        <server>
             <id>nexus-releases</id>
             <username>${env.MAVEN_NEXUS_USERNAME}</username>
             <password>${env.MAVEN_NEXUS_PASSWORD}</password>
         </server>
         <server>
             <id>nexus-snapshots</id>
             <username>${env.MAVEN_NEXUS_USERNAME}</username>
             <password>${env.MAVEN_NEXUS_PASSWORD}</password>
         </server>
    </servers>
    <mirrors>
        <mirror>
            <!--This sends everything else to /public -->
            <id>nexus</id>
            <mirrorOf>*</mirrorOf>
            <url>${env.MAVEN_PUBLIC_URL}</url>
        </mirror>
    </mirrors>
    <profiles>
        <profile>
            <id>nexus</id>
            <!--repository.url will be used in each project for simplicity-->
            <properties>
                <nexus.releases>${env.MAVEN_RELEASES_URL}</nexus.releases>
                <nexus.snapshots>${env.MAVEN_SNAPSHOTS_URL}</nexus.snapshots>
            </properties>
            <!--Enable snapshots for the built in central repo to direct -->
            <!--all requests to nexus via the mirror -->
            <repositories>
                <repository>
                    <id>central</id>
                    <url>https://central</url>
                    <releases><enabled>true</enabled></releases>
                    <snapshots><enabled>true</enabled></snapshots>
                </repository>
            </repositories>
            <pluginRepositories>
                <pluginRepository>
                    <id>central</id>
                    <url>https://central</url>
                    <releases><enabled>true</enabled></releases>
                    <snapshots><enabled>true</enabled></snapshots>
                </pluginRepository>
            </pluginRepositories>
        </profile>
    </profiles>
    <activeProfiles>
        <!--make the profile active all the time -->
        <activeProfile>nexus</activeProfile>
    </activeProfiles>
</settings>
```

## 安装浏览器

* 作为Java开发者，使用什么浏览器能提供你的工作效率呢，我推荐Chrome，或者以Chrome为内核的其它浏览器，用chrome的目的是里面有很多开发用的工具及插件，如Postman。

## 安装Bash终端

* 如果你是Windows用户，有两个方法来改善用户体验，提升你的工作效率
  [下载并安装 ConEmu](https://stage.vpclub.cn/file/conemu/ConEmuSetup.161206.exe), 替代内置终端 cmd.
