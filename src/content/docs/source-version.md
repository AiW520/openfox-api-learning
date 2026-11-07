---
title: "源码版本与本地差异"
---

本课程依据已审阅的本地快照编写，包含未提交修改。远端仓库为 `AiW520/openfox-API`，核对提交为 `1e826339eb7d083e7d94e434e073db5337fa2dfd`。**这不是远端 main 的无差异说明书。**

## 差异对学习的影响

本地 SetApiRouter 增加 docsSites 可选参数及文档搜索路由注册；web/package.json 在构建前增加文档生成检查，并增加若干 UI/文档依赖。依赖声明不等于已经在业务路径使用；本课程分析的令牌核心处理文件与远端一致。

## 如何使用版本证据

先阅读真实流程，再进入证据页。标记“与远端文件一致”的条目可对照固定提交；文件有差异时，只在片段逐行唯一匹配后提供远端行号。其余实现只代表本地快照。哈希（SHA-256）是内容指纹，文件改动后指纹会变化。

| 被引用文件 | 核对结果 |
|---|---|
| `router/api-router.go` | 本地文件存在差异 |
| `controller/token.go` | 与远端文件一致 |
| `model/token.go` | 与远端文件一致 |
| `web/package.json` | 本地文件存在差异 |
| `web/src/features/keys/components/api-keys-table.tsx` | 与远端文件一致 |
| `web/src/features/keys/components/api-keys-mutate-drawer.tsx` | 与远端文件一致 |
| `web/src/features/keys/components/api-keys-delete-dialog.tsx` | 与远端文件一致 |
| `web/src/lib/http-client.ts` | 与远端文件一致 |
| `controller/relay.go` | 与远端文件一致 |

## 本地版本

- `new-api`：提交 `1e826339eb7d083e7d94e434e073db5337fa2dfd`；快照 `5fdb5b5d23a471db54ceea349defacc1fed42249a427ea707f21862be157648b`；未提交修改：True。
- `openfox-release`：提交 `非 Git`；快照 `123e91eea11b3785430ef9f7e0373026ccfc897f83478b1a249df1edf1e062c3`；未提交修改：False。
- `openfox-followup`：提交 `非 Git`；快照 `7382166839bc88751e57aeef775843bbee8a92b82911460034e54feb5c072eeb`；未提交修改：False。
- `openfox-model-admin`：提交 `非 Git`；快照 `d2b909221444970ea4a86a6b80f98bcca2b6526668633ba002d7cc575e212c60`；未提交修改：False。
- `openfox-tags`：提交 `非 Git`；快照 `53f7fe9acf5ba71da37c95cee3a593e1e085901ae7e19a26ef5aa620e477e9ff`；未提交修改：False。
- `openfox-replica`：提交 `非 Git`；快照 `2e3adea7c03e7f0a27aa2b4b59512fd38086c360d9e6953bd47695e8d78df464`；未提交修改：False。
- `openfox-image-release`：提交 `非 Git`；快照 `8df0462f68e7644419520d555ee9562187c8b0604c303f561288023eb197c11a`；未提交修改：False。

文件差异可能发生在引用片段之外；不能用“文件不同”推断所有流程都改变。教程的测试结果只属于注明的本地版本，不代表线上生产环境。

[逐文件哈希报告](/reports/remote-review.json) · [分析与验证](../coverage/)
