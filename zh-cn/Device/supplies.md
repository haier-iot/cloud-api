#  设备耗材管理

## 查询设备耗材信息

**使用说明**

> 查询设备耗材信息

**接口描述**

?> **接入地址：** `/dcs/netdeviceinfo/find/device/consumables`</br>
**HTTP Method：** POST 

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
productCode|String|Body|必填|产品编码  
pageNum|int|Body|非必填|分页页码 
pageSize|int|Body|非必填|分页大小 


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
payload|List<Consumable >|Body|必填|返回数据


**Consumable**  

| **名称** | 设备耗材信息 |&emsp;| Consumable |   
| :----------: |:----------:|:-----:|:--------:|
|**字段名**|**类型**|**说明**|**备注**|  
|name| String | 耗材名称 |&emsp;|  
|code| String | 耗材标识 |&emsp;|  
|propDesc| List<PropDesc> | 属性标识描述 |&emsp;|    
|saleUrl| String | 耗材购买链接 |&emsp;|  


**PropDesc**

| **名称** | 属性标识描述 |&emsp;| PropDesc |   
| :----------: |:----------:|:-----:|:--------:|
|**字段名**|**类型**|**说明**|**备注**|  
|name| String | 属性名称 |&emsp;|  
|value| String | 属性值 |&emsp;|  


**示例**

**请求样例**

```
Header:
	appId: MB-*****-0000
	appVersion: 01.00.00.00000
	clientId: 111111111111
	accessToken: TGT3EM9DNXY65GYY20VNX6077ISHG0
	sign:37b1b9e18c9b12481e6ebcdec9c4e5f3b8da80fbc52930ae13efcdb57f159e31
	timestamp: 1585577435707 
	Content-Encoding: utf-8
	Content-type: application/json
Body:
	{
		"productCode":"1233333333"
	}

```
**请求应答**
```
{
  "retCode": "00000",
  "retInfo": "正确",
  "payload": [{
            "name":"滤芯325",
            "code":"1GE20D992SBAU",
            "propDesc":[
                {
                    "name":"xxx",
                    "value":"110"
                },
                {
                    "name":"xxx2",
                    "value":"120"
                }
            ],
            "saleUrl":"https://www.haigeek.com:443/resource/selfService/ "
        } ] 
}

```

**接口错误码**
> B00001、G20202、B00004、A00001、000001、A00006、A00005、A00007、A00008、A00009

</div>