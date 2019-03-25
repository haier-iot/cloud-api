
>  **当前版本**：[UWS 智能设备资源云存储 V1.0.0](zh-cn/ChangeLog/CapacityService_DeviceCloudStorage)  
 **更新时间**：{docsify-updated}  

### 简介
>云存储服务用于存储智能设备的资源管理服务。智能互联设备可以通过此服务对文件、存储区进行操作。

![云存储Rest接口图片][CapacityService_DeviceCloudStorage_type]

**针对设备的服务**</br>

1、存储区管理：文件资源的存储是基于对存储区域的管理，存储区是保证后续文件可以上传、下载等操作的基础存储服务，使用云服务存储首先需要通过服务建立存储区。</br>

2、资源管理：存储云端的文件存储与获取资源服务能力。包括对文件信息的管理与文件资源内容的获取。</br>

**针对应用的服务**</br>

本服务针对应用只提供了文件的查看功能，可以查看智能互联设备上传的文件资源。</br>


### 应用场景
适用于智能互联设备文件上传与存储服务，为设备在云平台服务端建立存储空间，用于存储图片、文本等各种文件资源，为智能设备建立统一的云端设备存储服务。




## 公共结构

### accesskey

accesskey为接口头信息，术语公共信息每个请求、应答应该包含

参数名|类型|位置|说明
:-|:-:|:-:|:-
accessKey|String|Header|云存储accessKey，由AccesskeyId与加密密文两部分组成，</br>accessKey=AccesskeyId:密文，加密密文采用AES对称加密</br>密文 = base64 (AccesskeyId+body)</br>秘钥 = AccesskeySecret</br>Body为空时传空字符。


### BuckInfo

参数名|类型|说明
:-|:-:|:-
bucketId|int|id
bucketName|String|name
privilegeType|int|权限：0，私有；1，公共读
createTime|Data|创建时间

### BucketList
参数名|类型|说明
:-|:-:|:-
buckets|BucaketInfo[]|存储区列表
total|int|记录总数
nextMarker|int|下一页页码，第一页从0开始,最后一页或无数据返回时为-1

### FileInfo
参数名|类型|说明
:-|:-:|:-
fileId|long|id
fileName|String|name
privilegeType|int|权限：0，私有；1，公共读；2，缺省（同所在存储区权限）
fileSize|long|文件大小
bucketId|int|所属存储区id
creteTime|Data|创建时间

### FileList
参数名|类型|说明
:-|:-:|:-
infos|FileInfo[]|信息列表
total|int|文件记录总数
nextMarker|int|下一页页码，第一页从0开始,最后一页或无数据返回时为-1


## 接口清单

### 针对设备的服务

#### 创建存储区

> 1.主用户用来创建存储区，保证后续文件上传下载等文件存储功能；</br>
> 2.存储区名字智能为小写字母或数字或短横线，必须以小写字母或数字开通和结尾，长度在3~63个字符之间；</br>
> 3.存储区一旦创建后名字不能修改；</br>
> 4.同一用户下的存储区不能重名。</br>

##### 1、接口定义
?> **接入地址：** `/css/v1/createBucket`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
bucketName|String|Body|必填|要创建的存储区名字
Privilege|int|Body|必填|权限：0，私有；1，公共读
accessKey|String|header|必填|用户主ak

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
bucketInfo|BucketInfo|Body|必填|创建成功的存储区

##### 2、请求样例

**请求样例**
```
请求地址：http://*****/css/v1/createBucket
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
> 1.根据存储区id查询存储区信息；</br>
> 2.存储区必须存在，且当前用户拥有访问权限

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
接入地址：http://******/css/v1/queryBucket/107
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

> 1.根据存储区id修改存储区的权限，可选值为：0，私有；1，公共读；</br>
> 2.存储区必须存在，且当前用户拥有访问权限；权限值只能为0 or 1,其它值报错

##### 1、接口定义
?>  **接入地址：** `/css/v1/modifyBucketACL/{bucketId}`</br>
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
接入地址：http://*********/css/v1/modifyBucketACL/123
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

> 1.根据存储区id删除该存储区，但需要里面没有文件。否则删除不了；</br>
> 2.存储区必须存在，且当前用户拥有访问权限；</br>
> 3.如果存储区里还有文件，则不能被删除，会抛出相应的异常信息</br>

##### 1、接口定义
?> **接入地址：**  `/css/v1/deleteBucket/{bucketId}`</br>
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
请求地址：http://***********/css/v1/deleteBucket/107
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

> 1.根据当前用户的accessKey信息查询其创建的所有存储区；</br>
> 2.按照创建时间降序返回当前用户拥有所有权的存储区列表，超过一页翻页展示，每页20条


##### 1、接口定义  

?> **接入地址：** `/css/v1/bucketList？pageNo={pageNo}`
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
请求地址：http://*******/css/v1/bucketList?pageNo=0
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



#### 获取存储区内文件

> 1.根据存储区id查询存储区里面的文件列表，按照文件创建时间降序排列，超过1页则翻页，每页20条，最多可以翻页到499页；</br>
> 2.存储区必须存在，且当前用户拥有访问权限

##### 1、接口定义
?> **接入地址：** `/css/v1/fileList/{bucketId}?pageNo={pageNo}`</br>
**HTTP Method：** GET

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
bucketId|int|url|必填|存储区ID
pageNo|int|Param|必填|查询请求当前页码，从0开始
accessKey|String|header|必填|用户主ak

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
fileList|FileList|Body||


##### 2、请求样例

**请求样例**
```
接入地址：http://*******/css/v1/fileList/119?pageNo=0
Header：
	accessKey: 1681314738825497:2K0MymVucAOQ==
