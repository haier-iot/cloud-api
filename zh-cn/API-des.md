#  调用API说明

## 使用前提
业务方使用UWS服务，必须在海极网（ http://www.haigeek.com/ ）上提前创建应用，不同业务，创建不同应用；
应用上线需要经过海极网审核认证，未经过审核认证的服务，海极网有权随时下线该应用访问UWS服务权限；

## 接入协议    

对外提供的服务统一使用HTTPS协议，默认使用443端口， 在服务端使用TSL进行单向加解密处理。服务调用方在进行调用时，无需下载或安装证书。

## 接入地址    


> 国内平台环境域名：`https://uws.haier.net`  
> 东南亚平台环境域名：`https://uws-sea.haieriot.net`  
> 北美平台环境域名：`https://uws-gea-us.haieriot.net`  
> 欧洲平台环境域名:`https://uws-euro.haieriot.net`



## 安全规范

1. **数据安全**

App应用要保证自己接入UWS时通过海极网获取的appid、appkey等信息的使用安全，出现因外泄导致违规使用UWS服务而被平台禁用的风险由APP开发者自行承担。
App通过UWS获取的数据等要确保存储安全。

2. **接口访问安全**

要严格按照UWS接口服务的使用文档调用,开发相关安全参照“海尔优家安全开发规范V1.0.pdf”执行。


## 数据类型

1. **字段类型说明**

|限定类型|	说明|	格式|	json示例|
|---|---|---|---|
|DateTime	|日期时间类型的字符串|	`yyyy-MM-dd hh:mm:ss`| `	{“lgTime”:“2013-10-08 08:00:00”}`|
|Date	|日期类型的字符串	|`yyyy-MM-dd` 	|`{“lgDate”:“2013-10-08”}`|
|String	|字符串		|&nbsp;|`{“address”:“street 123”}`|
|int	|整形数字	|&nbsp;|	`{“age”:1234}`|
|long	|长整形数字	|&nbsp;|	`{“oid”:1234567890123}`|
|double	|浮点数数字	|&nbsp;|	`{“price”:12.35}`|
|boolean	|布尔型（true或false）	|&nbsp;|	`{“idOld”:true}`|

2.**null值说明**  

为避免解析错误， uws各接口的返回参数，不返回null值。
必填参数，无论输入还是输出，必须有值，不能为null
非必填参数，则说明如下：
数值类型数据（int、long、double）只会返回数字，包括正数、零及负数。
布尔类型数据（boolean）只返回true和false
以上基本类型本身不包含null值。

DateTime、Date、String及结构体类型的数据，如果为null时，所对应的属性将不返回。

**DateTime类型**  

birthday为DateTime类型，不为null时：

    {"name":"Tom","age":23,"birthday":"2013-10-08 08:00:00","address":{"city":"beijing","street":"haidian"} }

birthday为null时，则birthday属性不返回

    {"name":"Tom","age":23, "address":{"city":"beijing","street":"haidian"} }  

**String类型**  

name是String类型，不为null时：

    {"name":"Tom","age":23,"birthday":"2013-10-08 08:00:00","address":{"city":"beijing","street":"haidian"} }

name为null时，则name属性不返回

    {"age":23,"birthday":"2013-10-08 08:00:00","address":{"city":"beijing","street":"haidian"} }


**结构体类型**

address为结构体类型，不为null时：
  
    {"name":"Tom","age":23,"birthday":"2013-10-08 08:00:00","address":{"city":"beijing","street":"haidian"} }

address为null时，address属性不返回

	{"name":"Tom","age":23,"birthday":"2013-10-08 08:00:00" }


## 	服务接口健壮性要求

**数据解析要求**  

为保证服务API升级，接口中字段，不影响业务方使用，业务方需要处理数据时，严禁按照固定格式解析；



