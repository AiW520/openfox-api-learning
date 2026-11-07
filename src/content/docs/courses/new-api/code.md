---
title: "04 · 精读校验、密钥与失败分支"
sidebar:
  order: 4
---

> **阅读版本：** 本课程含本地未提交修改，请先查看[源码版本与远端差异](../../../source-version/)。


> **学完能做什么：** 说明修改校验或错误处理会影响哪些层。

## 必要概念

信任边界（trust boundary）是数据从不可信输入进入内部业务的关口；响应契约（response contract）约定调用方应检查哪些字段。

## 创建函数怎样读

输入为 Gin Context 中的 JSON 与认证结果；调用者是路由注册表，核心被调用者是计数、密钥生成、Insert 和公共响应函数。先画早返回分支，再看最终写库：JSON 失败 → 参数失败 → 配额/数量失败 → 密钥生成失败 → 插入失败 → 成功。

new-api 对 Name 使用 len(token.Name)>50。Go 的字符串 len 按字节计数，不是中文字符数；UI 的字符限制与后端字节限制可能不同。cleanToken 逐字段复制且 UserId 取上下文，阻止直接使用输入对象全部字段。

修改校验阈值会影响表单提示、接口测试和旧客户端；修改响应 key 字段会影响复制密钥、审计和缓存策略。保留 no-store 不能防止所有代理日志或浏览器扩展泄露，因此不应在教程中展示真实 key。

## 请求去重与状态

前端 GET 在途去重减少相同请求，缓存键与身份边界必须一致。这种 Promise 复用不等于 POST 幂等。页面成功后重新查询，能读取后端处理后的状态，但失败时仍需保留用户输入并提示。

## 转发边界

模型转发从 /v1 的 TokenAuth、模型限流和 Distribute 进入 Relay。Relay 按协议解析请求，构造 RelayInfo，再准备计费与失败退款。请求体过大对应 413，普通参数错误对应 400；SSE（Server-Sent Events，服务器事件流）已开始写出后，通过流内错误传递，不能再次修改已发出的 HTTP 状态。具体渠道重试和账务事务仍是扩展阅读范围，未做付费上游实测。

## 源码定位

- [AddToken · controller/token.go:299–380](../../../evidence/new-api-create/)
- [AddToken · cleanToken · controller/token.go:353–380](../../../evidence/new-api-create-return/)
- [api / inFlightGet · web/src/lib/http-client.ts:51–78](../../../evidence/new-api-client/)
- [Relay · deferred error / GetAndValidateRequest · controller/relay.go:100–150](../../../evidence/new-api-relay/)
- [PrepareRequestBilling / RefundFailedRequestBilling · controller/relay.go:165–180](../../../evidence/new-api-billing/)

## 常见误区

GET 请求去重可以防止所有重复提交；或把已经写出的流响应当作普通 JSON 响应重写。

## 自检

在哪一层增加名称限制最可靠？

<details>
<summary>展开参考思路</summary>

后端入口必须验证，前端同步做提示以改善体验；两层的长度单位必须一致。

</details>