```

**请求应答**
```
Body：
{
    "fileList":
    {
        "infos":
        [
            {
                "bucketId":119,
                "createTime":"2017-01-12 18:54:09",
                "fileId":101844908215,
                "fileName":"a.jpg",
                "fileSize":845941,
                "meta":
                {
                    "macid":"00-0C-29-55-56-B9",
                    "province":"q\u001C\u0001",
                    "userid":"12345",
                    "district":"\u0002q:",
                    "city":"R\u009B\u0002"
                },
                "privilegeType":0
            },
            {
                "bucketId":119,
                "createTime":"2017-01-12 18:53:53",
                "fileId":101843303252,
                "fileName":"a.jpg",
                "fileSize":845941,
                "meta":
                {
                },
                "privilegeType":0
            },
            {
                "bucketId":119,
                "createTime":"2017-01-12 18:53:28",
                "fileId":101840817636,
                "fileName":"asa.jpg",
                "fileSize":845941,
                "meta":
                {
                },
                "privilegeType":0
            },
            {
                "bucketId":119,
                "createTime":"2017-01-12 18:52:14",
                "fileId":101833419283,
                "fileName":"a.jpg",
                "fileSize":845941,
                "meta":
                {
                    "macid":"00-0C-29-55-56-B9",
                    "province":"q\u001C\u0001",
                    "userid":"12345",
                    "district":"\u0002q:",
                    "city":"R\u009B\u0002"
                },
                "privilegeType":0
            },
            {
                "bucketId":119,
                "createTime":"2017-01-12 17:50:06",
                "fileId":101460633186,
                "fileName":"a.jpg",
                "fileSize":114391,
                "meta":
                {
                    "macid":"00-0C-29-55-56-B9",
                    "province":"q\u001C\u0001",
                    "userid":"12345",
                    "district":"\u0002q:",
                    "city":"R\u009B\u0002"
                },
                "privilegeType":0
            },
            {
                "bucketId":119,
                "createTime":"2017-01-12 17:32:54",
                "fileId":101357479206,
                "fileName":"a.jpg",
                "fileSize":114391,
                "meta":
                {
                    "macid":"00-0C-29-55-56-B9",
                    "province":"q\u001C\u0001",
                    "userid":"12345",
                    "district":"\u0002q:",
                    "city":"R\u009B\u0002"
                },
                "privilegeType":0
            },
            {
                "bucketId":119,
                "createTime":"2017-01-12 17:29:34",
                "fileId":101337486533,
                "fileName":"a.jpg",
                "fileSize":114391,
                "meta":
                {
                    "macid":"00-0C-29-55-56-B9",
                    "province":"q\u001C\u0001",
                    "userid":"12345",
                    "district":"\u0002q:",
                    "city":"R\u009B\u0002"
                },
                "privilegeType":0
            },
            {
                "bucketId":119,
                "createTime":"2017-01-12 17:28:33",
                "fileId":101331317320,
                "fileName":"a.jpg",
                "fileSize":114391,
                "meta":
                {
                    "macid":"00-0C-29-55-56-B9",
                    "province":"q\u001C\u0001",
                    "userid":"12345",
                    "district":"\u0002q:",
                    "city":"R\u009B\u0002"
                },
                "privilegeType":0
            },
            {
                "bucketId":119,
                "createTime":"2017-01-12 16:56:28",
                "fileId":101138873157,
                "fileName":"1.jpg",
                "fileSize":879394,
                "meta":
                {
                    "city":"R\u009B"
                },
                "privilegeType":1
            },
            {
                "bucketId":119,
                "createTime":"2017-01-12 14:52:08",
                "fileId":100392893042,
                "fileName":"yuanminyuan.jpg",
                "fileSize":108566,
                "meta":
                {
                    "city":"R\u009B"
                },
                "privilegeType":0
            }
        ],
        "nextMarker":-1,
        "total":10
    },
    "retCode":"00000",
    "retInfo":"成功"
}
```

#### 上传文件及meta信息

> 把一个本地文件上传到云存储服务的指定存储区中：</br>
> 1.存储区名字符合要求（具体参见创建存储区），如存储区存在，则上传文件到此存储区，如不存在，则自动创建此名字的存储区，权限为私有。</br>
> 2.文件名使用UTF-8编码，长度必须在1-100字符之间。不能包含/\|;*?"<>字符。</br>
> 3.文件大小范围为1-5Mbytes</br>
> 4.每条Meta信息包含key/value值对的json格式字符串，key不能为空，整个Meta信息的json字符串长度不能超过4000；key重复将导致value会被覆盖,</br>
> 目前预定义的meta的key有：</br>省，province;</br>  市,city；</br>  区县，district；</br>  Mac标识，macid；</br>   User标识，userid；</br>    其它meta的key可以自定义

##### 1、接口定义
?> **接入地址：** `/css/v1/uploadFile/{bucketName}`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
filename|String|Header|非必填|文件下载时的名字，此处不传或者填“”（任意个空格）则为本地文件名；</br>也可以在此重命名，重命名后的文件名需使用UTF-8编码，文件名整个长度（含后缀）在1-100字节之间。不能包含/\  &#124;；*?"<>字符。</br>需要urlencode编码：  `URLEncoder.encode(meta, "UTF-8")`。</br>另外，本地文件名暂时只支持英文字符，其他字符可能产生乱码问题。
privilegeType|int|Header|必填|权限信息
meta|String|Header|非必填|文件meta信息，传json,需要urlecdoe编码：`URLEncoder.encode(meta, "UTF-8")`
accessKey|String|Header|必填|用户主ak
file|File|Body|必填|文件

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
fileInfo|FileInfo|Body|是|

##### 2、请求样例

**请求样例**
```
接入地址：http://*******/css/v1/uploadFile/test-123
Header：
	accessKey: 1681314738825497:2K0MymVucAOQ==
	fileName: 青蛙王子.jpg
	privilegeType: 1
	meta:
	{
		"province":"qd",
		"city":"qd",
		"macid":"00-0C-29-55-56-B9",
		"district":"hs",
		"userid":"12345"
}	
```

**请求应答**
```
Body：
{
    "fileInfo":
    {
        "bucketId":123,
        "createTime":"2017-01-12 20:26:45",
        "fileId":102400553047,
        "fileName":"RÙ\u008BP.jpg",
        "fileSize":63550,
        "meta":
        {
            "macid":"00-0C-29-55-56-B9",
            "userid":"12345",
            "province":"qd",
            "district":"hs",
            "city":"qd"
        },
        "privilegeType":0
    },
    "retCode":"00000",
    "retInfo":"成功"
}
```

#### 按meta搜索文件

> 1.文件上传时可以带有文件相关的meta信息。</br>
> 2.用户可以根据指定的meta值（多个是and的关系），上传时间（到天） ，把当前用户所有的存储文件中满足条件的文件列表搜索出来，超过一页分页显示，每页10条。</br>
> 3.只能在当前用户拥有所有权的文件中搜索，条件中可以输入多个meta值，它们是and的关系。超过一页则分页展示，每页10条，最多可以翻页到999页

##### 1、接口定义
?> **接入地址：** `/css/v1/fileListByMeta?pageNo={pageNo}`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
startTime|Data|Body|可选|开始时间，格式为yyy-MM-dd HH : mm:ss
endTime|Data|Body|可选|结束时间，格式为yyy-MM-dd HH : mm:ss
meta|Map|Body|可选|Meta信息
pageNo|int|Param|必填|差选请求当前页码，从0开始
accessKey|String|Header|必填|用户主ak


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
fileList|FileList|Body|必填|

MetaInfo类型的对象，包含属性：MAP<String,String> 属性值 描述文件相关信息

##### 2、请求样例

**请求样例**
```
接入地址：http://*******/css/v1/fileListByMeta?pageNo=0
Header：
	accessKey: 1681314738825497:2K0MymVucAOQ==
