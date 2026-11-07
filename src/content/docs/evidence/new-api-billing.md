---
title: "new-api-billing · PrepareRequestBilling / RefundFailedRequestBilling"
---

> **版本核对：** 与远端文件一致。本页行号对应已审阅的本地快照。 引用片段与远端一致：[固定提交源码](https://github.com/AiW520/openfox-API/blob/1e826339eb7d083e7d94e434e073db5337fa2dfd/controller/relay.go#L165-L180)（远端行 165–180；私有仓库需要访问权限）。


## 结论

请求准备计费与失败退还用 defer 连接；具体账务正确性需要继续读实现和测试。

## 源码定位

- 项目：`new-api`
- 相对路径：`controller/relay.go`
- 符号：`PrepareRequestBilling / RefundFailedRequestBilling`
- 行号：165–180
- 文件 SHA-256：`ebce879d61aba2e8c49599bac02d4cfd7898f721fb31236cb999fffc6f16fd75`

工作区可能有未提交修改；以本页文件哈希为精确依据。提交版本和范围见[覆盖报告](../../coverage/)。

## 核对片段

以下是已审阅的必要片段，不包含完整文件。代码仅作为文本展示。起始行：165。

````go
	}

	if newAPIError = relay.PrepareRequestBilling(c, relayInfo); newAPIError != nil {
		return
	}
	defer func() {
		newAPIError = relay.RefundFailedRequestBilling(c, relayInfo, newAPIError)
	}()

	retryParam := &service.RetryParam{
		Ctx:         c,
		TokenGroup:  relayInfo.TokenGroup,
		ModelName:   relayInfo.OriginModelName,
		RequestPath: c.Request.URL.Path,
		Retry:       common.GetPointer(0),
	}
````
