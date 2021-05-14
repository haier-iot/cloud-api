#  设备功能标签

## 简介

> 功能标签接口使用Https协议，使用`https://uws.haier.net/rcs/label/`进行访问

本文档使用接口类型定义接口url，关键字为toc和tob  
例如/rcs/label/{url_type}/add/label  
toC接口为：/rcs/label/toc/add/label  
toB接口为：/rcs/label/tob/add/label


## 签名认证

> 调用方需要对发送到uws的请求进行签名，执行签名计算的签名值需要赋值到Header头中的sign属性（见公共部分说明），以便服务端进行签名验证。签名算法区分toC接口和toB接口两种方式。  


**待签名字符串为**：  
toC：url + body +appId+appKey +timestamp；  
toB：url + body +systemId+systemKey +timestamp；


| 名称 | 说明 |
| :----------: |:----------:|
|url 字符串| 指请求的接口地址去除https://域名+端口 后剩余的路径部分|
|Body字符串| 指应用发送请求的Body部分去除所有空白字符后的JSON字符串，没有body时为空字符串（不是null） |  
|appId| Header头中的属性，海极网申请移动应用时获取 | 
|appKey| 在海极网给应用申请的appKey，不能作为参数传递，仅参与签名计算 |
|systemId| Header头中的属性，海极网申请云应用时获取 |
|systemKey| 在海极网给应用申请的systemKey，不能作为参数传递，仅参与签名计算 |
|timestamp| Header头中的属性，时间戳 |


## 公共结构


**请求头信息定义（ToC）**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
appId|String|Header|必填|app应用ID，toC接口使用
clientId|String|Header|必填|客户端ID， 主要用途为唯一标识客户端 (例如，手机)，可调用usdk得到客户端ID的值
accessToken|String|Header|必填|安全令牌token，32位字符；
sign|String|Header|必填|对请求进行签名运算产生的签名
timestamp|long|Header|必填|Unix时间戳，精确到毫秒
appVersion|String|Header|非必填|应用版本最多32 位字符,应用版本标识
apiVersion|String|Header|必填|接口版本，默认传v1
Content-Type|String|Header|必填|必须为application/json;charset=UTF-8


**请求头信息定义（ToB）**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
systemId|String|Header|必填|云服务，云应用ID,SV开头，toB使用
sign|String|Header|必填|对请求进行签名运算产生的签名
timestamp|long|Header|必填|Unix时间戳，精确到毫秒
appVersion|String|Header|非必填|应用版本最多32 位字符,应用版本标识
apiVersion|String|Header|必填|接口版本，默认传1.0
Content-Type|String|Header|必填|必须为application/json;charset=UTF-8


**标签类型labelType**  

| **名称** | 标签类型 | labelType |   
| :----------: |:----------:|:-----:|
|**类型**|**Code**|**使用情景**|
|指纹| fingerprint | 智能门 |
|人脸| face | 智能门 |
|声纹|voiceprint  | 智能门 |   
|密码| password | 智能门 |  
|卡| card | 智能门 |
|角色| role | 智能门 |
|情景模式| scene | 安防70面板 |


**标签行为labelAction**

| **名称** | 标签行为 |
| :----------: |:----------:|
|**行为**|**Code**|
|开门| open_door |  
|触发场景| enable_scene | 


## 创建标签

**使用说明**

> 创建标签

**接口描述**

?> **接入地址：** `https://uws.haier.net/rcs/label/{url_type}/add/label`</br>
**HTTP Method：** POST 

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|Body|必填|设备ID  
deviceAttribute|String|Body|必填|设备属性（数据来源：标准设备模型） 
deviceAttributeValue|String|Body|必填|设备属性值（数据来源：标准设备模型）  
labelType|String|Body|必填|标签类型
labelName|String|Body|必填|标签名称，别名
labelAction|String|Body|必填|标签行为（数据来源：标准设备模型）
userId|String|Body|非必填|userId，家庭场景下传家庭成员的userId；非家庭场景传用户中心userId；非用户场景下不传该参数


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|必填|错误码
retInfo|String|Body|必填|错误详细信息
payload|Object|Body|必填|payload
labelId|String|payload|必填|标签ID


