---
layout: post
title: "Java springboot框架"
img: spring.jpg # Add image post (optional)
date: 2021-06-10 12:50:00 +0300
description: You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. # Add post description (optional)
tag: [Java, 编程, spring]
---

**IOC（控制反转）：**
IOC容器具有依赖注入功能的容器，IOC容器负责实例化对象，及建立这些对象间的依赖，对象不需要由程序使用new，控制权由程序转向IOC容器，如Spring的BeanFactory。

**AOP（面向切面编程）：**
AOP代表的是一个横向的层与层的关系，将那些与业务无关，却为业务模块所共同调用的逻辑或责任封装起来，降低模块间的耦合度
**
**
**Mybatis：**
支持普通 SQL的高级映射持久层框架，MyBatis 消除了JDBC代码和参数的手工设置以及结果集的检索。MyBatis 使用简单的 XML或注解用于配置和原始映射

**Maven：**
Maven是个项目管理和构建自动化工具，使用它通过依赖来管理项目中或者远程仓库中的jar包，Maven 有以下三个标准的生命周期：

- **clean**：项目清理的处理
- **default(或 build)**：项目部署的处理

![](https://img-blog.csdnimg.cn/img_convert/8a7f3e29dabf878ba4942f8c11fca706.png#align=left&display=inline&height=285&margin=[object Object]&originHeight=285&originWidth=840&size=0&status=done&style=none&width=840)

- **site**：项目站点文档创建的处理



**Servlet:**
处理 HTTP 客户端请求和服务器数据库或应用程序之间的中间层，位于Controller层
**① **读取客户端（浏览器）发送的显式数据，
**② **读取客户端（浏览器）发送的隐式HTTP请求数据，
**③ **通过服务器底层处理数据并生成结果，
**④** 发送显式数据回客户端（HTML 或 XML文本、二进制文件、Excel 等）
**⑤** 发送隐式数据回客户端（ cookies 和缓存参数等）




**SpringMVC**：




**注解**：
@Component：表示注释的类是一个“组件”，成为Spring管理的Bean，可以被注入到spring容器进行管理。
@Controller：控制层注解，处理http请求的控制器
@Autowired：
@Repository：作用于持久层（数据访问层）
@Service：应用在service层（业务逻辑层）
@RequestMapping：配置url映射（@PostMapping 等同于 @RequestMapping(method = RequestMethod.POST)）
