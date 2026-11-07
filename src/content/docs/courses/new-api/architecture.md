---
title: "02 · 从页面到数据库的模块边界"
sidebar:
  order: 2
---

> **阅读版本：** 本课程含本地未提交修改，请先查看[源码版本与远端差异](../../../source-version/)。


> **学完能做什么：** 解释每层输入输出和令牌数据生命周期。

## 必要概念

分层（layering）把交互、协议处理和持久化职责分开；ORM（Object Relational Mapping）将对象操作映射为数据库查询，但并不自动补上对象授权。

## 架构图

![令牌管理执行链](/diagrams/new-api.svg)

[单独查看图示（可缩放）](/diagrams/new-api.svg)

图从页面向下追踪一次令牌管理操作；响应沿相反方向返回。图中的认证发生在 controller 之前，用户条件继续进入 model。不是浏览器直接访问数据库。

| 技术/模块 | 实际职责与协作 | 版本证据 |
| --- | --- | --- |
| React 前端 | TanStack Query 管理列表查询，页面上下文触发刷新，抽屉管理表单 | web/package.json 与前端证据 |
| Axios | 封装 HTTP 请求、身份信息和错误 | 请求实例源码与锁文件 |
| Gin | 路由注册、中间件、JSON 绑定、响应 | go.mod 与 router |
| GORM | model 内按 id/user_id 查询、插入和删除 | go.mod 与 model/token.go |
| 数据库 | 令牌、用户和配置的持久化；选型依配置 | model/main.go |

作者选型动机未见明确说明。分析推测：Go + 嵌入前端便于单进程交付，分离 React 便于复杂交互；代价是两套工具链。小控制台可用服务端模板减少构建复杂度，复杂交互则继续采用前后端分层。

## 数据层

一个用户拥有多个 Token；关键字段包括 Id、UserId、Name、Key、ExpiredTime、RemainQuota、UnlimitedQuota 和 Group。新令牌 UserId 来自认证上下文，不能信任请求中的用户编号。创建写入，列表按用户读取，密钥显示走专门接口，删除也带用户范围。

名称校验、额度判断、计数上限在 controller；数据库访问在 model。计数后插入是否在并发下严格满足总数限制，需要单独并发测试，不能仅从顺序代码推断。不要把数据库查询之外的内存/Redis 缓存假定为立即一致；本课没有穷尽全部缓存失效路径。

## 源码定位

- [useQuery · web/src/features/keys/components/api-keys-table.tsx:264–303](../../../evidence/new-api-front/)
- [api / inFlightGet · web/src/lib/http-client.ts:51–78](../../../evidence/new-api-client/)
- [GetTokenByIds · model/token.go:262–269](../../../evidence/new-api-ownership/)
- [AddToken · controller/token.go:299–380](../../../evidence/new-api-create/)

## 常见误区

所有权在前端隐藏按钮后就能保证。真正限制来自服务端身份与查询条件。

## 自检

为什么删除查询同时需要 id 和 user_id？

<details>
<summary>展开参考思路</summary>

id 定位对象，user_id 限制对象属于当前身份；只保留前者会扩大访问范围。

</details>
