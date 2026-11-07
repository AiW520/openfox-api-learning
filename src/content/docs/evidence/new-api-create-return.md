---
title: "new-api-create-return · AddToken · cleanToken"
---

> **版本核对：** 与远端文件一致。本页行号对应已审阅的本地快照。 引用片段与远端一致：[固定提交源码](https://github.com/AiW520/openfox-API/blob/1e826339eb7d083e7d94e434e073db5337fa2dfd/controller/token.go#L353-L380)（远端行 353–380；私有仓库需要访问权限）。


## 结论

tuzi 创建成功返回 id/key；new-api 创建成功仅 success/message。

## 源码定位

- 项目：`new-api`
- 相对路径：`controller/token.go`
- 符号：`AddToken · cleanToken`
- 行号：353–380
- 文件 SHA-256：`daef4343ed8234f5fe8a6c16849794f04d6fa5c234dbba807fef9e0f2d5a073f`

工作区可能有未提交修改；以本页文件哈希为精确依据。提交版本和范围见[覆盖报告](../../coverage/)。

## 核对片段

以下是已审阅的必要片段，不包含完整文件。代码仅作为文本展示。起始行：353。

````go
	cleanToken := model.Token{
		UserId:             c.GetInt("id"),
		Name:               token.Name,
		Key:                key,
		CreatedTime:        common.GetTimestamp(),
		AccessedTime:       common.GetTimestamp(),
		ExpiredTime:        token.ExpiredTime,
		RemainQuota:        token.RemainQuota,
		UnlimitedQuota:     token.UnlimitedQuota,
		ModelLimitsEnabled: token.ModelLimitsEnabled,
		ModelLimits:        token.ModelLimits,
		AllowIps:           token.AllowIps,
		Group:              token.Group,
		CrossGroupRetry:    token.CrossGroupRetry,
		AutoGroups:         token.AutoGroups,
	}
	cleanToken.AllowUserQuotaQuery = request.AllowUserQuotaQuery == nil || *request.AllowUserQuotaQuery
	err = cleanToken.Insert()
	if err != nil {
		common.ApiError(c, err)
		return
	}
	params["id"] = cleanToken.Id
	common.SetContextKey(c, constant.ContextKeyTokenAuditSucceeded, true)
	c.JSON(http.StatusOK, gin.H{
		"success": true,
		"message": "",
	})
````
