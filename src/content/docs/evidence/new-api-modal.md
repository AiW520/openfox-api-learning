---
title: "new-api-modal · createApiKey loop"
---

> **版本核对：** 与远端文件一致。本页行号对应已审阅的本地快照。 引用片段与远端一致：[固定提交源码](https://github.com/AiW520/openfox-API/blob/1e826339eb7d083e7d94e434e073db5337fa2dfd/web/src/features/keys/components/api-keys-mutate-drawer.tsx#L307-L341)（远端行 307–341；私有仓库需要访问权限）。


## 结论

逐个提交创建，统计成功数量并触发刷新；不能视为原子批量事务。

## 源码定位

- 项目：`new-api`
- 相对路径：`web/src/features/keys/components/api-keys-mutate-drawer.tsx`
- 符号：`createApiKey loop`
- 行号：307–341
- 文件 SHA-256：`cb53b912ac4a90805daa72f8e8afd41035e32985baa790feb5e6130a90468161`

工作区可能有未提交修改；以本页文件哈希为精确依据。提交版本和范围见[覆盖报告](../../coverage/)。

## 核对方法

打开上述相对路径，跳转到行号并定位符号；本页不展示此大段源码。
