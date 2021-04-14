# 设备拓扑关系

## 根据DeviceId查询拓扑关系

**使用说明**

根据DeviceId和token查询设备拓扑关系，如果用户对该设备有权限继续查询，反之返回错误码(1200001: 当前用户与该设备不匹配);


**接口描述**  

?> **接入地址：** `/dcs/device-service-2c/get/device/topological/relation`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|Body|是|设备身份标识
accessToken|String|Header|是|用户token信息
apiVersion|String|Header|是|接口版本，此版本为v1.

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|是|返回码
retInfo|String|Body|是|用于调试的返回信息，不支持国际化，也不能直接显示在UI上
payload|Map<String,List<ServiceDeviceTopologicalResultDto>|Body|是|用户对该设备没有权限，返回错误码1200001<br/>查询的设备不在线，返回错误码1200002<br/>查询到的拓扑关系列表中，(1)如果设备离线则该条记录不返回; (2)如果用户对设备无操作权限，只返回desc,其中desc值为"未知设备",其他字段不返回。<br/>具体返回结构见5.1 请求示例返回返回结构中，key:  father 表示该设备的父设备  child表示该设备的子设备


**示例**

**请求样例**
```
POST https://uws.haier.net/dcs/device-service-2c/get/device/topological/relation
data:
	{
	    "deviceId":"DC330D861B7C"
	}
[no cookies]
Request Headers:
	Connection:keep-alive
	appId: ************************
	apiVersion: v1
	clientId: test123456
	sequenceId: 20161020153428000015
	accessToken: ************************
	sign:314ad2d5b240dad28e69acc1f012c0d915fb9ecb00f41b745e949b1c2abdb2f4
	timestamp: 1590053835901
	language: zh-cn
	timezone: +8
	Content-Encoding: utf-8
	Content-type: application/json

```

**请求应答**
```
{   
    "retCode":"00000",   
    "retInfo":"成功",   
    "payload":{
    "father": [
      {
        "deviceId": "deviceId1",
        "familyName": "familyName1",
        "productCode": "productCode",
        "typeId": "typeId",
        "productImg": "productImg"
      },
     {
       "desc": "未知设备"
     }
  ],
  "child": [
    {
      "deviceId": "deviceId1",
      "familyName": "familyName1",
      "productCode": "productCode",
      "typeId": "typeId",
      "productImg": "productImg"
    },
    {
      "desc": "未知设备"
    }
  ]
 }

}

```


