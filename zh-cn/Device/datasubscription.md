#  设备权限

## 绑定设备

**使用说明**

> 用户绑定设备的接口  <font color=red>（单用户绑定设备数量<=300个，绑定时设备必须平台上线。）</font>

**接口描述**

?> **接入地址：** `/uds/v1/protected/bindDevice`</br>
**HTTP Method：** POST 

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
deviceId|String|body|必填|设备ID 
name|String|body|必填|设备名称
data|String|body|必填|绑定加密数据


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|是|返回码，00000代表请求成功，其它值代表错误
retInfo|String|Body|是|错误描述信息，本描述信息仅是用于调试的返回信息，不支持国际化，不能直接显示在UI上，字符串长度范围：0~256


**示例**

**请求样例**

```
请求地址：/uds/v1/protected/bindDevice
Header：
    Connection: keep-alive
    appId: MB-****-0000
    appVersion: 99.99.99.99990
    clientId: 123
    sequenceId: 2014022801010
    accessToken: TGT1ANW5WCQ2SXRD2DGIYRRAVLOMS0
    sign: bb2a5c1e432eac8bea8eecb89b408937382e7e95486ee0a60944a46504fa0015
    timestamp: 1491013448628 
    language: zh-cn
    timezone: +8
    appKey: 6cdd4658b8e7dcedf287823b94eb6ff9
    Content-Encoding: utf-8
    Content-type: application/json
Body
{
    "deviceId": "************",
    "name": "test002",
    "data": "5f10bf4f5af08db934c8165c32140227"
}

```
**请求应答**
```
{
    "retCode":"00000",
    "retInfo":"用户绑定设备成功"
}

```

**接口错误码**
> A00001、B00001、G20202、A00004、B00001、D00006、G20904、G20908、G20910、G20914

</div>