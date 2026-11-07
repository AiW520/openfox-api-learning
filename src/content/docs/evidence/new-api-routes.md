---
title: "new-api-routes · tokenRoute"
---

> **版本核对：** 本地文件存在差异。本页行号对应已审阅的本地快照。 引用片段与远端一致：[固定提交源码](https://github.com/AiW520/openfox-API/blob/1e826339eb7d083e7d94e434e073db5337fa2dfd/router/api-router.go#L297-L310)（远端行 297–310；私有仓库需要访问权限）。


## 结论

令牌列表、创建与删除注册到明确的认证中间件；查看密钥另有限流与禁缓存。

## 源码定位

- 项目：`new-api`
- 相对路径：`router/api-router.go`
- 符号：`tokenRoute`
- 行号：301–314
- 文件 SHA-256：`687495ff5756b96daf9c095c317aac2b41bf68531dcb4d348a345bf1775378b0`

工作区可能有未提交修改；以本页文件哈希为精确依据。提交版本和范围见[覆盖报告](../../coverage/)。

## 核对片段

以下是已审阅的必要片段，不包含完整文件。代码仅作为文本展示。起始行：301。

````go
		tokenRoute := apiRouter.Group("/token")
		tokenRoute.Use(middleware.UserAuth())
		tokenRoute.Use(middleware.TokenOperationAudit())
		{
			tokenRoute.GET("/", controller.GetAllTokens)
			tokenRoute.GET("/search", middleware.SearchRateLimit(), controller.SearchTokens)
			tokenRoute.GET("/auto-groups", controller.GetTokenAutoGroups)
			tokenRoute.GET("/:id", controller.GetToken)
			tokenRoute.POST("/:id/key", middleware.CriticalRateLimit(), middleware.DisableCache(), controller.GetTokenKey)
			tokenRoute.POST("/", controller.AddToken)
			tokenRoute.PUT("/", controller.UpdateToken)
			tokenRoute.DELETE("/:id", controller.DeleteToken)
			tokenRoute.POST("/batch", controller.DeleteTokenBatch)
			tokenRoute.POST("/batch/keys", middleware.CriticalRateLimit(), middleware.DisableCache(), controller.GetTokenKeysBatch)
````
