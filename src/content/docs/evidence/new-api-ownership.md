---
title: "new-api-ownership · GetTokenByIds"
---

> **版本核对：** 与远端文件一致。本页行号对应已审阅的本地快照。 引用片段与远端一致：[固定提交源码](https://github.com/AiW520/openfox-API/blob/1e826339eb7d083e7d94e434e073db5337fa2dfd/model/token.go#L262-L269)（远端行 262–269；私有仓库需要访问权限）。


## 结论

记录编号和用户编号共同参与查询，业务值参数绑定。

## 源码定位

- 项目：`new-api`
- 相对路径：`model/token.go`
- 符号：`GetTokenByIds`
- 行号：262–269
- 文件 SHA-256：`2883125c0adfb123c89a56cee77f6172dacdb736b51c94b662d5061a2fe3c205`

工作区可能有未提交修改；以本页文件哈希为精确依据。提交版本和范围见[覆盖报告](../../coverage/)。

## 核对片段

以下是已审阅的必要片段，不包含完整文件。代码仅作为文本展示。起始行：262。

````go
func GetTokenByIds(id int, userId int) (*Token, error) {
	if id == 0 || userId == 0 {
		return nil, common.NewMessage("Token ID or user ID is empty")
	}
	token := Token{Id: id, UserId: userId}
	var err error = nil
	err = DB.First(&token, "id = ? and user_id = ?", id, userId).Error
	return &token, err
````
