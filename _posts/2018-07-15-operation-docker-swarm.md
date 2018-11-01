---
ID: 307
post_title: 小型项目的容器部署方案
post_name: operation-docker-swarm
author: 邓冰寒
post_date: 2018-07-15 13:19:58
layout: post
link: >
  https://devops.vpclub.cn/operation-docker-swarm/
published: true
tags:
  - Deployment
  - Docker
  - Simplified
  - Swarm
categories:
  - 运维
  - 运维
---

# 小型项目的容器部署方案

## 方案背景

容器和容器编排历经四年多的发展日益成熟，越来越多的企业将传统服务迁移到容器部署。容器编排方案首选Kubernetes，其优点不用多说，本来容器编排有Mesos, Docker Swarm和Kubernetes竞争，在去年Kubernetes已经取得了压倒性优势。

当然，Kubernetes需要相对较多的服务器才能部署到生产环境，这个成本对于小企业或者一些小项目来说成本有所偏高，因此，我们有必要采用一种简化版本的容器部署方案。我们采用Docker Swarm来解决这个问题。

## Docker Swarm方案特点

### 优点

* 服务器成本低
* 安装相当简单
* 支持有限手动水平扩展

### 缺点

由于是简化方案，故有其局限性。

* 所有操作需要通过命令行进行
* 没有自动化部署
* 没有持续集成和持续交付
* 需要投入更多人力，运营人力成本高
* 当服务增多到一定规模， 会给运营带来诸多困难
* 适合小项目使用，强烈建议不要用在核心大项目的生产环境

## 具体方案

### 最低要求

* **系统硬盘：** 40G或以上
* **外挂硬盘：** 200G或以上
* **内存容量：** 8G或以上
* **操作系统：** Centos／RedHat 7.2 或以上
* **容器版本：** Docker 1.13或以上版本，docker-compose v2 以上
* **服务器数量：** 根据服务的多少或业务量来定，最少5台（其中一台管理节点，一台ETCD服务发现和存储，三台服务器作为节点），如果服务较多，需要增加节点，如果服务需要的服务器需要12台以上，不建议使用本方案。

### 部署架构

![swarm-cluster](/images/operation-docker-swarm/docker-swarm.png)

Docker引擎CLI包含Swarm集群管理指令，如增加或删除节点。CLI也包含将服务部署到Swarm集群以及对服务编排进行管理的指令。

在Swarm模式外运行Docker引擎时，需执行容器命令。在Swarm模式下运行引擎时，则是对服务进行编排。

### 方案描述

> 方案描述参考 [docker官方文档](https://docs.docker.com/engine/swarm/)

本方案集群管理与Docker引擎相结合，使用Docker引擎命令行客户端便可创建一个Docker引擎的Swarm，在这个集群中进行应用服务的部署。对于Swarm集群的创建和管理，无需其他额外的编排软件

* Docker Engine 集成集群管理
  使用Docker Engine CLI 创建一个Docker Engine的Swarm模式，在集群中部署应用程序服务。
* 去中心化设计
  Swarm角色分为Manager和Worker节点，Manager节点故障不影响应用使用。
* 扩容和缩容
  可以声明每个服务运行的容器数量，通过添加或删除容器数自动调整期望的状态。
* 期望，状态与协调
  Swarm Manager节点不断监视集群状态，并调整当前状态与期望状态之间的差异。例如，设置一个服务运行10个副本容器，如果两个副本的服务器节点崩溃，Manager将创建两个新的副本替代崩溃的副本。并将新的副本分配到可用的worker节点。
* 多主机网络
  多主机网络可以为服务指定overlay网络。当初始化或更新应用程序时，Swarm manager会自动为overlay网络上容器分配IP地址。
* 服务发现
  Swarm manager节点为集群中的每个服务分配唯一的DNS记录和负载均衡VIP。可以通过Swarm内置的DNS服务器查询集群中每个运行的容器。
* 负载均衡 
  实现服务副本负载均衡，提供入口访问。也可以将服务入口暴露给外部负载均衡器再次负载均衡。
* 安全与传输
  Swarm中的每个节点使用TLS相互验证和加密，确保安全的其他节点通信。
* 滚动更新
  升级时，逐步将应用服务更新到节点，如果出现问题，可以将任务回滚到先前版本。使用swarm 要保证端口2377（manger和worker 之间的通信端口）tcp/udp 7946（worke 节点之间）和4789(overlay 网络之间的通信）