**示例**

**请求样例**

```
Header:
	toC POST url: https://uws.haier.net/rcs/label/toc/add/label
	toC = Header:
	appId: MB-*****-0000
	accessToken: TGT1RALFFU4RMLAI27B7848DJWQPN0
	sign:f30684c4abbdc2acad7142317c9d2a931552425555c67fd2271c356eb2f937dd
	timestamp: 1595470190284
	clientId: testclientid
	apiversion:v1
	
	toB POST url: https://uws.haier.net/rcs/label/tob/add/label
	toB = Header:
	Content-Type: application/json;charset=UTF-8
	systemId: SV-*****-0000
	sign: f30684c4abbdc2acad7142317c9d2a931552425555c67fd2271c356eb2f937dd
	timestamp: 1595470190284
	apiversion:v1

Body:
	{
	    "deviceId":"A10010",
	    "deviceAttribute":"开门指纹序号",
	    "deviceAttributeValue":"001",
	    "labelType":"fingerprint",
	    "labelName":"爸爸的指纹",
	    "labelAction":"open_dor",
		"userId":"234569"
	}

```
**请求应答**
```
{
    "retCode":"00000",
    "retInfo":"成功",
    "payload":{
        "labelId":"8574645466"
    }
}

```

**接口错误码**
> 23100、23110、23800、23810、23400、23410、23420、23430

</div>


## 根据标签ID查询标签信息

**使用说明**

> 根据标签ID查询标签信息

**接口描述**

?> **接入地址：** `https://uws.haier.net/rcs/label/{url_type}/get/label`</br>
**HTTP Method：** POST 

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
labelId|String|Body|必填|标签ID  


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
payload|Object|Body|必填|payload
labelId|String|payload|必填|标签ID  
deviceId|String|payload|必填|设备ID
deviceAttribute|String|payload|必填|设备属性
deviceAttributeValue|String|payload|必填|设备属性值
labelType|String|payload|必填|标签类型
labelName|String|payload|必填|标签名称，别名
labelAction|String|payload|必填|标签行为
userId|String|payload|非必填|IoT账号 userId


**示例**

**请求样例**

```
Header：
	toC=POST url: https://uws.haier.net/rcs/label/toc/get/label
	toC=Header:
	appId: MB-*****-0000
	accessToken: TGT1RALFFU4RMLAI27B7848DJWQPN0
	sign: f30684c4abbdc2acad7142317c9d2a931552425555c67fd2271c356eb2f937dd
	timestamp: 1595470190284
	clientId: testclientid
	apiversion:v1
	
	toB = POST url: https://uws.haier.net/rcs/label/tob/get/label
	toB = Header:
	Content-Type: application/json;charset=UTF-8
	systemId: SV-*****-0000
	sign: f30684c4abbdc2acad7142317c9d2a931552425555c67fd2271c356eb2f937dd
	timestamp: 1595470190284
	apiversion:v1
Body
	{
		"labelId":"8574645466"
	}

```
**请求应答**
```
{
    "retCode":"00000",
    "retInfo":"成功",
    "payload":{
        "labelId":"8574645466",
        "deviceId":" A10010",
        "deviceAttribute":"开门指纹序号",
        "deviceAttributeValue ":"001",
        "labelType":"fingerprint",
        "labelName":"爸爸的指纹",
        "labelAction":"open",
        "userId":"234569"
    }
}

```

**接口错误码**
> 23100、23110、23800、23810、23400、23410、23420、23430

</div>


## 查询标签列表


**使用说明**

> 根据条件组合（设备ID或设备ID和账号ID，标签类型，设备属性组合）查询标签列表

**接口描述**