Body：
	{
		"startTime":"2017-01-20",
		"endTime":"2017-01-31",
		"meta":
		{
			"macid":"00-0C-29-55-56-B9"
		}
		 "meta":{}
	}
```

**请求应答**
```
Body：
{
   "fileList":
   {
      "infos":
      [
         {
            "bucketId":209,
            "createTime":"2017-01-23 15:12:39",
            "fileId":195555933708,
            "fileName":"a.jpg",
            "fileSize":879394,
            "meta":
            {
               "macid":"00-0C-29-55-56-B9",
               "userid":"12345",
               "province":"山东省",
               "district":"崂山区",
               "city":"青岛市"
            },
            "privilegeType":0
         },
         {
            "bucketId":209,
            "createTime":"2017-01-23 15:12:10",
            "fileId":195553017445,
            "fileName":"b.jpg",
            "fileSize":845941,
            "meta":
            {
               "macid":"00-0C-29-55-56-B9",
               "userid":"12345",
               "province":"山东省",
               "district":"崂山区",
               "city":"青岛市"
            },
            "privilegeType":0
         },
         {
            "bucketId":219,
            "createTime":"2017-01-23 14:19:50",
            "fileId":195239095710,
            "fileName":"a.jpg",
            "fileSize":879394,
            "meta":
            {
               "macid":"00-0C-29-55-56-B9",
               "userid":"12345",
               "province":"山东省",
               "district":"崂山区",
               "city":"青岛市"
            },
            "privilegeType":0
         }
      ],
      "nextMarker":-1,
      "total":3
   },
   "retCode":"00000",
   "retInfo":"成功"
}
```

#### 查看文件
> 1.查看文件概要信息，包含ID，名字，权限，大小，上传时间，meta值等；</br>
> 2.当前用户有文件的所有权

##### 1、接口定义
?> **接入地址：** `/css/v1/queryFile/{fileID}`</br>
**HTTP Method：** GET

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
fileID|long|url|必填|要查询的文件ID
accessKey|String|Header|必填|用户主ak


**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
fileInfo|FileInfo|Body|必填|

##### 2、请求样例

**请求样例**
```
接入地址：http://*******/css/v1/queryFile/2900851854051
Header：
	accessKey: 1681314738825497:2K0MymVucAOQ==
