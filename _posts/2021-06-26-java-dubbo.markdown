---
layout: post
title: "Java Dubbo服务架构"
img: dubbo.jpg # Add image post (optional)
date: 2021-06-26 12:50:00 +0300
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
tag: [Java, Dubbo]
---

<a name="1zcca"></a>
## 一、概述
<a name="K4gN1"></a>
#### （一）定义
阿里巴巴公司开源的一个高性能、轻量级的开源 Java RPC（远程过程调用）服务框架，是一种**分布式服务框架，**使得应用可通过高性能的 RPC 实现服务的输出和输入功能，可以和 Spring 框架无缝集成（采用全Spring配置）。<br />简单的说就是一个远程服务调用的分布式框架，只有在有分布式需求的时候，才需要用到dubbo服务框架。

<a name="hDH0Y"></a>
#### （二）发展过程
在互联网的发展中，后端架构的演变过程:

1. 单一应用架构（一个服务器，程序全部打包好就可以）
1. 应用和数据库单独部署
1. 应用和数据库集群部署
1. 数据库压力变大，读写分离
1. 使用缓存技术加快速度
1. 数据库分库分表
1. 应用分为不同的类型拆分（例如：pc端网站，微信端网站，后台服务，接口服务）



<a name="kDjn4"></a>
#### （三）存在问题

- 当服务越来越多时，服务URL配置管理变得非常困难，F5硬件负载均衡器的单点压力也越来越大。
- 当进一步发展，服务间依赖关系变得错踪复杂，甚至分不清哪个应用要在哪个应用之前启动，架构师都不能完整的描述应用的架构关系。
- 接着，服务的调用量越来越大，服务的容量问题就暴露出来，这个服务需要多少机器支撑？什么时候该加机器？



<a name="4GDXe"></a>
## 二、核心功能
<br />
> 三大核心功能如下：

**① 面向接口的远程方法调用**（远程通讯）：提供对多种基于长连接的NIO框架抽象封装<br />**② 智能容错和负载均衡**（集群容错）：提供基于接口方法的透明远程过程调用<br />**③ 以及服务自动注册和发现**（自动发现）： 基于注册中心目录服务，使消费方能动态的查找服务提供方的地址

<a name="lNrdB"></a>
## 三、结构组成



| 节点 | 角色说明 |
| :---: | :---: |
| Provider | 暴露服务的服务提供方 |
| Consumer | 调用远程服务的服务消费方 |
| Registry | 服务注册与发现的注册中心 |
| Monitor | 统计服务的调用次数和调用时间的监控中心 |
| Container | 服务运行容器 |

![](https://img-blog.csdnimg.cn/img_convert/d5694df92930f0f379f6e730f3da010d.png#align=left&display=inline&height=453&margin=[object Object]&originHeight=824&originWidth=1060&size=0&status=done&style=none&width=583)<br />其实Dubbo的架构很简单，就像生产者-消费者模型，只是在这种模型上，加上了注册中心和监控中心，用于管理提供方提供的url，以及管理整个过程。
<a name="Ff20K"></a>
### 调用执行过程：
<br />

- 整个发布-订阅的过程：
   1. 服务容器负责启动，加载，运行服务提供者。
   1. 服务提供者在启动时，向注册中心注册自己提供的服务。
   1. 服务消费者在启动时，向注册中心订阅自己所需的服务。



- 如果考虑失败或变更的情况，就需要考虑下面的过程：
   1. 注册中心返回服务提供者地址列表给消费者，如果有变更，注册中心将基于长连接推送变更数据给消费者。
   1. 服务消费者，从提供者地址列表中，基于软负载均衡算法，选一台提供者进行调用，如果调用失败，再选另一台调用。
   1. 服务消费者和提供者，在内存中累计调用次数和调用时间，定时每分钟发送一次统计数据到监控中心。


<br />
<br />*注：基于TCP/IP的**长连接**和**短连接**解释

- **长连接：**长连接，也叫持久连接，在TCP层握手成功后，**不立即**断开连接，在此连接的基础上进行多次消息交互，直至连接的 任意一方 主动断开连接，此过程称为一次完整的长连接。
- **短连接**：客户端收到服务端的响应后，**立刻发送FIN消息**，主动释放连接。也有服务端主动断连的情况，凡是在一次消息交互之后立刻断开连接的情况都称为短连接。



**频繁交互**的场景使用长连接，如即时通信工具（微信/QQ等），**单次交互**的则使用短连接，比如普通的web网站，只有当浏览器发起请求时才会建立连接，服务器返回相应后，连接立即断开。维持长连接会有一定的系统开销，用户量上去，有可能把服务器资源耗尽。<br />

