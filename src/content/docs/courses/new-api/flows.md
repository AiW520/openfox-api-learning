---
title: "03 · 三条可逐步核对的真实流程"
sidebar:
  order: 3
---

> **阅读版本：** 本课程含本地未提交修改，请先查看[源码版本与远端差异](../../../source-version/)。


> **学完能做什么：** 追踪查询、创建、删除的请求参数、认证、状态码和页面反馈。

## 必要概念

业务成功（business success）与 HTTP 成功是两个维度；200 仅表示这份 HTTP 响应成功到达，不代表令牌业务已执行。

## 流程一：分页查看

用户切换页码 → api-keys-table 的 useQuery 使用页码、筛选、refresh 作为查询条件 → `GET /api/token/?p=1&size=10` → 请求封装携带当前身份 → UserAuth → GetAllTokens 获取上下文用户 → GetAllUserTokens 按分页查询并构造脱敏项 → HTTP 200、success/data → 查询结果提供 items/total，查询错误交由错误状态处理。

合成空列表的响应形状：`{"success":true,"data":{"items":[],"total":0}}`，这里省略了分页附加字段。空数组是正常空数据，不能当网络错误。搜索失败或业务 success=false 显示消息；身份不足在中间件阻止。

## 流程二：创建

填写名称与额度 → api-keys-mutate-drawer 的提交处理 → `POST /api/token/`，JSON 合成示例 `{"name":"learning-token","remain_quota":1000,"unlimited_quota":false,"expired_time":-1}` → UserAuth 和操作审计 → AddToken 绑定 JSON、校验名称/额度/数量与路由配置 → GenerateKey → 构造当前用户的 cleanToken → Insert → 200 → 检查 success → 提示、关闭表单并刷新列表。

响应为 {"success":true,"message":""}，没有返回完整 key。需要通过独立的密钥揭示接口读取；不能照抄 Tuzi 的响应教程。

无效 JSON、额度不合法或数量超限在插入前返回错误；多项业务错误使用 200 + success=false。网络超时无法证明服务端未插入，创建接口的重放安全不能凭前端 loading 保证。不要自动重试创建真实密钥。

## 流程三：删除

选择行并确认 → handleDelete → deleteApiKey → `DELETE /api/token/123/`（合成编号）→ 路由匹配/尾斜杠处理 → UserAuth → DeleteToken 取上下文用户 → GetTokenByIds 后 token.Delete → 数据库按编号和用户查询/删除 → 200 success=true → 成功 toast、关闭确认框、triggerRefresh，再 finally 清除 isDeleting。

访问别人的令牌或不存在编号不会进入正常删除成功路径；错误响应具体消息应以该版本公共错误函数为准，不编造统一 404。失败时不要只看状态码。

## 源码定位

- [tokenRoute · router/api-router.go:301–314](../../../evidence/new-api-routes/)
- [GetAllTokens · controller/token.go:132–144](../../../evidence/new-api-list/)
- [useQuery · web/src/features/keys/components/api-keys-table.tsx:264–303](../../../evidence/new-api-front/)
- [AddToken · controller/token.go:299–380](../../../evidence/new-api-create/)
- [AddToken · cleanToken · controller/token.go:353–380](../../../evidence/new-api-create-return/)
- [createApiKey loop · web/src/features/keys/components/api-keys-mutate-drawer.tsx:307–341](../../../evidence/new-api-modal/)
- [DeleteToken · controller/token.go:383–402](../../../evidence/new-api-delete/)
- [GetTokenByIds · model/token.go:262–269](../../../evidence/new-api-ownership/)
- [handleDelete · web/src/features/keys/components/api-keys-delete-dialog.tsx:44–61](../../../evidence/new-api-remove-ui/)
- [api / inFlightGet · web/src/lib/http-client.ts:51–78](../../../evidence/new-api-client/)

## 常见误区

两个网关沿用同一个响应示例；或认为前端批量创建具有服务端事务语义。

## 自检

创建返回 success=true 却没有 key，是失败吗？

<details>
<summary>展开参考思路</summary>

在 new-api 当前证据版本是正常响应；Tuzi 则返回 id/key。应按版本契约判断。

</details>