?> **接入地址：** `https://uws.haier.net/rcs/label/{url_type}/find/labels-by-properties`</br>
**HTTP Method：** POST 

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
labelId|String|Body|必填|标签ID  
labelTypes|String[]|Body|非必填|标签类型，数组最大长度限制20  
deviceAttributes|String[]|Body|非必填|设备属性（数据来源：设备标准模型），数组最大长度限制20  
userId|String|Body|非必填|家庭场景下传家庭成员的userId；非家庭场景传用户中心userId；非用户场景下不传该参数


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
payload|Object|Body|必填|payload，返回标签列表
labelId|String|payload|必填|标签ID  
deviceAttribute|String|payload|必填|设备属性
deviceAttributeValue|String|payload|必填|设备属性值
labelType|String|payload|必填|标签类型
labelName|String|payload|必填|标签名称，别名
labelAction|String|payload|必填|标签行为
userId|String|payload|非必填|IoT账号 userId


**示例1：根据设备ID查询**

**请求样例**

```
Header：
	toC = POST url: https://uws.haier.net/rcs/label/toc/find/labels-by-properties
	toC = Header:
	appId: MB-*****-0000
	accessToken: TGT1RALFFU4RMLAI27B7848DJWQPN0
	sign: f30684c4abbdc2acad7142317c9d2a931552425555c67fd2271c356eb2f937dd
	timestamp: 1595470190284
	clientId: testclientid
	apiversion:v1
	
	toB = POST url: https://uws.haier.net/rcs/label/tob/find/labels-by-properties
	toB = Header:
	Content-Type: application/json;charset=UTF-8
	systemId: SV-*****-0000
	sign: f30684c4abbdc2acad7142317c9d2a931552425555c67fd2271c356eb2f937dd
	timestamp: 1595470190284
	apiversion:v1
Body:
	{
		 "deviceId":"A10010"
	}

```
**请求应答**
```
{
	"retCode":"00000",
    "retInfo":"成功",
    "payload":[
        {
            "labelId":"8574645466",
            "deviceAttribute":"开门指纹序号",
            "deviceAttributeValue ":"001",
            "labelType":"fingerprint",
            "labelName":"爸爸的指纹",
            "labelAction":"open",
            "userId":"234569"
        },
     {
            "labelId":"8574645465",
            "deviceAttribute":"开门指纹序号",
            "deviceAttributeValue ":"002",
            "labelType":"fingerprint",
            "labelName":"妈妈的指纹",
            "labelAction":"open",
            "userId":"234569"
        },
        {
            "labelId":"8574645464",
            "deviceAttribute":"开门角色序号",
            "deviceAttributeValue ":"002",
            "labelType":"fingerprint",
            "labelName":"妈妈的指纹",
            "labelAction":"open",
            "userId":"234569"
        }
    ]
}

```

**示例2：根据设备ID+标签类型查询**

**请求样例**

```
Header:
	POST url: https://uws.haier.net/rcs/label/find/labels-by-properties
	Header: 略

Body:
	{
		"deviceId":"A10010",
	    "labelType":["fingerprint","role"]
	}

```
**请求应答**
```
{
    "retCode":"00000",
    "retInfo":"成功",
    "payload":[
        {
            "labelId":"8574645466",
       "deviceId":"A10010",
            "deviceAttribute":"开门指纹序号",
            "deviceAttributeValue ":"001",
            "labelType":"fingerprint",
            "labelName":"爸爸的指纹",
            "labelAction":"open",
            "userId":""
        },
     {
            "labelId":"8574645465",
       "deviceId":"A10010",
            "deviceAttribute":"开门指纹序号",
            "deviceAttributeValue ":"002",
            "labelType":"fingerprint",
            "labelName":"妈妈的指纹",
            "labelAction":"open",
            "userId":""
        },
     {
            "labelId":"8574645465",
       "deviceId":"A10010",
            "deviceAttribute":"开门角色序号",
            "deviceAttributeValue ":"001",
            "labelType":"role",
            "labelName":"妈妈开门",
            "labelAction":"open",
            "userId":""
        }
    ]
}

```


**示例3：根据设备ID+设备属性查询**

**请求样例**

