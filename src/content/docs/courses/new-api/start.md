---
title: "01 · 环境与可控启动"
sidebar:
  order: 1
---

> **阅读版本：** 本课程含本地未提交修改，请先查看[源码版本与远端差异](../../../source-version/)。


> **学完能做什么：** 准备不会接触生产数据的副本，解释构建与启动失败的原因。

## 必要概念

嵌入资源（embedded assets）在 Go 编译时纳入二进制；前端产物缺失会在后端编译之前就报错。

## 版本证据与命令状态

当前主版本 go.mod 声明 Go 1.26.10。前端 React、TypeScript、Rsbuild 与 TanStack Query 的精确范围见 package.json，实际解析版本以锁文件为准。**本次未构建或启动两个网关**，因此下列命令是从入口和脚本核对的参考步骤。

1. 复制源码到新目录，排除 .env、数据库、日志与真实凭据；为该副本建立专用数据库。
2. 安装匹配 go.mod 的 Go 工具链，以及前端脚本需要的 Bun。保留锁文件。
3. 在 web 中安装并构建，再回根目录启动；不要跳过前端构建，因为 main.go 嵌入 web/dist。

```sh
cd web
bun install --frozen-lockfile
bun run build
cd ..
go run .
```

构建会执行项目自带检查脚本；实验前应阅读它们并限制在副本。Go/Bun 工具链未在本次验证，若某副本锁文件格式不同，先核对该版本的构建配置，不能删除锁文件强行升级。

## 配置与预期

SQL_DSN 指向专用数据库；SESSION_SECRET 使用实验新生成随机值，不能复用线上值。数据库选择与迁移从 model/main.go 开始读。new-api 的 InitDB 在主节点迁移，非主节点走另一分支；这不表示任意版本或数据库都已迁移成功。

启动后先检查控制台能加载，再创建实验用户、进入令牌页；预期空列表，创建后刷新出现一行。无需配置真实上游即可理解管理链路。Ctrl+C 停止进程，实验库单独清理。

常见问题：web/dist 缺失 → 先完成前端构建；Go 版本不匹配 → 安装声明版本；连接拒绝 → 核对测试数据库；接口返回 200 但页面报错 → 检查 JSON success 和 message；批量部分成功 → 不能当作全失败无副作用重试。

## 源码定位

- [scripts / dependencies · web/package.json:1–35](../../../evidence/new-api-manifest/)
- [AddToken · controller/token.go:299–380](../../../evidence/new-api-create/)
- [tokenRoute · router/api-router.go:301–314](../../../evidence/new-api-routes/)

## 常见误区

go run . 成功编译就表示上游模型、支付和全部后台任务可用。它仅覆盖启动的一部分。

## 自检

为什么不把原目录的 .env 一并复制？

<details>
<summary>展开参考思路</summary>

其中可能含生产数据库和上游密钥；副本运行必须使用隔离配置。

</details>
