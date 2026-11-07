---
title: "new-api-remove-ui · handleDelete"
---

> **版本核对：** 与远端文件一致。本页行号对应已审阅的本地快照。 引用片段与远端一致：[固定提交源码](https://github.com/AiW520/openfox-API/blob/1e826339eb7d083e7d94e434e073db5337fa2dfd/web/src/features/keys/components/api-keys-delete-dialog.tsx#L44-L61)（远端行 44–61；私有仓库需要访问权限）。


## 结论

成功后关闭确认框并刷新，异常和业务失败分别处理，finally 清除删除状态。

## 源码定位

- 项目：`new-api`
- 相对路径：`web/src/features/keys/components/api-keys-delete-dialog.tsx`
- 符号：`handleDelete`
- 行号：44–61
- 文件 SHA-256：`3ce55ed2aa8a0b71685bece614cc0979b6a1ffde4469ce8753a085e1941a9417`

工作区可能有未提交修改；以本页文件哈希为精确依据。提交版本和范围见[覆盖报告](../../coverage/)。

## 核对片段

以下是已审阅的必要片段，不包含完整文件。代码仅作为文本展示。起始行：44。

````tsx
  const handleDelete = async () => {
    if (!currentRow) return

    setIsDeleting(true)
    try {
      const result = await deleteApiKey(currentRow.id)
      if (result.success) {
        toast.success(t(SUCCESS_MESSAGES.API_KEY_DELETED))
        setOpen(null)
        triggerRefresh()
      } else {
        handleServerError(result, t(ERROR_MESSAGES.DELETE_FAILED))
      }
    } catch (error) {
      handleServerError(error, t(ERROR_MESSAGES.UNEXPECTED))
    } finally {
      setIsDeleting(false)
    }
````