```
Header:
	POST url: https://uws.haier.net/rcs/label/find/labels-by-properties
	Header: 略

Body:
	{
	   "deviceId":"A10010",
	   "deviceAttribute":["开门指纹序号","开门角色序号"]
	}

```
**请求应答**
```
{
    "retCode":"00000",
    "retInfo":"成功",
    "payload":[
        {
            "labelId":"8574645466",
            "deviceAttribute":"开门指纹序号",
            "deviceAttributeValue ":"001",
            "labelType":"fingerprint",
            "labelName":"爸爸的指纹",
            "labelAction":"open",
            "userId":""
        },
        {
            "labelId":"8574645465",
            "deviceAttribute":"开门指纹序号",
            "deviceAttributeValue ":"002",
            "labelType":"fingerprint",
            "labelName":"妈妈的指纹",
            "labelAction":"open",
            "userId":"234569"
        },
     {
            "labelId":"8574645464",
       "deviceId":"A10010",
            "deviceAttribute":"开门角色序号",
            "deviceAttributeValue ":"001",
            "labelType":"role",
            "labelName":"妈妈开门",
            "labelAction":"open",
            "userId":""
        }
    ]
}

```


**示例4：根据设备ID+账号ID（可能没有角色数据）**

**请求样例**   

```
Header:
	POST url: https://uws.haier.net/rcs/label/find/labels-by-properties
	Header:略

Body:
	{
	 	"deviceId":"A10010",
	    "userId":"234569"
	}

```
**请求应答**
```
{
    "retCode":"00000",
    "retInfo":"成功",
    "payload":[
        {
            "labelId":"8574645466",
       "deviceId":"A10010",
            "deviceAttribute":"开门指纹序号",
            "deviceAttributeValue ":"001",
            "labelType":"fingerprint",
            "labelName":"爸爸的指纹1",
            "labelAction":"open",
            "userId":"234569"
        },
     {
            "labelId":"8574645465",
       "deviceId":"A10010",
            "deviceAttribute":"开门指纹序号",
            "deviceAttributeValue ":"002",
            "labelType":"fingerprint",
            "labelName":"爸爸的指纹2",
            "labelAction":"open",
            "userId":"234569"
        }
    ]
}

```


**接口错误码**
> 23100、23110、23800、23810、23400、23410、23420、23430

</div>


## 修改标签


**使用说明**

> 修改标签

**接口描述**

?> **接入地址：** `https://uws.haier.net/rcs/label/{url_type}/update/label`</br>
**HTTP Method：** POST 

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
labelId|String|Body|必填|标签ID  
labelName|String|Body|必填|标签名称，目前仅支持对名称修改  


**输出参数**

标准输出


**示例**

**请求样例**

```
Header:
	toC = POST url: https://uws.haier.net/rcs/label/toc/update/label
	toC = Header:
	appId: MB-*****-0000
	accessToken: TGT1RALFFU4RMLAI27B7848DJWQPN0
	sign: f30684c4abbdc2acad7142317c9d2a931552425555c67fd2271c356eb2f937dd
	timestamp: 1595470190284
	clientId: testclientid
	apiversion:v1
	
	toB = POST url: https://uws.haier.net/rcs/label/tob/update/label
	toB = Header:
	Content-Type: application/json;charset=UTF-8
	systemId: SV-*****-0000
	sign: f30684c4abbdc2acad7142317c9d2a931552425555c67fd2271c356eb2f937dd
	timestamp: 1595470190284
	apiversion:v1
Body:
	{
		"labelId":"8574645466",
	    "labelName":"妈妈的指纹"
	}

```
**请求应答**
```
{
    "retCode":"00000",
    "retInfo":"成功"
}

```

**接口错误码**
> 23100、23110、23800、23810、23400、23410、23420、23430

</div>


## 删除标签


**使用说明**

> 根据标签ID删除标签

**接口描述**

?> **接入地址：** `https://uws.haier.net/rcs/label/{url_type}/delete/label`</br>
**HTTP Method：** POST 

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
labelId|String|Body|必填|标签ID  
isCascade|String|Body|必填|删除角色时必传，true 1， false 0 


**输出参数**

标准输出


**示例**

**请求样例**

