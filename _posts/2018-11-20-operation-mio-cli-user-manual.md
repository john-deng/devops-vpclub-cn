---
ID: 365
post_title: 'mio应用部署操作手册'
post_name: operation-mio
author: 邓冰寒
post_date: 2018-11-20 12:00:00
layout: post
link: >
  https://devops.vpclub.cn/operation-mio/
published: true
tags:
  - mio
  - CI/CD
categories:
  - 运维

---

## 摘要

使用mio来管理 pipeline。


### 参见

| 命令       | 描述     | 示例       |
|------------|----------|------------|
| mio info | 查看客户端连接配置信息 | mio info |
| mio set | 设置客户端连接配置信息| mio set --name={name} --token={token} --server={server_url} |
| mio login | 登陆客户端 | mio login |
| mio run | 启动 pipeline 任务 | mio run --project={namspaces} --app={app_name} --sourcecode={java;nodejs;golang} |
| mio logs | 查看 pipeline 任务日志 | mio logs --project={namspaces} --app={app_name} --sourcecode={java;nodejs;golang} |


```
一句话的使用说明
客户端配置设置用[mio set]查看配置用[mio info]先登陆[mio login]才能使用[mio run]启动pipeline 任务,查看日志用[mio logs]
不在项目路径下[mio run]和[mio logs]需要加参数。
```


### 名词解释:
| 属性       | 描述     | 示例       |
|------------|----------|------------|
|project|集群中的namespace，openshift中的Projects| -p demo |
|app|项目名称|-a hello-world|
|sourcecode|代码类型|-s java|
|token|Pipeline任务启动的验证信息，通过[mio login]命令自动写入配置文件| -t XXXXX|
|server|服务端连接地址|-s http://1.1.1.1:80|
|watch|是否实时显示Pipeline任务日志信息|-w |


### 例子：

以下例子分别用全称和简写展示
```bash
设置客户端server_url信息

mio set --server=http://100.100.100.100:8080
mio set -s http://100.100.100.100:8080
---客户端输出---:
______  ____________             _____
___  / / /__(_)__  /_______________  /_
__  /_/ /__  /__  __ \  __ \  __ \  __/
_  __  / _  / _  /_/ / /_/ / /_/ / /_     Hiboot Application Framework
/_/ /_/  /_/  /_.___/\____/\____/\__/     https://hidevops.io/hiboot


[INFO] Set successful.
```


```bash
同时设置客户端token和name信息
mio set --token=XXXXXX --name=zhangshan
mio set -t XXXXXX -n zhangshan
---客户端输出---:
______  ____________             _____
___  / / /__(_)__  /_______________  /_
__  /_/ /__  /__  __ \  __ \  __ \  __/
_  __  / _  / _  /_/ / /_/ / /_/ / /_     Hiboot Application Framework
/_/ /_/  /_/  /_.___/\____/\____/\__/     https://hidevops.io/hiboot


[INFO] Set successful.
```


```bash
查看客户端配置信息

mio info
---客户端输出---:
______  ____________             _____
___  / / /__(_)__  /_______________  /_
__  /_/ /__  /__  __ \  __ \  __ \  __/
_  __  / _  / _  /_/ / /_/ / /_/ / /_     Hiboot Application Framework
/_/ /_/  /_/  /_.___/\____/\____/\__/     https://hidevops.io/hiboot

/Users/wang/.mio/config :  {"Name":"","Token":"eyJpkx8PWpw*******","Server":"http://100.100.100.100:8080"}
```


```bash
登陆mio[此操作会更新Token信息，使用gitlab账号信息登陆]

mio login
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


```
启动 pipeline 任务
在项目代码路径下执行
mio run
不在项目代码路径下执行需要加参数
mio run --project=demo --app=hello-world --sourcecode=java
mio run -p demo -a hello-world -s java
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


```
启动 pipeline 任务 并实时输出Pipeline任务日志信息
在项目代码路径下执行
mio run --watch
mio run -w
不在项目代码路径下执行需要加参数
mio run --project=demo --app=hello-world --sourcecode=java --watch
mio run -p demo -a hello-world -s java -w
---客户端输出---:
______  ____________             _____
___  / / /__(_)__  /_______________  /_
__  /_/ /__  /__  __ \  __ \  __ \  __/
_  __  / _  / _  /_/ / /_/ / /_/ / /_     Hiboot Application Framework
/_/ /_/  /_/  /_.___/\____/\____/\__/     https://hidevops.io/hiboot

[INFO] 2018/11/20 11:03 Resolving dependencies
[INFO] 2018/11/20 11:03 Injecting dependencies
[INFO] 2018/11/20 11:03 Injected dependencies
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


```
查看 pipeline 任务日志
在项目代码路径下执行
mio logs
不在项目代码路径下执行需要加参数
mio logs --project=demo --app=hello-world --sourcecode=java
mio logs -p demo -a hello-world -s java
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
```



