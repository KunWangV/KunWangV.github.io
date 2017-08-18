---
title: login
date: 2017-08-18 10:55:28
tags: login
---
# 微服务下登陆方式

> 参考 Email: david.borsos@opencredo.com

## SSO 单点登陆

角色：客户端、app服务器、认证服务器
流程：

- 1、客户端请求
- 2、app服务器检查local用户状态，没有登陆，重定向到认证服务器
- 3、用户到认证服务器登陆，登陆成功返回token
- 4、用户使用token重新发起请求
- 5、app服务器向认证服务器校验token并请求user details.

问题：

- 用户登陆不透明
- SSO服务器单点故障
- 内部流量爆炸
- 每个交换机都需要SSO

## 分布式会话

角色：client,server,Authentication server，分布式会话中心
配置环境：微服务和认证服务器都需要连接到分布式的存储中心
流程：

- 1、用户请求
- 2、server发现没有登录，重定向到Auth Server
- 3、用户登录，a、把用户登录状态写入session store；b、设置session id 返回client
- 4、用户使用session id登录
- 5、server根据session id从 session store读取更新登录状态

## 客户端 token

角色：client, server, auth server
流程：

- 1、client 请求 server
- 2、server 发现没登陆重定向到 auth server
- 3、client 在auth server登录，获取token
- 4、client使用token请求server，server不跟auth server进行交互，server能自己解析token，能够校验token

### token 可以采用JWT的格式

#### JWT分为三段

- heaer
- body
- signature

#### JWT的特点

- 标准
- 简单
- 可扩展
- 支持各种签名（SHA、RSA）
- lib支持好
- 支持对称加密，公钥、私钥加密;

## 客户端 toke+API网关

角色：client、api gateway、token store、server、auth server

流程：

- 1、client 请求
- 2、没有认证转发的到 auth server
- 3、认证成功，返回token
- 4、网关转换token=>opaque
- 5、client 使用opaque token
- 6、网关转换opaque token=>token
- 7、server解析token

登出：
撤销在网关处的token

## 总结

![](/images/login-methods.png)