```

**请求应答**
```
Body：
{
    "fileInfo":
    {
        "bucketId":105,
        "createTime":"2017-01-10 20:37:28",
        "fileId":2900851854051,
        "fileName":"RÙ\u008BP.jpg",
        "fileSize":63550,
        "meta":
        {
            "w23":"qq",
            "'f":"cc"
        },
        "privilegeType":0
    },
    "retCode":"00000",
    "retInfo":"成功"
}
```


#### 修改文件权限
> 1.修改文件的权限，合法值为：0，私有，1，公共读，2，缺省（继承其存储区的权限）；</br>
> 2.当前用户有文件的所有权

##### 1、接口定义
?> **接入地址：** `/css/v1/modifyFileACL/{fileID}`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
fileID|long|url|必填|要修改的文件id
privilegeType|int|Body|必填|权限：0，私有；1，公共读,2,缺省
accessKey|String|Header|必填|用户主ak

**输出参数**： 标准输出参数


##### 2、请求样例

**请求样例**
```
接入地址：http://*******/css/v1/modifyFileACL/101844908215
Header：
	accessKey: 1681314738825497:2K0MymVucAOQ==
Body：
	{
		"privilegeType":1
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

#### 删除文件
> 用户使用主ak，拥有文件的私有权限，删除文件

##### 1、接口定义
?> **接入地址：** `/css/v1/deleteFile/{fileID}`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
fileID|long|url|必填|要删除的文件ID
accessKey|String|Header|必填|用户主ak

**输出参数**： 标准输出参数


##### 2、请求样例

**请求样例**
```
接入地址：http://*******/css/v1/deleteFile/101844908215
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
### 针对应用的服务

#### 文件匿名下载

> 1.文件匿名下载，检查文件权限，如果文件为公共读，可以被用户下载；</br>
> 2.文件ID必须存在

##### 1、接口定义
?> **接入地址：** `/css/v1/anonymous/{fileID}`</br>
**HTTP Method：** GET

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
fileID|long|url|必填|文件id

**输出参数：** 标准输出参数


##### 2、请求样例

**请求样例**
```
接入地址：http://*******/css/v1/anonymous/102400553047
```

**请求应答：** 直接返回文件内容，如为图片则返回图片文件

#### 文件下载
> 把一个云存储中用所有权的文件下载下来；</br>
> 文件ID必须存在，且当前用户拥有其所有权限或者该文件权限为公共读

##### 1、接口定义
?> **接入地址：** `/css/v1/download/{fileID}`</br>
**HTTP Method：** GET

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
fileID|long|url|必填|文件id
accessKey|String|Header|必填|用户主ak

**输出参数：** 标准输出参数

##### 2、请求样例

**请求样例**
```
接入地址：http://*******/css/v1/download/102604239767
Header：
	accessKey: 1681314738825497:2K0MymVucAOQ==
```

**请求应答：** 直接返回文件内容，如为图片则返回图片文件





## 常见问题



[^-^]:常用图片注释
[CapacityService_DeviceCloudStorage_type]:_media/_CapacityService_DeviceCloudStorage/CapacityService_DeviceCloudStorage_type.png

[CapacityService_DeviceCloudStorage_liucheng]:_media/_CapacityService_DeviceCloudStorage/CapacityService_DeviceCloudStorage_liucheng.png

