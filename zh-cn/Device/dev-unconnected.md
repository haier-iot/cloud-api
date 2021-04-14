# 设备管理-非网器


## 绑定

**使用说明**

> 绑定https接口

**接口描述**  

?> **接入地址：** `/dcs/device-service-2c/add/user/non-network/device/binding`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
apiVersion|String|Header|必填|接口版本，此版本为v1
accessToken|String|Header|必填|用户身份令牌
appId|String|Header|必填|应用id
appVersion|String|Header|必填|应用版本
deviceId|String|Body|必填|设备身份标识(机编)
deviceName|String|Body|非必填|用户绑定设备名称
productCode|String|Body|必填|型号编码

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|body|必填|返回码（其中00000代表请求成功,其它代表错误，错误码及描述见附录错误码表）
retInfo|String|body|必填|用于调试的返回信息，不支持国际化，也不能直接显示在UI上
payload|Boolean|body|必填|返回数据：true，绑定成功；false，绑定失败


**示例**

**请求样例**

```
POST https://uws.haier.net/dcs/device-service-2c/add/user/non-network/device/binding
data:
	{
	    "deviceId":"deviceId",
	    "deviceName":"deviceName",
	    "productCode":"productCode"
	}
	[no cookies]
Request Headers:
	Connection:
	appId: MB-SBZXFWFWPTQ656-0000
	apiVersion: v1
	appVersion: 6.0.0
	clientId: test123456
	sequenceId: 20161020153428000015
	accessToken: TGT34DXO535N5UDV2IWPQGGJ6B2BV0
	sign:314ad2d5b240dad28e69acc1f012c0d915fb9ecb00f41b745e949b1c2abdb2f4
	timestamp: 1590053835901
	language: zh-cn
	timezone: +8
	appKey: 7a95f33dfe67d76cf79534b8d02093a7
	Content-Encoding: utf-8
	Content-type: application/json
	Content-Length: 34
	Host: 10.159.59.16:8844
	User-Agent: Apache-HttpClient/4.5.2 (Java/1.8.0_211)

```

**请求应答**

```
{
    "retCode":"00000",   
    "retInfo":"成功",   
    "payload":"true"
}

```


## 解绑

**使用说明**

> 解绑https接口

**接口描述**  

?> **接入地址：** `/dcs/device-service-2c/user/non-network/device/unbinding`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
apiVersion|String|Header|必填|接口版本，此版本为v1
accessToken|String|Header|必填|用户身份令牌
deviceId|String|Body|必填|设备身份标识(机编)

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|body|必填|返回码（其中00000代表请求成功,其它代表错误，错误码及描述见附录错误码表）
retInfo|String|body|必填|用于调试的返回信息，不支持国际化，也不能直接显示在UI上
payload|Boolean|body|必填|返回数据：true，解绑成功；false，解绑失败


**示例**

**请求样例**

```
POST https://uws.haier.net/dcs/device-service-2c/user/non-network/device/unbinding
data:
	{
	    "deviceId":"deviceId"
	}
	[no cookies]
Request Headers:
	Connection:
	appId: MB-SBZXFWFWPTQ656-0000
	apiVersion: v1
	clientId: test123456
	sequenceId: 20161020153428000015
	accessToken: TGT34DXO535N5UDV2IWPQGGJ6B2BV0
	sign:314ad2d5b240dad28e69acc1f012c0d915fb9ecb00f41b745e949b1c2abdb2f4
	timestamp: 1590053835901
	language: zh-cn
	timezone: +8
	appKey: 7a95f33dfe67d76cf79534b8d02093a7
	Content-Encoding: utf-8
	Content-type: application/json
	Content-Length: 34
	Host: 10.159.59.16:8844
	User-Agent: Apache-HttpClient/4.5.2 (Java/1.8.0_211)

```

**请求应答**

```
{
    "retCode":"00000",   
    "retInfo":"成功",   
    "payload":"true"
}

```


## 位置信息设置

**使用说明**

> 保存非网器设备位置信息

**接口描述**  

?> **接入地址：** `/dcs/device-service-2c/add/non-network/device/location/info`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
apiVersion|String|Header|必填|接口版本，此版本为v1
accessToken|String|Header|必填|用户身份令牌
deviceId|String|Body|必填|设备身份标识(机编)
longitude|Double|Body|必填|经度
latitude|Double|Body|必填|纬度
cityCode|String|Body|必填|城市编码

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|body|必填|返回码（其中00000代表请求成功,其它代表错误，错误码及描述见附录错误码表）
retInfo|String|body|必填|用于调试的返回信息，不支持国际化，也不能直接显示在UI上
payload|Boolean|body|必填|返回数据：true，设置成功；false，设置失败


**示例**

**请求样例**

```
POST https://uws.haier.net/dcs/device-service-2c/add/non-network/device/location/info
data:
	{
	    "deviceId":"deviceId",
	    "longitude":"longitude",
	    "latitude":"latitude",
	    "cityCode":"cityCode"
	}
	[no cookies]
Request Headers:
	Connection:
	appId: MB-SBZXFWFWPTQ656-0000
	apiVersion: v1
	clientId: test123456
	sequenceId: 20161020153428000015
	accessToken: TGT34DXO535N5UDV2IWPQGGJ6B2BV0
	sign:314ad2d5b240dad28e69acc1f012c0d915fb9ecb00f41b745e949b1c2abdb2f4
	timestamp: 1590053835901
	language: zh-cn
	timezone: +8
	appKey: 7a95f33dfe67d76cf79534b8d02093a7
	Content-Encoding: utf-8
	Content-type: application/json
	Content-Length: 34
	Host: 10.159.59.16:8844
	User-Agent: Apache-HttpClient/4.5.2 (Java/1.8.0_211)

```

