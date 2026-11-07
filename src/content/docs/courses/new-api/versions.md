---
title: "06 · 同项目的版本差异"
sidebar:
  order: 6
---

> **阅读版本：** 本课程含本地未提交修改，请先查看[源码版本与远端差异](../../../source-version/)。


> **学完能做什么：** 知道共享课程适用到哪些版本，避免跨版本照抄接口契约。

## 必要概念

内容寻址（content addressing）用文件内容哈希比较版本；哈希不同只能证明字节变化，不能自动证明功能差异。

## 文件级比较

所有数量仅统计允许读取的文本源码/配置/锁文件。共同路径哈希相同可确认文件字节一致；变更文件必须进一步阅读才能解释行为。压缩包、二进制与生产日志不进入比较。

| 副本 | 共同路径 | 哈希相同 | 变化文件 | 仅主版本 / 仅副本 |
| --- | --- | --- | --- | --- |
| openfox-release | 2429 | 2365 | 64 | 290 / 3 |
| openfox-followup | 2702 | 2698 | 4 | 17 / 1 |
| openfox-model-admin | 2463 | 2445 | 18 | 256 / 1 |
| openfox-tags | 2703 | 2701 | 2 | 16 / 1 |
| openfox-replica | 2701 | 2693 | 8 | 18 / 1 |
| openfox-image-release | 2443 | 2391 | 52 | 276 / 1 |

## 已逐段核对的行为

new-api/OpenFox 这些副本的 AddToken 成功段返回 success/message；Tuzi 的两份版本返回 id/key 并禁缓存。令牌管理课程以各自主版本为准，响应片段按副本独立留证。课程不能保证所有差异只涉及这一个函数。

完整变更路径保存在[版本差异 JSON](/reports/version-differences.json)。新增、删除和变化分开保存，保留后续增量分析所需依据。变化清单中尚未写出语义说明的文件明确属于待深入阅读，不标记成完成。

## 源码定位

- [AddToken response · controller/token.go:366–381](../../../evidence/openfox-release-create-response/)
- [AddToken response · controller/token.go:366–381](../../../evidence/openfox-followup-create-response/)
- [AddToken response · controller/token.go:366–381](../../../evidence/openfox-model-admin-create-response/)
- [AddToken response · controller/token.go:366–381](../../../evidence/openfox-tags-create-response/)
- [AddToken response · controller/token.go:366–381](../../../evidence/openfox-replica-create-response/)
- [AddToken response · controller/token.go:366–381](../../../evidence/openfox-image-release-create-response/)

## 常见误区

相同 commit 可以代表当前 dirty 工作区的全部文件。

## 自检

同名函数的文件哈希变了，但响应段相同，该怎样表述？

<details>
<summary>展开参考思路</summary>

只能说已读的响应段行为一致；不能据此说整个函数或项目没有变化。

</details>
