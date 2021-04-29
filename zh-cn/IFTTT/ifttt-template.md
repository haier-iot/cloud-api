
# 场景模板

!> **更新时间**：{docsify-updated}  


## 批量下载基础场景

**使用说明**

>APP从场景Store中下载基础场景。</br>
注：同一个家庭可以多次下载同一个场景

**接口描述**

?> **接入地 址：**  `/iftttscene/scene/store/downloadStoreScene`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|
| storeSceneIds| String[] |32| Body| 必填|需要下载的场景|  
| familyId     | String |32| Body| 必填|家庭Id|  


**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  |  String | Body  |  必填 |  &emsp;  |
|  retInfo  |  String | Body  |  必填 |  &emsp; |
|  data  |  String[] | Body  |  必填 | 下载后的新场景Id   |


**示例** 

**请求样例**
```java  
Header：
appId: MB-****-0000
appVersion: 3.3.0
clientId: 123456
sequenceId: 20161020153428000015
accessToken: TGT1IIOXZJHLWOXF2FED4YZTGIQ3B0
sign: 51f839ee62312c41931a42d7353b4e74f50d9f03bedfcd1a227f1be2efc7a91e
timestamp: 1542183603622 
language: zh-cn
timezone: +8
appKey: f50c76fbc8271d361e1f6b5973f54585
Content-Encoding: utf-8
Content-type: application/json
Body
{
	"storeSceneIds": ["2258bce4c54d422b940167a8f30f04f3",
	"6e5faad84ef143e9a497c310e903baa4"],
	"familyId": "zf_platform"
}

```  

**请求应答**

```java
{
	"retCode": "00000",
	"retInfo": "成功",
	"data": ["7fc6f082f77343e2baac4a71b26044f7",
	"72b1f0084ead44229331e477d0de282a"]
}
```

 3、错误码  
> 见 快速开始——常用信息——平台公共错误码 






## 查询模板详情

**使用说明**

>根据家庭id、模板id查询场景的模板详情信息
模版条件的desc描述拼接规则：
（1）ifLable=1时，条件desc =场景模版条件中key的desc值+条件逻辑运算符operationSign中文描述+条件value的desc值。其中条件value的desc由前端H5拼接，其余部分场景引擎拼接。        如果没有设备，条件desc =场景模版条件中key的desc值+条件逻辑运算符operationSign中文描述+条件value的desc值，由场景引擎拼接模版条件描述。
（2）ifLable=0时，条件desc =场景模版条件中key的desc值+条件逻辑运算符operationSign中文描述+条件value的desc值，由场景引擎拼接模版条件描述。

**接口描述**

?> **接入地 址：**  `/iftttscene/scene/template/find/template-info`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| familyId| String |N/A| Body| 必填|家庭Id | 
| templateId| String |N/A| Body| 必填|模板id   | 

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  |String| Body  |必填| &emsp;|
|  retInfo  |String| Body  |必填| &emsp;|
|  data  |SceneTemplateDto| Body  |必填|&emsp;|




## 根据场景模板Id启用场景，优化接口

**使用说明**

>根据场景模板Id下载场景,并设置参数，如果该场景是开关类场景则把场景打开。

 **接口描述**
?> **接入地 址：**  `/iftttscene/scene/sceneEnabled`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|    
| familyId| String |32| Body| 必填|家庭Id| 
| templateId| String |32| Body| 必填|场景模板Id| 
| ruleSettings| RuleSettings[] |N/A| Body| 必填|条件或者动作配置参数| 
| sceneName| String |255| Body| 选填|场景名称| 
| sceneTagList| List<SceneTagDto> |N/A| Body| 选填|场景位置标签| 
| taskInfoList| List<TaskInfoDto> |N/A| Body| 选填|场景生效时间| 
     

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  |String| Body  |必填| &emsp;|
|  retInfo  |String| Body  |必填| &emsp;|
|  sceneId  |String| Body  |必填| 返回保存下载后场景更新规则参数成功的场景Id|


## 场景模版一键启用

**使用说明**

>根据家庭id、场景模板Id（目前只支持可启用的场景模版id）自动下载场景。 

 **接口描述**

?> **接入地 址：**  `/iftttscene/scene/sceneAutoDownload`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| familyId| String |32| Body| 必填|家庭Id | 
| templateIds| String[] |512| Body| 必填|模板Id数组   | 

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  |String| Body  |必填| &emsp;|
|  retInfo  |String| Body  |必填| &emsp;|





