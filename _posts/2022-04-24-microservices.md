---
layout: post
subtitle: Spring Boot with Spring Cloud
gh-repo: ittarvin/micro-services
gh-badge: [star, fork, follow]
tags: [Spring Cloud]
comments: true
cover-img: /assets/img/IMG_1615.JPG
thumbnail-img: /assets/img/Spring应用分层.png
share-img: /assets/img/IMG_1482.JPG
---
This is a proof-of-concept application which demonstrates Microservices Architecture Pattern use Spring Boot and Spring Cloud.

# 微服务架构体系
## 1.架构图及技术栈
![微服务架构图.png](/assets/img/微服务架构图.png)

| 中间件 |
| --- | 
|  Nginx|  
|  Spring Cloud Gateway|  
|  NetFlix Eureka|  
|  携程 Apollo|  
|  Spring Boot|  
|  Alibaba Druid|  
|  Mysql|  
|  |  

## 2.微服务介绍
### 2.1.项目代码管理目录结构
![](/assets/img/59145CF4-D2FF-49ff-84F2-3AC4C76695E3.png)

> 项目采用Maven进行项目代码管理。

#### 2.1.1.代码结构介绍

- zjcw-gateway
>服务网关服务
- zjcw-${占位符}-micro-services
> 按模块划分的微服务
- zjcw-notifcation
>通知服务（短信，钉钉，推送等）
- zjcw-statistics
> 定时任务执行服务
- zjcw-oauth（待定）
> 微服务的安全体系
- zjcw-plugin
> 插件管理
- zjcw-utils
> 公共工具类


### 3.服务网关（Gateway）
>Gateway 作为更底层的微服务网关，通常是作为外部 Nginx 网关和内部微服务系统之间的桥梁，起了这么一个承上启下的作用。

#### 3.1.简介
>网关服务基于Spring Cloud Gateway 实现。实现功能包括 请求路由，服务负载，请求降级等。（[文档](https://spring.io/projects/spring-cloud-gateway)）


#### 3.1.2.扩展
- 实现基于Apollo的路由动态配置更新 。
- 集成车旺链路追踪（Zipkin）。

### 4.业务微服务（zjcw-${}-micro-services）
#### 4.1.微服务应用分层
![](/assets/img/Spring应用分层.png)
> **Controler** 业务请求控制层【不包括业务逻辑】
> **Service**  服务层【与 domain 共同实现业务逻辑，主要担任 domain 层的业务协调工作】
> **Domain** 层负责关键业务计算
> **Proxy** 服务代理与其他服务的交互（如：Fegin Client）
> **DAO** 数据库持久层（Mybatis）

#### 4.2.微服务应用测试

##### 4.2.1.测试覆盖原则

![](/assets/img/测试金字塔.png)

- 单元测试（UT）
  ![](/assets/img/单元测试.png)
> 使用技术：Junit，Spring Mock MVC，[Mpckito](https://site.mockito.org)
- 集成测试（IT）
  ![](/assets/img/集成测试.png)
> 主要针对外部依赖进行测试。
- 组件测试（CT）
  ![](/assets/img/组件测试.png)
>  内部 Mock **AND** 外部 Mock
>  内部：Spring MockBean，外部工具 [WireMock](http://wiremock.org)
>  外部： [hoverfly](https://hoverfly.io)
>  将外部依赖进行 Mock
- 端到端（End-to-End Test）
> API 测试工具 [Rest-assured](http://rest-assured.io)
- 契约测试（不推荐）
> 国内应用较少

##### 4.2.2.测试收益

| 分类 |功能  |
| --- | --- |
| 单元测试 | 保证类，模块功能的正常 |
| 集成测试 | 确保组件之间接口，交互，链路的正常 |
| 组件测试 | 确保服务作为独立整体，接口的功能正常 |
| 端到端测试 | 确保整个引用满足用户的需求 |
| 契约测试（不推荐） | 确保服务提供方和消费放都遵守契约规范（成本较高） |

