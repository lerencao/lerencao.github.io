---
title: 一个好的配置中心应该有什么
date: 2017-06-13T23:14:51+08:00
tags:
- confserver
---

### 服务端 ###

- 更改可以准实时同步到客户端，两种主要方式。
  - 配置中心推送到客户端，实现方式？
  - 客户端对配置中心轮询。
- 完善的管理功能。
  - 管理多个环境下多个服务的多个版本的配置。
  - 提供 Typed Config Properties。
  - 提供 USER 管理，GROUP 管理。不同 GROUP/USER 看到的是不同的环境和服务配置。
  - 最好，配置中心可以和服务发现机制相结合，能够提供更多的功能。比如说：
    灰度发布，A/B test，流量控制 等等。
- 能够知晓当前服务的状况。
  比如说有某个服务有多少个客户端。
  客户端使用的事哪个版本的配置。
- 能够知晓配置更新的状态。比如，
  新的配置在哪些客户端生效了，哪些客户端没有响应，没响应的客户端要不要重新推送配置。
- HA ?
  - HA 主要影响的是配置中心到底需要使用什么数据库。
    比如说 MySQL, ETCD, ZK，或者其他。

### 客户端 ###

- 可能需要针对不同语言实现不同的客户端。
- 客户端启动的时候，将自己注册到配置中心，表明自己对哪个服务的那个版本的配置感兴趣。
- 然后拉取最新的配置，缓存到本地内存。
- 保持和配置中心的连接，发送心跳信息，（TCP，或者HTTP WebSocket, 或者 Long Polling ）
- 配置中心有更新推送过来的时候，更新本地内存。
- optional，定时去配置中心拉取配置。




### 调研 ###


- QConf: zk
- DisConf: zk
- Spring Cloud Config: 基于 git/file/vault
- Apollo: 携程开源的基于 eureka
- Netflix/archaius: a java library support config dynamic changed from external sources, like zk, etcd
