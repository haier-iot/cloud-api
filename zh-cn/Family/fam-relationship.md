# 个人用户关系

!> **更新时间**：{docsify-updated}  





## 验证邀请用户和被邀请用户的临时ID

**使用说明**

> 被邀请用户通过验证时向本服务发送验证请求，验证成功后返回用户的真实UserId

**接口描述**

?> **接入地 址：**  `/ufm/v1/protected/personalService/invitation/verification`  
 **HTTP Method：** POST

**输入参数**  

| 类型    | 参数名  | 位置  | 必填|说明|
| ------|:-----:|:-----:|:------:|:------:|  
|  String    | userId | Body| 必填|邀请者的userId，临时或真实userId|  
|  String    | targetUserId | Body| 必填|被邀请者的临时userId| 
|  String    | relation | Body| 必填|关系描述| 
|  Map<String,String>    | extendData | Body| 选填|为附加内容，用户共享参数，可作为好友用户共享数据| 




**输出参数**  

|   类型   |    参数名  | 位置  |必填 |说明|
| ------- |:----------:|:-----:|:--------:|:---------:|
| String |  userId  |   Body  |  必填  | IOT平台用户真实ID，邀请的用户ID |
| String |  targetUserId  |   Body  |  必填  | IOT平台用户真实ID，被邀请的用户真实ID |

**示例**  

**请求样例**
```java  
请求地址：https://{baseuri}/ufm/v1/protected/personalService/invitation/verification
Body:
{
   "userId":"11358726",
	"targetUserId":"100013957366154693",
	"relation":"friend",
	"extendData":{}

}



```  

**请求应答**

```java
{
    "retCode": "00000",
	"retInfo": "成功"，
	"userId": "100013957366154693",
	"targetUserId":"100013957366154693"
}



```

**接口错误码**  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008、F34002  


## 查询有效邀请记录

**使用说明**

> 被邀请用户查询被邀请记录，保留时间可以和IOT商议，验证完成后不再保留    

**接口描述**

?> **接入地 址：**  `/ufm/v1/protected/personalService/invitation/ records `  
 **HTTP Method：** GET

**输入参数**  

| 类型         | 参数名         | 位置  | 必填|说明|
| ------------- |:-------------:|:-----:|:--------:|:--------:|
| &emsp; |  &emsp;  |   &emsp;  |  &emsp;  | &emsp; |



**输出参数**  

|   类型      |     参数名      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
| PersonalInvitation[] |  invitations  |   Body  |  必填  | 邀请本用户的所有未验证记录，超时时间可以协商定义 |

**示例**  

**请样例求**
```java  
请求地址：https://{baseuri}/ufm/v1/protected/personalService/invitation/records

```  

**请求应答**

```java
{
    "retCode": "00000",
	"retInfo": "成功"，
	"invitations": [ 
		{
		 "inviter":{}, 
		"relation":"friend",
		"extendData":{}
		},
		{ 
		"inviter":{}, 
		"relation":"friend",
		"extendData":{}
		}
	]
}

```

**接口错误码**  
> A00001、A00002、A00003、A00004、A00005、B00001、B00002、B00003、B00004、B00006、C00001、C00002、C00003、C00004、D00001、D00002、D00003、D00004、D00005、D00006、D00007、D00008






[^-^]:常用图片注释
[family_flow]:_media/_family/family_flow.png