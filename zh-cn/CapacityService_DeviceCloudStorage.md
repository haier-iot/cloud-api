!>  **当前版本**：[云存储Rest接口说明书V1.0.0][CapacityService_DeviceCloudStorage_document_url]  
 **发布时间**：

### 简介
>云存储服务用于存储智能设备的资源管理服务。智能互联设备可以通过此服务对文件、存储区进行操作。

![云存储Rest接口图片][CapacityService_DeviceCloudStorage_type]

### 名词解释


### 功能介绍
**针对设备的服务**</br>

1、存储区管理：文件资源的存储是基于对存储区域的管理，存储区是保证后续文件可以上传、下载等操作的基础存储服务，使用云服务存储首先需要通过服务建立存储区。</br>

2、资源管理：存储云端的文件存储与获取资源服务能力。包括对文件信息的管理与文件资源内容的获取。</br>

**针对应用的服务**</br>

本服务针对应用只提供了文件的查看功能，可以查看智能互联设备上传的文件资源。</br>


## 公共结构
### BuckInfo

参数名|类型|说明
:-|:-:|:-
bucketId|int|id
bucketName|String|name
privilegeType|int|权限：</br>0，私有；</br>1，公共读
createTime|Data|创建时间

### BucketList
参数名|类型|说明
:-|:-:|:-
buckets|BucaketInfo[]|存储区列表
total|int|记录总数
nextMarker|int|下一页页码，</br>第一页从0开始</br>最后一页或无数据返回时为-1

### FileInfo
参数名|类型|说明
:-|:-:|:-
fileId|long|id
fileName|String|name
privilegeType|int|权限：</br>0，私有；</br>1，公共读；</br>2，缺省（同所在存储区权限）
fileSize|long|文件大小
bucketId|int|所属存储区id
creteTime|Data|创建时间

### FileList
参数名|类型|说明
:-|:-:|:-
infos|FileInfo[]|信息列表
total|int|文件记录总数
nextMarker|int|下一页页码，</br>第一页从0开始</br>最后一页或无数据返回时为-1


## 接口清单

### 针对设备的服务

#### 创建存储区

> 主用户用来创建存储区，保证后续文件上传下载等文件存储功能；</br>
> 存储区名字智能为小写字母或数字或短横线，必须以小写字母或数字开通和结尾，长度在3~63个字符之间；</br>
> 存储区一旦创建后名字不能修改；</br>
> 同一用户下的存储区不能重名。</br>

##### 1、接口定义
?> **接入地址：** `/css/v1/createBucket`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
bucketName|String|Body|必填|要创建的存储区名字
Privilege|int|Body|必填|权限：</br>0，私有；</br>1，公共读
accessKey|String|header|必填|用户主ak

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
bucketInfo|BucketInfo|Body|必填|创建成功的存储区

##### 2、请求样例

**请求样例**
```
请求地址：http://123.103.113.62/css/v1/createBucket
Header：
	accessKey: 1681314738825497:2K0MymVucAOQ==
Body：
{
	"bucketName":"haier099",
	"privilegeType":1
}

```

**请求应答**
```
Body：
{
    "bucketInfo":
    {
        "bucketId":123,
        "bucketName":"haier099",
        "createTime":"2017-01-12 19:49:39",
        "privilegeType":1
    },
    "retCode":"00000",
    "retInfo":"成功"
}
```

#### 查看存储区
> 根据存储区id查询存储区信息；存储区必须存在，且当前用户拥有访问权限

##### 接口定义
?> **接入地址：** `/css/v1/queryBucket/{bucketId}`</br>
**HTTP Method：** GET

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
bucketId|int|url|必填|存储区ID
accessKey|String|header|必填|用户主ak

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
bucketInfo|BucketInfo|Body|必填|


##### 2、请求样例

**请求样例**
```
接入地址：http://123.103.113.62/css/v1/queryBucket/107
Header：
	accessKey: 1681314738825497:2K0MymVucAOQ==
```

**请求应答**
```
Body：
{
    "bucketInfo":
    {
        "bucketId":107,
        "bucketName":"haier009",
        "createTime":"2017-01-10 18:41:30",
        "privilegeType":1
    },
    "retCode":"00000",
    "retInfo":"成功"
}
```

#### 修改存储区权限

> 根据存储区id修改存储区的权限，可选值为：0，私有；1，公共读；</br>
> 存储区必须存在，且当前用户拥有访问权限；权限值只能为0 or 1,其它值报错

##### 1、接口定义
？> **接入地址：** `/css/v1/modifyBucketACL/{bucketId}`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
bucketId|int|url|必填|要修改的存储区ID
privilegeType|int|Body|必填|权限：</br> 0，私有；</br>1，公共读
acessKey|String|header|必填|用户主ak

**输出参数** ：标准输出参数

##### 2、请求样例

**请求样例**
```
接入地址：http://123.103.113.62/css/v1/modifyBucketACL/123
Header：
	accessKey: 1681314738825497:2K0MymVucAOQ==
Body：
{
	"privilegeType":0
}
```

**请求应答**
```
Body：
{
    "retCode": "00000", 
    "retInfo": "成功"
 }
```

#### 删除存储区

> 根据存储区id删除该存储区，但需要里面没有文件。否则删除不了；</br>
> 存储区必须存在，且当前用户拥有访问权限；</br>
> 如果存储区里还有文件，则不能被删除，会抛出相应的异常信息</br>

