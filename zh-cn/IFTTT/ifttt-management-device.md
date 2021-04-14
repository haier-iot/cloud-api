

# 组件类（Portal）

!> **更新时间**：{docsify-updated}  

## 查询组件信息

> 查询组件信息(增加业务组件为typeId组件时返回typeid,组件为型号组件时,返回设备型号)。

 1、接口定义
?> **接入地址：** `/iftttscene/component/getById`</br>
**HTTP Method：** POST

**输入参数** 

参数名|类型|最大长度|位置|必填|说明
:-|:-:|:-:|:-:|:-:|:-
id|String|32|Body|必填|组件Id

**输出参数:** 

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|必填|&nbsp;|
retInfo|String|Body|必填|&nbsp;|
data|ComponentDto|Body|必填|&nbsp;|


## 根据组件id功能id功能值和设备获取支持的型号属性信息

> 根据组件Id、功能id、功能值 和设备(型号产品编码列表) 获取具体设备的属性信息。

 1、接口定义
?> **接入地址：** `/iftttscene/component/getModelPropList`</br>
**HTTP Method：** POST

**输入参数** 

参数名|类型|最大长度|位置|必填|说明
:-|:-:|:-:|:-:|:-:|:-
componentId|String|32|Body|必填|可为产品id 中类组件id 型号组件id
functionId|String|32|Body|必填|可为产品功能id、中类属性id，型号属性id
functionVal|String|32|Body|可为空|兼容老版本，参数可不填，模板中对产品组件类型模板传值，其他组件不传
productCodeList|String|64|Body|必填|型号产品编码数组

