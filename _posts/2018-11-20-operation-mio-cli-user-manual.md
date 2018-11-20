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

* mio info  – 查看客户端连接配置信息
* mio set   - 设置客户端连接配置信息
* mio login - 登陆客户端
* mio run   - 启动 pipeline 任务
* mio logs  - 查看 pipeline 任务日志

```
一句话的使用说明
客户端配置设置用[mio set]查看配置用[mio info]先登陆[mio login]才能使用[mio run]启动pipeline 任务,查看日志用[mio logs]
不在项目路径下[mio run]和[mio logs]需要加参数。
```


```bash
mio set --name={name} --token={token} --server={server_url}
mio info
mio login
mio run --project={namspaces} --app={app_name} --sourcecode={java|nodejs|goland}
mio logs --project={namspaces} --app={app_name} --sourcecode={java|nodejs|goland}
```

### 名词解释:

* [token]       Pipeline任务启动的验证信息，通过[mio login]命令自动写入配置文件
* [server]        服务端连接地址
* [project]     集群中的namespace，openshift中的Projects
* [app]         项目名称
* [watch]       是否实时显示Pipeline任务日志信息
* [sourcecode]  代码类型


### 例子：

以下例子分别用全称和简写展示
```
设置客户端server_url信息

mio set --server=http://100.100.100.100:8080
mio set -s http://100.100.100.100:8080
```


```
同时设置客户端token和name信息
mio set --token=XXXXXX --name=zhangshan
mio set -t XXXXXX -n zhangshan
```


```
查看客户端配置信息

mio info
[root@123 ~]#{"Name":"sd","Token":"XXXXXX","Host":"http://100.100.100.100:8080"}
```


```
登陆mio[此操作会更新Token信息，使用gitlab账号信息登陆]
mio login
```


```
启动 pipeline 任务
在项目代码路径下执行
mio run
不在项目代码路径下执行需要加参数
mio run --project=demo --app=hello-world --sourcecode=java
mio run -p demo -a hello-world -s java
```


```
启动 pipeline 任务 并实时输出Pipeline任务日志信息
在项目代码路径下执行
mio run --watch
mio run -w
不在项目代码路径下执行需要加参数
mio run --project=demo --app=hello-world --sourcecode=java --watch
mio run -p demo -a hello-world -s java -w

```


```
查看 pipeline 任务日志
在项目代码路径下执行
mio logs
不在项目代码路径下执行需要加参数
mio logs --project=demo --app=hello-world --sourcecode=java
mio logs -p demo -a hello-world -s java
```



