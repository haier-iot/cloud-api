
# 场景商店类(Portal)

!> **更新时间**：{docsify-updated}  

## 根据appSceneId查询场景基本信息

>根据appId和appSceneId查询场景。 

 1、接口定义
?> **接入地址：** `/iftttscene/scene/store/findBasicSceneInfo`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|最大长度|位置|必填|说明
:-|:-:|:-:|:-:|:-:|:-
appSceneId|String|32|Body|必填|应用场景Id

**输出参数:** 

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|必填|&nbsp;|
retInfo|String|Body|必填|&nbsp;|
data|SceneDto|Body|必填|应用场景详情, 其中的规则rules中带有规则Id和规则名称以及规则描述

## 查询应用场景的规则信息

>查询应用场景的规则信息。

 1、接口定义
?> **接入地址：** `/iftttscene/scene/store/app/rule/getById`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|最大长度|位置|必填|说明
:-|:-:|:-:|:-:|:-:|:-
ruleId|String|32|Body|必填|规则Id

**输出参数:** 

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|必填|&nbsp;|
retInfo|String|Body|必填|&nbsp;|
data|RuleTemplateDto|Body|必填|&nbsp;

## 判断设备列表是否支持该场景列表

>判断型号列表是否支持该场景。

 1、接口定义
?> **接入地址：** `/iftttscene/sceneportal/store/sceneListUsable`</br>
**HTTP Method：** POST

**输入参数**

参数名|限定类型|最大长度|位置|必填|说明
:-|:-:|:-:|:-:|:-:|:-
sceneIdList|数组|32|Body|必填|["0000391158444616ac9124af2ecc0303"," 0000391158444616ac9124af2ecc0303","0000391158444616ac9124af2ecc0303"]
productCodeList|数组|32|Body|必填|["AA9EK3001","AA9EK3004","AA9EK3002"]



**输出参数:** 

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|必填|&nbsp;|
retInfo|String|Body|必填|&nbsp;|
Data|Object|Body|必填|{"sceneId1":true/false,"sceneId2":true/false}

## 获取所有场景标签列表

>查询场景所有标签列表

 1、接口定义
?> **接入地址：** `/iftttscene/sceneportal/store/getAllSceneTagList`</br>
**HTTP Method：** POST

**输入参数** ： 无

**输出参数:** 

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
Data|SceneTagDto[]|Body|[{"id":1,"name":""}]



