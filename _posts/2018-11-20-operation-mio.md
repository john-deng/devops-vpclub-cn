---
ID: 365
post_title: mio应用部署操作手册
post_name: operation-mio
author: 邓冰寒
post_date: 2018-11-20 12:00:00
layout: post
link: https://devops.vpclub.cn/operation-mio/
published: true
tags:
  - CI/CD
  - mio
categories:
  - 运维
  - 运维
---

## 摘要

使用mio来管理 pipeline。

### 安装

[点击下载](http://stage.vpclub.cn/file/hidevopsio/mio-cli/)相应版本的客户端后，拷贝到env任意PATH路径中即可


### 子命令

| 子命令       | 描述     | 示例       |
|------------|----------|------------|
| info | 查看客户端连接配置信息 | mio info |
| set | 设置客户端连接配置信息| mio set --name={name} --token={token} --server={server_url} |
| login | 登陆客户端 | mio login |
| run | 启动 pipeline 任务 | mio run --project={namspaces} --app={app_name} --sourcecode={java;nodejs;golang} --profile={test;dev;prod} --version={v1,v2}|
| logs | 查看 pipeline 任务日志 | mio logs --project={namspaces} --app={app_name} --sourcecode={java;nodejs;golang} --profile={test;dev;prod} --version={v1,v2}|

```
在项目路径下执行mio run和mio logs 可以自动推断app,project,sourcecode属性 直接执行mio run或mio logs即可
```


### 属性介绍:
| 属性       | 描述     | 示例       |
|------------|----------|------------|
|project|集群中的namespace，openshift中的Projects| -p demo |
|app|项目名称|-a hello-world|
|sourcecode|代码类型|-s java|
|token|Pipeline任务启动的验证信息，通过[mio login]命令自动写入配置文件| -t XXXXX|
|server|服务端连接地址|-s http://1.1.1.1:80|
|watch|是否实时显示Pipeline任务日志信息|-w |
|profile|指定发版环境(可选参数默认值为 dev)|-P test |
|version|指定版本信息(可选参数默认值为 v1)|-V v2|


### 例子：

以下例子分别用全称和简写展示

##### 设置客户端server_url信息
http://100.100.100.100:8080 需换成真实URL

```bash
mio set --server=http://100.100.100.100:8080
```

```bash
mio set -s http://100.100.100.100:8080
```

---客户端输出---:
```bash
______  ____________             _____
___  / / /__(_)__  /_______________  /_
__  /_/ /__  /__  __ \  __ \  __ \  __/
_  __  / _  / _  /_/ / /_/ / /_/ / /_     Hiboot Application Framework
/_/ /_/  /_/  /_.___/\____/\____/\__/     https://hidevops.io/hiboot


[INFO] Set successful.
```



##### 同时设置客户端token和name信息
```bash
mio set --token=XXXXXX --name=zhangshan
```
```bash
mio set -t XXXXXX -n zhangshan
```
---客户端输出---:
```bash
______  ____________             _____
___  / / /__(_)__  /_______________  /_
__  /_/ /__  /__  __ \  __ \  __ \  __/
_  __  / _  / _  /_/ / /_/ / /_/ / /_     Hiboot Application Framework
/_/ /_/  /_/  /_.___/\____/\____/\__/     https://hidevops.io/hiboot


[INFO] Set successful.
```



##### 查看客户端配置信息
```bash
mio info
```
```bash
---客户端输出---:
______  ____________             _____
___  / / /__(_)__  /_______________  /_
__  /_/ /__  /__  __ \  __ \  __ \  __/
_  __  / _  / _  /_/ / /_/ / /_/ / /_     Hiboot Application Framework
/_/ /_/  /_/  /_.___/\____/\____/\__/     https://hidevops.io/hiboot
/Users/wang/.mio/config :  {"Name":"","Token":"eyJpkx8PWpw*******","Server":"http://100.100.100.100:8080"}
```


##### 登陆mio
[此操作会更新Token信息，使用gitlab账号信息登陆]
```bash
mio login
```
```bash
---客户端输出---:
______  ____________             _____
___  / / /__(_)__  /_______________  /_
__  /_/ /__  /__  __ \  __ \  __ \  __/
_  __  / _  / _  /_/ / /_/ / /_/ / /_     Hiboot Application Framework
/_/ /_/  /_/  /_.___/\____/\____/\__/     https://hidevops.io/hiboot

Username zhangshan
Password ********
[INFO] Login successful.
```


##### 启动 pipeline 任务
在项目代码路径下执行
```bash
mio run
```

在项目代码路径下启动pipeline并且自定义[发版环境]与[版本信息]
```bash
mio run --profile=test --version=v2
mio run -P test -V v2
```

不在项目代码路径下执行需要加参数
```bash
mio run --project=demo --app=hello-world --sourcecode=java
mio run -p demo -a hello-world -s java
```

不在项目代码路径下启动pipeline并且自定义[发版环境]与[版本信息]
```bash
mio run --project=demo --app=hello-world --sourcecode=java --profile=test --version=v2
mio run -p demo -a hello-world -s java -P test -V v2
```

```bash
---客户端输出---:
______  ____________             _____
___  / / /__(_)__  /_______________  /_
__  /_/ /__  /__  __ \  __ \  __ \  __/
_  __  / _  / _  /_/ / /_/ / /_/ / /_     Hiboot Application Framework
/_/ /_/  /_/  /_.___/\____/\____/\__/     https://hidevops.io/hiboot

app:  hello-world
project:  demo
source code:  java
[INFO] 2018/11/20 11:01 [INFO] pipeline start succeed
```



##### 启动 pipeline 任务 并实时输出Pipeline任务日志信息
在项目代码路径下执行
```bash
mio run --watch
mio run -w
```

不在项目代码路径下执行需要加参数
```bash
mio run --project=demo --app=hello-world --sourcecode=java --watch
mio run -p demo -a hello-world -s java -w
```

```bash
---客户端输出---:
______  ____________             _____
___  / / /__(_)__  /_______________  /_
__  /_/ /__  /__  __ \  __ \  __ \  __/
_  __  / _  / _  /_/ / /_/ / /_/ / /_     Hiboot Application Framework
/_/ /_/  /_/  /_.___/\____/\____/\__/     https://hidevops.io/hiboot

app:  hello-world
project:  demo
source code:  java
[INFO] 2018/11/20 11:03 [INFO] pipeline start succeed
Event Type:ADDED Namespace:demo Pod:hello-world-v2-108-55b8549647-kt9bd Status:Pending
Event Type:MODIFIED Namespace:demo Pod:hello-world-v2-108-55b8549647-kt9bd Status:Running
[DBUG] 2018/11/20 03:03 Host: register server shutdown on interrupt(CTRL+C/CMD+C)
[INFO] clone https://gitlab.vpclub.cn/demo/hello-world.git start:
[INFO] clone https://gitlab.vpclub.cn/demo/hello-world.git succeed
....
Downloaded: http://nexus.vpclub.cn/repository/maven-public/org/eclipse/sisu/sisu-inject/0.0.0.M5/sisu-inject-0.0.0.M5.pom (14 KB at 27.6 KB/sec)
Downloading: http://nexus.vpclub.cn/repository/maven-public/org/codehaus/plexus/plexus-classworlds/2.4/plexus-classworlds-2.4.pom
Downloaded: http://nexus.vpclub.cn/repository/maven-public/org/codehaus/plexus/plexus-classworlds/2.4/plexus-classworlds-2.4.pom (4 KB at 118.5 KB/sec)
Downloading: http://nexus.vpclub.cn/repository/maven-public/org/apache/maven/maven-model-builder/3.1.1/maven-model-builder-3.1.1.pom
Downloaded: http://nexus.vpclub.cn/repository/maven-public/org/apache/maven/maven-model-builder/3.1.1/maven-model-builder-3.1.1.pom (3 KB at 105.5 KB/sec)
Downloading: http://nexus.vpclub.cn/repository/maven-public/org/apache/maven/maven-aether-provider/3.2.1/maven-aether-provider-3.2.1.pom
Downloaded: http://nexus.vpclub.cn/repository/maven-public/org/apache/m

```



##### 查看 pipeline 任务日志
在项目代码路径下执行
```bash
mio logs
```
不在项目代码路径下执行需要加参数
```bash
mio logs --project=demo --app=hello-world --sourcecode=java
mio logs -p demo -a hello-world -s java
```
```bash
---客户端输出---:
______  ____________             _____
___  / / /__(_)__  /_______________  /_
__  /_/ /__  /__  __ \  __ \  __ \  __/
_  __  / _  / _  /_/ / /_/ / /_/ / /_     Hiboot Application Framework
/_/ /_/  /_/  /_.___/\____/\____/\__/     https://hidevops.io/hiboot

Event Type:ADDED Namespace:demo Pod:hello-world-v2-108-55b8549647-kt9bd Status:Pending
Event Type:MODIFIED Namespace:demo Pod:hello-world-v2-108-55b8549647-kt9bd Status:Running
[DBUG] 2018/11/20 03:03 Host: register server shutdown on interrupt(CTRL+C/CMD+C)
[INFO] clone https://gitlab.vpclub.cn/demo/hello-world.git start:
[INFO] clone https://gitlab.vpclub.cn/demo/hello-world.git succeed
....
Downloaded: http://nexus.vpclub.cn/repository/maven-public/org/eclipse/sisu/sisu-inject/0.0.0.M5/sisu-inject-0.0.0.M5.pom (14 KB at 27.6 KB/sec)
Downloading: http://nexus.vpclub.cn/repository/maven-public/org/codehaus/plexus/plexus-classworlds/2.4/plexus-classworlds-2.4.pom
Downloaded: http://nexus.vpclub.cn/repository/maven-public/org/codehaus/plexus/plexus-classworlds/2.4/plexus-classworlds-2.4.pom (4 KB at 118.5 KB/sec)
Downloading: http://nexus.vpclub.cn/repository/maven-public/org/apache/maven/maven-model-builder/3.1.1/maven-model-builder-3.1.1.pom
Downloaded: http://nexus.vpclub.cn/repository/maven-public/org/apache/maven/maven-model-builder/3.1.1/maven-model-builder-3.1.1.pom (3 KB at 105.5 KB/sec)
Downloading: http://nexus.vpclub.cn/repository/maven-public/org/apache/maven/maven-aether-provider/3.2.1/maven-aether-provider-3.2.1.pom
Downloaded: http://nexus.vpclub.cn/repository/maven-public/org/apache/m
....
[INFO] image [docker-registry-default.app.vpclub.io/demo/hello-world:112] start build:
Step 1/9 : FROM docker.vpclub.cn/hidevopsio/java:8-jre-alpine
Step 2/9 : ENV TZ Asia/Shanghai
Removing intermediate container bbe2cd220d48
Step 3/9 : ENV APP_OPTIONS -Xms128m -Xmx512m -Xss512k
Removing intermediate container 543fa75cbecb
Step 4/9 : ENV APP_OPTIONS -Xms128m -Xmx512m -Xss512k
Removing intermediate container 9ea66006f4ac
Step 5/9 : COPY app.jar /root
Removing intermediate container ce03333a3e6a
Step 6/9 : EXPOSE 8080
Removing intermediate container f4bfa3d78d68
Step 7/9 : EXPOSE 7575
Removing intermediate container ae94478598a0
Step 8/9 : RUN ls -l
Removing intermediate container 3bfc2b932633
Step 9/9 : ENTRYPOINT sh -c java -jar /root/app.jar $APP_OPTIONS
Removing intermediate container 4cc3d3e88254
Successfully built 57e68859d1c0
[INFO] image [docker-registry-default.app.vpclub.io/demo/hello-world:112] start push:
[INFO] 2018/11/20 04:54 image push
status:The push refers to a repository [docker-registry-default.app.vpclub.io/demo/hello-world]
status:Preparing,progressDetail:,id:69f1666a0d89
status:Preparing,progressDetail:,id:44e3d4bd3ac6
status:Preparing,progressDetail:,id:20dd87a4c2ab
status:Preparing,progressDetail:,id:78075328e0da
status:Preparing,progressDetail:,id:9f8566ee5135
status:Layer already exists,progressDetail:,id:78075328e0da
status:Layer already exists,progressDetail:,id:9f8566ee5135
status:Layer already exists,progressDetail:,id:44e3d4bd3ac6
status:Layer already exists,progressDetail:,id:20dd87a4c2ab
status:Pushed,progressDetail:,id:69f1666a0d89
status:112: digest: sha256:712f30225f49fd8ed9191ab505acb802835ab07cacb3e485c4a94db4380d73d4 size: 1370
{"progressDetail":{},"aux":{"Tag":"112","Digest":"sha256:712f30225f49fd8ed9191ab505acb802835ab07cacb3e485c4a94db4380d73d4","Size":1370}}

```