**输出参数:** 

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|必填|&nbsp;|
retInfo|String|Body|必填|&nbsp;|
data|PropDto|Body|必填|[{<br>"productCode" //型号产品编码<br>"model"//型号名称<br>"typeId"       //typeid <br>"propClass"    //属性类型<br>"propName"    //属性标识名称;<br>"description"    //属性标识描述;<br>"functionName"  //属性功能标识名称; <br>"functionDesc"  //属性功能标识描述; <br>"propFixer"     //属性修饰词;<br>"propValType"  //取值类型;<br>"variants"      //值范围;<br>"readable     //是否可读;<br>"writable"     //是否可写;<br>"whenLabel"   //是否作为条件;<br>"thenLabel"   //是否作为动作;<br>"propSort"   //排序;<br>}]|



##	根据组命令功能名和设备获取具体组功能属性信息

> 根据组功能名称 和设备(型号产品编码列表) 获取具体设备的属性信息。

 1、接口定义
?> **接入地址：** `/iftttscene/component/getModelGroupPropList`</br>
**HTTP Method：** POST

**输入参数** 

参数名|类型|最大长度|位置|必填|说明
:-|:-:|:-:|:-:|:-:|:-
functionName|String|32|Body|必填|可为产品功能id、中类属性id，型号属性id
productCodeList|String[]|64|Body|必填|型号产品编码数组

**输出参数:** 

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|必填|&nbsp;|
retInfo|String|Body|必填|&nbsp;|
data|PropDto|Body|必填|[{<br>"productCode" //型号产品编码<br>"model"//型号名称<br>"typeId"       //typeid <br>"propClass"    //属性类型<br>"propName"    //属性标识名称;<br>"description"    //属性标识描述;<br>"functionName"  //属性功能标识名称; <br>"functionDesc"  //属性功能标识描述; <br>"propFixer"     //属性修饰词;<br>"propValType"  //取值类型;<br>"variants"      //值范围;<br>"readable     //是否可读;<br>"writable"     //是否可写;<br>"whenLabel"   //是否作为条件;<br>"thenLabel"   //是否作为动作;<br>"propSort"   //排序;<br>}]|


##	根据设备产品编码查询型号属性列表

> 根据设备型号产品编码，查询型号对应的功能，如果该型号没有相关数据，则返回空设备的功能会按照海极网设备功能配置的顺序输出。

 1、接口定义
?> **接入地址：** `/iftttscene/component/getModelPropByProductCode`</br>
**HTTP Method：** POST

**输入参数** 

参数名|类型|最大长度|位置|必填|说明
:-|:-:|:-:|:-:|:-:|:-
productCode|String|9位|Body|必填|设备型号产品编码


**输出参数:** 

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|必填|&nbsp;|
retInfo|String|Body|必填|&nbsp;|
data|FunctionsDto|Body|必填|{<br>    "sysProps":{<br>	"id":"",//产品编码<br>"componentId":""，<br>"componentType":""//组件类型 MODEL<br>},//app取出sysProps字段直接<br>	"actions":[{<br>		"propId":"",//属性主键<br>		"desc":"",//组命令前端呈现<br>"propClass":"",//属性类别<br>           “fixer”:””,//定语，属性的修饰词：比如“设置为”，“执行”；<br>"propSot":0,//属性排序数字<br>		"props":[]//类型为ComponentFunctionPropDto<br>	}],<br>	"conditions":[]//数据结构跟actions一致<br>}<br><br><br>ComponentFunctionPropDto结构如下：<br>{ <br>          "propId":"",//属性主键<br>"propClass":"",//属性类别<br>		"desc":"",<br>          “fixer”:””,//定语，属性的修饰词：比如“设置为”，“执行”；<br>		"propName":"",//原始命令值，app需要给引擎赋该值<br>        "functionName":"",//功能标识名称，在基于场景模板创建应用场景时，使用该字段匹配模板中的functionName来确定是否支持目标场景模板（大部分情况下跟propName相同）<br>		"propValType":"",//取值类型<br>		"variants":"",//取值范围，为json字符串<br>		"defaultValue":"",//预留字段，不维护任何值 ，<br>		"defaultValueDesc":"",//预留字段，不维护任何<br>}|



##	查询型号支持场景状态列表

> 通过型号产品编码列表和查询类型查询型号支持场景状态。状态包含否可以作为条件、是否可以作为动作。型号如果不支持场景，则结果集不返回。

 1、接口定义
?> **接入地址：** `/iftttscene/component/ getModelSupportSceneStateList`</br>
**HTTP Method：** POST

**输入参数** 

参数名|类型|最大长度|位置|必填|说明
:-|:-:|:-:|:-:|:-:|:-
productCodeList|List|暂不限制个数，理论上用户家庭不会超过20个设备。|Body|必填|型号产品编码列表。
type|int|&nbsp;|Body|必填|查询类型  0：查询未上线的型号  1：查询已上线型号 ，2 查询 全部     必传值。
App灰度测试之用到1和2


**输出参数:** 

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|必填|&nbsp;|
retInfo|String|Body|必填|&nbsp;|
data|SceneFunctionSupportDto[]|Body|必填|{<br>	[{<br>		"productCode":"",//型号编码<br>	     "supportSceneStatus":"",//true : 支持场景,false : 不支持场景<br>		"sceneType":""//1 :  只支持条件  2 :只支持动作  3 : 既支持条件也支持动作  <br>	}]<br>}
|


##	获取非设备类组件和属性功能列表

> 获取非设备类组件属性功能列表（天气、定时、延时、地理围栏）。

 1、接口定义
?> **接入地址：** `/iftttscene/component/getCmptPropList`</br>
**HTTP Method：** POST

**输入参数** 

参数名|类型|最大长度|位置|必填|说明
:-|:-:|:-:|:-:|:-:|:-
type|String|32|Body|必填|取值：天气 WEATHER, 定时 TIMER, 延时 DELAY, 地理围栏 GEOFENCE 



**输出参数:** 

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|必填|&nbsp;|
retInfo|String|Body|必填|&nbsp;|
data|List< ComponentFunctionsDto >|Body|必填|{<br>"componentId":""，<br>"componentType":" WEATHER"//组件类型  WEATHRE：天气组件<br>“componentName”: "当前天气"<br>"componentDesc":"当前天气"<br>"props":[]//类型为FunctionDto<br>	}],<br>}]<br><br><br>FunctionDto 结构<br>{ <br>          "id":" 0021ba83f2314d0b9b9702791193b22d ",//属性主键<br>"type":""//属性类型,		<br>"description":"温度",<br>          “fixer”:设置为”,//定语，属性的修饰词：比如“设置为”，“执行”；<br>		"name":"temperature ",//属性标识<br>        "whenLable":true,//可作为条件 <br>"thenLable":false,//可作为动作 <br>		"valType":"double",//取值类型<br>		"variants":" {"unit":"℃","minValue":-50,"step":1,"maxValue":50} <br>",//取值范围，为json字符串<br>}|

