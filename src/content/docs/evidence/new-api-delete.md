---
title: "new-api-delete · DeleteToken"
---

> **版本核对：** 与远端文件一致。本页行号对应已审阅的本地快照。 引用片段与远端一致：[固定提交源码](https://github.com/AiW520/openfox-API/blob/1e826339eb7d083e7d94e434e073db5337fa2dfd/controller/token.go#L383-L402)（远端行 383–402；私有仓库需要访问权限）。


## 结论

删除从认证上下文取 user_id，并调用带所有权条件的数据访问。

## 源码定位

- 项目：`new-api`
- 相对路径：`controller/token.go`
- 符号：`DeleteToken`
- 行号：383–402
- 文件 SHA-256：`daef4343ed8234f5fe8a6c16849794f04d6fa5c234dbba807fef9e0f2d5a073f`

工作区可能有未提交修改；以本页文件哈希为精确依据。提交版本和范围见[覆盖报告](../../coverage/)。

## 核对片段

以下是已审阅的必要片段，不包含完整文件。代码仅作为文本展示。起始行：383。

````go
func DeleteToken(c *gin.Context) {
	id, _ := strconv.Atoi(c.Param("id"))
	userId := c.GetInt("id")
	token, err := model.GetTokenByIds(id, userId)
	if err != nil {
		common.ApiError(c, err)
		return
	}
	params := tokenAuditParams(c)
	params["id"], params["name"] = token.Id, token.Name
	err = token.Delete()
	if err != nil {
		common.ApiError(c, err)
		return
	}
	common.SetContextKey(c, constant.ContextKeyTokenAuditSucceeded, true)
	c.JSON(http.StatusOK, gin.H{
		"success": true,
		"message": "",
	})
````