##### 1、接口定义
?> **接入地址：** `/css/v1/deleteBucket/{bucketId}`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
bucketId|int|url|必填|要删除的存储区ID
accessKey|String|header|必填|用户主ak

**输出参数** ：标准输出参数


##### 2、请求样例

**请求样例**
```
请求地址：http://123.103.113.62/css/v1/deleteBucket/107
Header：
	accessKey: 1681314738825497:2K0MymVucAOQ==
```

**请求应答**
```
Body：
{
    "retCode": "00000", 
    "retInfo": "成功"
 }
```

#### 获取存储区列表

> 根据当前用户的accessKey信息查询其创建的所有存储区；</br>
> 按照创建时间降序返回当前用户拥有所有权的存储区列表，超过一页翻页展示，每页20条


##### 1、接口定义
?> **接入地址：** `/css/v1/bucketList？pageNo={pageNo}`</br>
**HTTP Method：** GET

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
pageNo|int|Param|必填|查询请求当前页码，从0开始
accessKey|String|header|必填|用户主ak

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
bucketList|BucketList|Body|必填|


##### 2、请求样例

**请求样例**
```
请求地址：http://123.103.113.62/css/v1/bucketList?pageNo=0
Header：
	accessKey: 1681314738825497:2K0MymVucAOQ==
```

**请求应答**
```
Body：
{
    "bucketList":
    {
        "buckets":
        [
            {
                "bucketId":123,
                "bucketName":"haier099",
                "createTime":"2017-01-12 19:49:39",
                "privilegeType":0
            },
            {
                "bucketId":119,
                "bucketName":"haiertestbucket",
                "createTime":"2017-01-12 14:31:10",
                "privilegeType":0
            },
            {
                "bucketId":115,
                "bucketName":"testasdf",
                "createTime":"2017-01-12 14:01:56",
                "privilegeType":0
            },
            {
                "bucketId":111,
                "bucketName":"testasd",
                "createTime":"2017-01-12 13:56:29",
                "privilegeType":0
            },
            {
                "bucketId":105,
                "bucketName":"haier007",
                "createTime":"2017-01-10 17:25:19",
                "privilegeType":0
            },
            {
                "bucketId":101,
                "bucketName":"haier005",
                "createTime":"2017-01-10 16:48:45",
                "privilegeType":1
            },
            {
                "bucketId":83,
                "bucketName":"haier001",
                "createTime":"2017-01-06 14:54:08",
                "privilegeType":1
            },
            {
                "bucketId":81,
                "bucketName":"enjoyfree12",
                "createTime":"2017-01-06 11:23:25",
                "privilegeType":1
            },
            {
                "bucketId":77,
                "bucketName":"enjoyfree11",
                "createTime":"2017-01-06 11:16:16",
                "privilegeType":1
            },
            {
                "bucketId":73,
                "bucketName":"enjoyfree8",
                "createTime":"2017-01-05 18:49:26",
                "privilegeType":1
            },
            {
                "bucketId":71,
                "bucketName":"enjoyfree7",
                "createTime":"2017-01-05 18:38:34",
                "privilegeType":1
            },
            {
                "bucketId":69,
                "bucketName":"enjoyfree6",
                "createTime":"2017-01-05 18:37:21",
                "privilegeType":1
            },
            {
                "bucketId":67,
                "bucketName":"enjoyfree5",
                "createTime":"2017-01-05 18:36:33",
                "privilegeType":1
            },
            {
                "bucketId":65,
                "bucketName":"enjoyfree4",
                "createTime":"2017-01-05 18:34:40",
                "privilegeType":1
            },
            {
                "bucketId":63,
                "bucketName":"enjoyfree3",
                "createTime":"2017-01-05 18:33:58",
                "privilegeType":1
            },
            {
                "bucketId":61,
                "bucketName":"enjoyfree2",
                "createTime":"2017-01-05 18:16:42",
                "privilegeType":1
            },
            {
                "bucketId":59,
                "bucketName":"enjoyfree1",
                "createTime":"2017-01-05 17:54:17",
                "privilegeType":1
            },
            {
                "bucketId":57,
                "bucketName":"enjoyfree",
                "createTime":"2017-01-05 17:50:09",
                "privilegeType":1
            },
            {
                "bucketId":55,
                "bucketName":"hello12131",
                "createTime":"2017-01-05 17:37:11",
                "privilegeType":0
            }
        ],
        "nextMarker":-1,
        "total":19
    },
    "retCode":"00000",
    "retInfo":"成功"
}
```




## 使用方式

### 开通流程

![开通流程][CapacityService_DeviceCloudStorage_liucheng]


### 应用场景
适用于智能互联设备文件上传与存储服务，为设备在云平台服务端建立存储空间，用于存储图片、文本等各种文件资源，为智能设备建立统一的云端设备存储服务。

## 文档资料
[云存储Rest接口说明书V1.0.0][CapacityService_DeviceCloudStorage_document_url]

## 常见问题



[^-^]:文本连接注释
[CapacityService_DeviceCloudStorage_document_url]:_document/_CapacityService_DeviceCloudStorage/

[^-^]:常用图片注释
[CapacityService_DeviceCloudStorage_type]:_media/_CapacityService_DeviceCloudStorage/CapacityService_DeviceCloudStorage_type.png

[CapacityService_DeviceCloudStorage_liucheng]:_media/_CapacityService_DeviceCloudStorage/CapacityService_DeviceCloudStorage_liucheng.png

