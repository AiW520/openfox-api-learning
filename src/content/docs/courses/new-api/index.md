---
title: "00 · 先分清两类令牌"
sidebar:
  order: 0
---

> **阅读版本：** 本课程含本地未提交修改，请先查看[源码版本与远端差异](../../source-version/)。


> **学完能做什么：** 解释控制台管理与模型转发的不同入口，沿三条流程找到源码。

## 必要概念

API 网关（API gateway）统一接入上游服务；控制台身份和用于调用模型的 API key 承担不同职责，不能互换。

## 项目解决什么问题

用户在 Web 控制台管理访问令牌、额度和模型接入；应用程序通过兼容 API 调用上游模型。本课程以令牌的查询、创建、删除作为可控的入口，再介绍转发边界。典型读者是要维护模型接入服务的 Web/Go 开发者。先修知识：HTTP、React 状态、Go 函数与 SQL 条件查询。

## 阅读顺序

最短：本章 → 快速上手 → 三条流程 → 边界测试。深入：前端请求封装 → 路由认证 → controller 校验 → model 查询 → 数据迁移 → 转发与配额 → 版本差异。所有课程命令都应在独立副本和测试数据库中运行。

## 版本组织

本课合并 new-api 与 OpenFox 构建目录源码。2429 个共同路径中 2365 个哈希相同、64 个变化；构建目录无 Git 提交，按文件快照定位。

这里的数量来自排除依赖、产物和敏感内容后的源码清单，不代表逐文件语义审查。改动文件清单与版本哈希见[覆盖报告](../../coverage/)。不同副本仅共享课程框架，差异页单独列出。

## 源码定位

- [tokenRoute · router/api-router.go:301–314](../../evidence/new-api-routes/)
- [GetAllTokens · controller/token.go:132–144](../../evidence/new-api-list/)
- [scripts / dependencies · web/package.json:1–35](../../evidence/new-api-manifest/)

## 常见误区

同名文件或相同 module 就等于同一运行版本。实际行为要核对文件哈希和具体处理函数。

## 自检

控制台登录成功后，是否可以直接把任意管理凭据用作模型 API key？

<details>
<summary>展开参考思路</summary>

不能；控制台 UserAuth 与转发 TokenAuth 的信任边界不同，要沿对应中间件核对。

</details>