```
Header：
	toC = POST url: https://uws.haier.net/rcs/label/toc/delete/label
	toC = Header:
	appId: MB-*****-0000
	accessToken: TGT1RALFFU4RMLAI27B7848DJWQPN0
	sign: f30684c4abbdc2acad7142317c9d2a931552425555c67fd2271c356eb2f937dd
	timestamp: 1595470190284
	clientId: testclientid
	apiversion:v1
	
	toB = POST url: https://uws.haier.net/rcs/label/tob/delete/label
	toB = Header:
	Content-Type: application/json;charset=UTF-8
	systemId: SV-*****-0000
	sign: f30684c4abbdc2acad7142317c9d2a931552425555c67fd2271c356eb2f937dd
	timestamp: 1595470190284
	apiversion:v1
Body:
	{
	    "labelId":"8574645466"
	}

```
**请求应答**
```
{
    "retCode":"00000",
    "retInfo":"成功"
}

```

**接口错误码**
> 23100、23110、23800、23810、23400、23410、23420、23430

</div>


## 批量删除标签


**使用说明**

> 根据标签ID列表批量删除标签

**接口描述**

?> **接入地址：** `https://uws.haier.net/rcs/label/{url_type}/delete/labels`</br>
**HTTP Method：** POST 

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
labelIds|String[]|Body|必填|标签ID集合，长度限制30  


**输出参数**

标准输出


**示例**

**请求样例**

```
Header：
	toC = POST url: https://uws.haier.net/rcs/label/toc/delete/labels
	toC = Header:
	appId: MB-*****-0000
	accessToken: TGT1RALFFU4RMLAI27B7848DJWQPN0
	sign: f30684c4abbdc2acad7142317c9d2a931552425555c67fd2271c356eb2f937dd
	timestamp: 1595470190284
	clientId: testclientid
	apiversion:v1
	
	toB = POST url: https://uws.haier.net/rcs/label/tob/delete/labels
	toB = Header:
	Content-Type: application/json;charset=UTF-8
	systemId: SV-*****-0000
	sign: f30684c4abbdc2acad7142317c9d2a931552425555c67fd2271c356eb2f937dd
	timestamp: 1595470190284
	apiversion:v1
Body:
	{
	    "labelId":["8574645466","8574645465"]
	}

```
**请求应答**
```
{
    "retCode":"00000",
    "retInfo":"成功"
}

```

**接口错误码**
> 23100、23110、23800、23810、23400、23410、23420、23430

</div>


## 根据设备ID批量删除标签


**使用说明**

> 根据设备ID批量删除标签

**接口描述**

?> **接入地址：** `https://uws.haier.net/rcs/label/{url_type}/delete/labels-by-deviceId`</br>
**HTTP Method：** POST 

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|Body|必填|设备ID


**输出参数**

标准输出


**示例**

**请求样例**

```
Header：
	toC = POST url: https://uws.haier.net/rcs/label/toc/delete/labels-by-deviceId
	toC = Header:
	appId: MB-*****-0000
	accessToken: TGT1RALFFU4RMLAI27B7848DJWQPN0
	sign: f30684c4abbdc2acad7142317c9d2a931552425555c67fd2271c356eb2f937dd
	timestamp: 1595470190284
	clientId: testclientid
	apiversion:v1
	
	toB = POST url: https://uws.haier.net/rcs/label/tob/delete/labels-by-deviceId
	toB = Header:
	Content-Type: application/json;charset=UTF-8
	systemId: SV-*****-0000
	sign: f30684c4abbdc2acad7142317c9d2a931552425555c67fd2271c356eb2f937dd
	timestamp: 1595470190284
	apiversion:v1
Body:
	{
	    "deviceId":"123423424"
	}

```
**请求应答**
```
{
    "retCode":"00000",
    "retInfo":"成功"
}

```

**接口错误码**
> 23100、23110、23800、23810、23400、23410、23420、23430

</div>


## 错误码


| **错误码** | **描述** |
| :----------: |:----------:|
|23100| 必填参数不能为空|
|23110| 参数类型错误 |  
|23800| 数据库访问异常 | 
|23810| 远程调用失败 |
|23400| 用户不存在 |
|23410| 删除标签失败 |
|23420| 标签已存在 |
|23430| 不允许删除关联标签 |

