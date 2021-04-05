
#  公共参数

	公共参数说明了每个请求、应答应包含的公共字段。这些字段无特殊情况，不在相关服务类接口报文定义中重复说明。


**请求Header头**  

|参数名|	类型|位置|是否必填|说明|  
|:-----:|:-----:|:-----:|:-----:|--|
|appId|	String|	Header	|必填	|应用ID40位以内字符,Haier uHome 云平台全局唯一。开发者通过海极网申请获得。|
|appVersion	|String	|Header	|必填	|应用版本32 位字符,Haier uHome 云平台全局唯一。|  
|<font color="#FF0000">version</font>	|<font color="#FF0000">String</font>|<font color="#FF0000">Header</font>	|<font color="#FF0000">选填</font>|<font color="#FF0000">场景引擎服务必填，接口版本号,本期默认值：0.3</font>|  
|clientId	|String	|Header	|必填	|客户端ID27 位字符,客户端机编码与客户端 MAC 地址 拼合成唯一的客户端标识。 主要用途为唯一标识客户端 (例如,手机)。手机机编码为 IMEI 码。 手机 MAC 为 12 位地址。命名规范:客户端机编码(15 位)-客户 端 MAC 地址(12 位)格式: XXXXXXXXXXXXXXX-XXXXXXXXXXXX 举例: 356877020056553-08002700DC94。APP端可调用usdk获取，其他服务端自定义标识，不能为空。 |
|sequenceId	|String	|Header|必填	|报文流水(客户端唯一)客户端交易流水号。20 位, 前 14 位时间戳（格式：yyyyMMddHHmmss）,后 6 位流水 号。交易发生时,根据交易 笔数自增量。App应用访问uws接口时必须确保每次请求唯一，不能重复。|
|accessToken	|String	|Header|必填（登录后不为空，登录前可为空）|安全令牌 token，30 位字符。 用户登录 Haier U+ 云平台,由系统创建。用户退出 Haier U+ 云平台,由系统销毁。未登录时，访问不需要登录的平台接口，仍然需要传入本参数，参数值可为空或任意值（不超过30字符）|
|sign	|String|	Header|	必填|对请求进行签名运算产生的签名,签名算法见附录。|
|timestamp	|String	|Header	|必填|	应传入用户所在地时间戳，long型时间戳,精确到毫秒|
|language	|String	|Header|	必填	|该参数为多语言版提供支持。国内服务填写"zh-cn"即可。|
|timezone	|String|	Header|	必填	|代表客户端使用的时区。传入用户所在时区ID，国内服务请填写"Asia/Shanghai"即可|
|Content-Type|String|	Header|	必填	|互联网媒体信息，默认为"application/json;charset=UTF-8" |


**输出参数**

|参数名|	类型|位置|是否返回|说明|  
|:-----:|:-----:|:-----:|:-----:|--|
|retCode|	String	|Body|	是	|返回码（其中00000代表请求成功,其它代表错误，错误码及描述见公共错误码或服务内描述）|
|retInfo	|String	|Body	|是	|返回信息（用于调试的返回信息，不支持国际化，也不能直接显示在UI上）|



