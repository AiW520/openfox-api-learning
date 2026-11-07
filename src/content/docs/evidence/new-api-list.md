---
title: "new-api-list · GetAllTokens"
---

> **版本核对：** 与远端文件一致。本页行号对应已审阅的本地快照。 引用片段与远端一致：[固定提交源码](https://github.com/AiW520/openfox-API/blob/1e826339eb7d083e7d94e434e073db5337fa2dfd/controller/token.go#L132-L144)（远端行 132–144；私有仓库需要访问权限）。


## 结论

列表读取当前用户并返回分页结果；不能从客户端传入任意 user_id 扩大读取范围。

## 源码定位

- 项目：`new-api`
- 相对路径：`controller/token.go`
- 符号：`GetAllTokens`
- 行号：132–144
- 文件 SHA-256：`daef4343ed8234f5fe8a6c16849794f04d6fa5c234dbba807fef9e0f2d5a073f`

工作区可能有未提交修改；以本页文件哈希为精确依据。提交版本和范围见[覆盖报告](../../coverage/)。

## 核对片段

以下是已审阅的必要片段，不包含完整文件。代码仅作为文本展示。起始行：132。

````go
func GetAllTokens(c *gin.Context) {
	userId := c.GetInt("id")
	pageInfo := common.GetPageQuery(c)
	tokens, err := model.GetAllUserTokens(userId, pageInfo.GetStartIdx(), pageInfo.GetPageSize())
	if err != nil {
		common.ApiError(c, err)
		return
	}
	total, _ := model.CountUserTokens(userId)
	pageInfo.SetTotal(int(total))
	pageInfo.SetItems(buildMaskedTokenResponses(tokens))
	common.ApiSuccess(c, pageInfo)
}
````
