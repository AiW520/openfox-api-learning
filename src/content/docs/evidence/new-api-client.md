---
title: "new-api-client · api / inFlightGet"
---

> **版本核对：** 与远端文件一致。本页行号对应已审阅的本地快照。 引用片段与远端一致：[固定提交源码](https://github.com/AiW520/openfox-API/blob/1e826339eb7d083e7d94e434e073db5337fa2dfd/web/src/lib/http-client.ts#L51-L78)（远端行 51–78；私有仓库需要访问权限）。


## 结论

请求带凭据，GET 去重包含会话 sid，避免不同会话共用同一在途请求。

## 源码定位

- 项目：`new-api`
- 相对路径：`web/src/lib/http-client.ts`
- 符号：`api / inFlightGet`
- 行号：51–78
- 文件 SHA-256：`40031b5450c424fc20e1fe3b54c933bb65b5ca5ee654fa06a627fa1264f4985c`

工作区可能有未提交修改；以本页文件哈希为精确依据。提交版本和范围见[覆盖报告](../../coverage/)。

## 核对片段

以下是已审阅的必要片段，不包含完整文件。代码仅作为文本展示。起始行：51。

````ts
  withCredentials: true,
  headers: {
    // no-store forbids storage; no-cache also revalidates any older cached response.
    'Cache-Control': 'no-cache, no-store',
  },
})

const inFlightGet = new Map<string, Promise<unknown>>()
const originalGet = api.get.bind(api)

api.get = ((url: string, config: ApiRequestConfig = {}) => {
  if (config.disableDuplicate) return originalGet(url, config)

  const params = config.params ? JSON.stringify(config.params) : '{}'
  const sessionSID = useAuthStore.getState().auth.session?.sid || 'anonymous'
  const key = `${sessionSID}:${url}?${params}`
  const existingRequest = inFlightGet.get(key)
  if (existingRequest) return existingRequest

  const request = originalGet(url, config).finally(() => {
    inFlightGet.delete(key)
  })
  inFlightGet.set(key, request)
  return request
}) as typeof api.get

function redirectToSignIn(): void {
  if (
````
