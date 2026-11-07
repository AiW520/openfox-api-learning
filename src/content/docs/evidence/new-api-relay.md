---
title: "new-api-relay · Relay · deferred error / GetAndValidateRequest"
---

> **版本核对：** 与远端文件一致。本页行号对应已审阅的本地快照。 引用片段与远端一致：[固定提交源码](https://github.com/AiW520/openfox-API/blob/1e826339eb7d083e7d94e434e073db5337fa2dfd/controller/relay.go#L100-L150)（远端行 100–150；私有仓库需要访问权限）。


## 结论

请求校验失败映射 400/413；已开始的 SSE 响应写入流错误，而非重新假装能改 HTTP 状态。

## 源码定位

- 项目：`new-api`
- 相对路径：`controller/relay.go`
- 符号：`Relay · deferred error / GetAndValidateRequest`
- 行号：100–150
- 文件 SHA-256：`ebce879d61aba2e8c49599bac02d4cfd7898f721fb31236cb999fffc6f16fd75`

工作区可能有未提交修改；以本页文件哈希为精确依据。提交版本和范围见[覆盖报告](../../coverage/)。

## 核对片段

以下是已审阅的必要片段，不包含完整文件。代码仅作为文本展示。起始行：100。

````go
	defer func() {
		if newAPIError != nil {
			service.RecordRequestPolicyTermination(c, newAPIError)
			logger.LogError(c, common.LogText("relay error: %s", common.LocalLogPreview(newAPIError.Error())))
			newAPIError.SetMessage(common.MessageWithRequestId(newAPIError.Error(), requestId))
			upstreamURLs := common.UpstreamAddresses(c)
			if relayFormat != types.RelayFormatOpenAIRealtime && c.Writer.Written() &&
				strings.HasPrefix(c.Writer.Header().Get("Content-Type"), "text/event-stream") {
				writeRelayStreamError(c, relayFormat, newAPIError)
				return
			}
			if relayFormat != types.RelayFormatOpenAIRealtime {
				c.Header("Content-Type", "application/json; charset=utf-8")
			}
			switch relayFormat {
			case types.RelayFormatOpenAIRealtime:
				helper.WssError(c, ws, newAPIError.ToOpenAIError(upstreamURLs...))
			case types.RelayFormatClaude:
				c.JSON(newAPIError.StatusCode, gin.H{
					"type":  "error",
					"error": newAPIError.ToClaudeError(upstreamURLs...),
				})
			default:
				c.JSON(newAPIError.StatusCode, gin.H{
					"error": newAPIError.ToOpenAIError(upstreamURLs...),
				})
			}
		}
	}()

	request, err := helper.GetAndValidateRequest(c, relayFormat)
	if err != nil {
		// Map "request body too large" to 413 so clients can handle it correctly
		if common.IsRequestBodyTooLargeError(err) || errors.Is(err, common.ErrRequestBodyTooLarge) {
			newAPIError = types.NewErrorWithStatusCode(err, types.ErrorCodeReadRequestBodyFailed, http.StatusRequestEntityTooLarge, types.ErrOptionWithSkipRetry())
		} else {
			newAPIError = types.NewError(err, types.ErrorCodeInvalidRequest, types.ErrOptionWithStatusCode(http.StatusBadRequest), types.ErrOptionWithSkipRetry())
		}
		return
	}

	relayInfo, err := relaycommon.GenRelayInfo(c, relayFormat, request, ws)
	if err != nil {
		newAPIError = types.NewError(err, types.ErrorCodeGenRelayInfoFailed)
		return
	}

	defer func() {
		recovered := recover()
		resultErr := newAPIError
		if recovered != nil {
````