**请求应答**

```
{
    "retCode":"00000",   
    "retInfo":"成功",   
    "payload":"true"
}
```


## 位置信息查询

**使用说明**

> 查询非网器设备位置信息

**接口描述**  

?> **接入地址：** `/dcs/device-service-2c/get/non-network/device/location/info`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
apiVersion|String|Header|必填|接口版本，此版本为v1
accessToken|String|Header|必填|用户身份令牌
deviceId|String|Body|必填|设备身份标识(机编)

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|body|必填|返回码（其中00000代表请求成功,其它代表错误，错误码及描述见附录错误码表）
retInfo|String|body|必填|用于调试的返回信息，不支持国际化，也不能直接显示在UI上
payload|ServiceDeviceLocationResultDto|body|必填|返回数据

**ServiceDeviceLocationResultDto：设备位置详细信息**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
longitude|Double|Body|是|经度
latitude|Double|Body|是|纬度
cityCode|String|Body|是|城市编码


**示例**

**请求样例**

```
POST https://uws.haier.net/dcs/device-service-2c/get/non-network/device/location/info
data:
	{
	    "deviceId":"deviceId"
	}
	[no cookies]
Request Headers:
	Connection:
	appId: MB-SBZXFWFWPTQ656-0000
	apiVersion: v1
	clientId: test123456
	sequenceId: 20161020153428000015
	accessToken: TGT34DXO535N5UDV2IWPQGGJ6B2BV0
	sign:314ad2d5b240dad28e69acc1f012c0d915fb9ecb00f41b745e949b1c2abdb2f4
	timestamp: 1590053835901
	language: zh-cn
	timezone: +8
	appKey: 7a95f33dfe67d76cf79534b8d02093a7
	Content-Encoding: utf-8
	Content-type: application/json
	Content-Length: 34
	Host: 10.159.59.16:8844
	User-Agent: Apache-HttpClient/4.5.2 (Java/1.8.0_211)

```

**请求应答**

```
{   
    "retCode":"00000",   
    "retInfo":"成功",   
    "payload":{
        "longitude": "longitude",
        "latitude": "latitude",
        "cityCode":"cityCode"
    }
}

```


## 耗材信息系查询

**使用说明**

> 非网器耗材信息查询

**接口描述**  

?> **接入地址：** `/dcs/device-service-2c/get/non-network/device/material/info`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
apiVersion|String|Header|必填|接口版本，此版本为v1
accessToken|String|Header|必填|用户身份令牌
productCode|String|Body|必填|型号编码

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|body|必填|返回码（其中00000代表请求成功,其它代表错误，错误码及描述见附录错误码表）
retInfo|String|body|必填|用于调试的返回信息，不支持国际化，也不能直接显示在UI上
payload|List<ServiceOrdinaryDeviceMaterialDto>|body|必填|返回数据

**ServiceOrdinaryDeviceMaterialDto：设备耗材结构**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
name|String|Body|是|耗材名称
code|String|Body|是|耗材标识
propDesc|List<ServiceOrdinaryDeviceMaterialPropDto>|Body|是|耗材属性列表，耗材属性最大可添加20条
saleUrl|String|Body|是|购买地址

**ServiceOrdinaryDeviceMaterialPropDto：设备耗材名称和属性值对应**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
name|String|Body|是|耗材名称
value|String|Body|是|耗材属性值

**示例**

**请求样例**

```
POST https://uws.haier.net/dcs/device-service-2c/get/non-network/device/material/info
data:
	{
	    "productCode":"AU00989UIY"
	}
	[no cookies]
Request Headers:
	Connection:
	appId: MB-SBZXFWFWPTQ656-0000
	apiVersion: v1
	clientId: test123456
	sequenceId: 20161020153428000015
	accessToken: TGT34DXO535N5UDV2IWPQGGJ6B2BV0
	sign:314ad2d5b240dad28e69acc1f012c0d915fb9ecb00f41b745e949b1c2abdb2f4
	timestamp: 1590053835901
	language: zh-cn
	timezone: +8
	appKey: 7a95f33dfe67d76cf79534b8d02093a7
	Content-Encoding: utf-8
	Content-type: application/json
	Content-Length: 34
	Host: 10.159.59.16:8844
	User-Agent: Apache-HttpClient/4.5.2 (Java/1.8.0_211)

```

**请求应答**

```
{   
    "retCode":"00000",   
    "retInfo":"成功",   
    "payload":[
             {
                "name": "PPC复合滤芯",
                 "code": "F70WV4000",
                 "propDesc": [
                      {
                          "name": "滤芯标识位",
                          "value": "priFilterSelect"
                     },
                     {
						  "name": "滤芯级数",
                          "value": "第1级"
                     }
					],
					"saleUrl":"https://m.ehaier.com/sgmobile/goodsDetails?container_type=3&productId=36061"
		}
	]
}

```