---
title: "openfox-release-create-response · AddToken response"
---

> **版本核对：** 此证据来自另一份本地版本，仅用于课程中的版本比较，未与本次远端 main 等同。


## 结论

核对该副本创建响应；完整文件是否相同另看版本差异清单。

## 源码定位

- 项目：`openfox-release`
- 相对路径：`controller/token.go`
- 符号：`AddToken response`
- 行号：366–381
- 文件 SHA-256：`daef4343ed8234f5fe8a6c16849794f04d6fa5c234dbba807fef9e0f2d5a073f`

工作区可能有未提交修改；以本页文件哈希为精确依据。提交版本和范围见[覆盖报告](../../coverage/)。

## 核对片段

以下是已审阅的必要片段，不包含完整文件。代码仅作为文本展示。起始行：366。

````go
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
}
````
