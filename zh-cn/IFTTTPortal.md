!> **当前版本：** [场景引擎portal]()  
**更新时间**：{docsify-updated} 

## 简介 
场景引擎portal。
![场景引擎protal][]

### 名词解释

-  **名词**


## 应用场景
APP调用设置场景



## 功能介绍

## 公共结构说明

### SceneDto 场景对象
权限内容，其中至少一项为ture

参数名|类型|说明|备注
:-|:-:|:-:|:-
Id|String|场景唯一Id|不填；UUID
sourceId|String|场景来源ID|选填；用户自建场景忽略
basisSceneId|String|基础场景Id|基础创景Id+基础版本号来区分是否需要升级
sceneName|String|场景名称（应用场景名称）|必填；最大长度255
appSceneAlias|String|应用场景别名|选填；最大长度255，用户自建场景忽略
basicSceneAlias|String|公共场景别名|选填；最大长度255，用户自建场景忽略
sceneDesc|string|场景描述|必填；最大长度600
userId|String|用户ID|必填；最大长度32
familyId|String|家庭ID|选填
type|String|场景类型|必填；最大长度32，用户自建场景忽略
rule|RuleTemplateDto[]|规则模板|必填，可以是多个规则片段组成场景整体规则；</br>一个场景最多支持10个片段；</br>每个片段最多支持25个条件的运算;
auto|boolean|是否支持自动实例化|必填；默认为false用户自建场景忽略
canAppTrigger|boolean|是否支持APP手动触发执行|选填，默认是不支持</br>0:开启后自动执行；1：一键离家
status|String|基础场景当前状态|目前只关注三个状态，draft，默认不填为草稿；publish，已发布；deleted，已删除</br>用户不关注（场景测试app可以根据该字段显示场景模型开发状态）
appId|String|appId|应用标识
isOpen|Integer|场景是否开启|1，开启；0，关闭
weight|Integer|应用场景权重|必填，用户自建场景忽略
appSceneStatus|Integer|应用场景状态|必填，0，下架；1，上架；用户自建场景忽略
version|String|应用场景版本|最大长度32，用户自建场景忽略，但是需要app显示
sorList|List<SceneSortDto>|应用场景分类列表|选填，最大长度256，多个分割，用户自建场景忽略
tagList|List<SceneTagDto>|应用场景标签编号|选填，最淡长度256，多个分割，用户自建场景忽略
prompt|String|用户填写的“提示”信息|最大长度256
createTime|Timestamp|创建时间|
updateTime|Timestamp|最后更新时间|
activeBeginTimes|Timestamp|场景生效开始时间|最大长度64
activeEndTime|Timestamp|场景生效结束时间|最大长大户64</br>说明：生效开始时间可以大于结束时间，该情况视为跨天执行，但是最多不能跨24小时</br>并且生效时间段支队开关类场景起作用手动触发场景不受限制
recommend|String|推荐|最大长度64，用户自建场景忽略
collect|String|收藏|最大长度64，用户自建场景忽略
aiKLeyWord|String|语音标签|最大长度256
icon|String|场景图标URL|最大长度256
inoutSide|Integer|场景标识|0，外部-非官网；1，内部-官网；</br>用户自建场景忽略
basisSceneVersion|String|系统场景版本|最大长度32，用户自建场景忽略
basisSceneVersion|String|系统场景名称|最大长度64，用户自建场景忽略
basisSceneDescription|String|系统场景描述|最大长度1800，用户自建场景忽略
basisSceneTags|String|系统场景表亲啊|最大长度256，平台选择项（空调、新风机等）用户自建场景忽略
createUserNickname|String|场景开发者展示名称|最大长度64，城阳景开发者在海极网填写的昵称信息，用户自建场景忽略
createUserLogo|String|场景开发者展示LOGO|最大长度256，场景开发者在海极网配置的logo信息PNG或JPG格式，128*128或64*64规则用户自建场景忽略
taskInfo|TaskInfo|定时策略信息|
eVersion|String|引擎执行版本号|最大32位，不需要app前端关注
aVersion|String|应用场景版本号|
uVersion|String|用户场景版本号|最大32位
dType|String|下载类型，Basis，基础场景；App，应用场景；User，用户自建|最大16位



### BasisSCeneDto 基础场景对象

参数名|类型|说明|备注
:-|:-:|:-:|:-
Id|String|场景Id唯一|基础场景Id|基础版本号来区分是否需要升级
sceneName|String|场景名称（应用场景名称）|必填；最大长度255
sceneAlias|String|场景别名|选填；最大长度255，用户自建场景忽略
sceneDesc|String|场景描述|必填；最大长度32，用户自建场景忽略
type|String|场景类型|必填；最大长度32，用户自建场景忽略
rules|RuleTemplateDto[]|规则模板|必填；可以是多个规则片段组成场景整体规则，一个场景最多支持10个片段；每个片段最多支持25个条件
auto|boolean|是否支持自动实例化|
triggerType|Boolean|是否支持App手动触发执行|选填，默认是不支持；</br>false,平台触发类场景</br>true，手动触发场景；</br>空类型未平台类场景对应之前的canApopTrigger
status|String|基础场景当前状态|选填；目前只关注三个状态，默认不填为draft，草稿；</br>publish,已发布；deleted，已删除；</br>用户不关注（场景测试app可以根据该字段显示场景模板开发状态）
version|String|场景版本|最大长度32（场景模板Id+版本号唯一标识规则信息）
createTime|Timestamp|创建时间|
updateTime|Timestamp|最后更新时间|
taskInfo|TaskInfoDto|时间策略信息|有两个策略，周期生效（支支持平台触发），定期执行（只支持手动触发），同一个场景智能支持一个策略
aiKeyWord|String|语音标签|最大长度256
banner|String|场景banner图URL|最大长度256
icon|String|场景图标URL|最大长度256
createUserId|String|场景创建者Id|最大64
createUserNickname|String|场景开发者展示名称|最大长度64，场景开发者在海极网填写的昵称信息，用户自建场景忽略
createUserLogo|String|场景开发者展示LOGO|最大长度256，场景开发者在海极网配置的logo信息PNG或者JPG格式，128*128或64*64规则，用户自建场景忽略
engineVersion|String|引擎执行版本号|最大32位，当前版本号位1.0（来源由场景引擎提供，Portal入库需要提供该版本号）

### AppSceneDto（应用场景对象）

参数名|类型|说明|备注
:-|:-:|:-:|:-
Id|String|场景唯一Id|基础场景Id+基础版本号来区分是否需要升级
sceneName|String|场景名称（应用场景名称）|必填；最大长度255
sceneAlias|String|场景别名|选填；最大长度255用户自建场景忽略
sceneDesc|String|场景描述|必填；最大长度600
type|String|场景类型|鼻涕那；最大长度32
rules|RuleTemplateDto[]|规则模板|必填；可以是多个规则片段组成场景整体规划;</br>一个场景最多支持10个片段；</br>每个片段最毒支持25个条件的运算
auto|boolean|是否支持自动实例化|（后续讨论更新）
triggerType|Boolean|是否支持App手动触发执行|选填，默认是不支持；</br>false,平台触发类场景;true,手动触发场景；</br>空类型为平台类场景,对应之前的canAppTrigger
status|String|场景当前状态|选填，目前只关注三个状态，draft，默认不填为草稿;</br>publish，已发布;deleted，已删除；</br>用户不关注（场景测试app可以根据该字段显示场景模板开发状态）
version|String|基础场景版本|最大长度32（基础场景模板Id+版本号唯一标识规则信息）
weight|Integer|用户场景权重|选填，按该字段进行排序
createTime|Timestamp|创建时间|选填
updateTime|Timestamp|最后更新时间|选填
taskInfo|TaskInfoDto[]|时间策略信息|选填,有两个策略;</br>周期生效（只支持平台触发类），定期执行（只支持手动触发类）</br>同一个场景只能支持一个时间策略
aiKeyWord|String|语音标签|最大长度256
banner|String|场景banner图URL|最大长度256
icon|String|场景图标URL|最大长度256
createUserId|String|创建者Id|最大64
createUserNickname|String|场景开发展示名称|最大长度64，场景开发者在海极网填写的昵称信息，用户自建场景忽略
createUserLogo|String|场景开发者展示LOGO|最大长度256，场景开发者在海极网配置的logo信息PNG或者JPG格式，128*128或64*64规则，用户自建场景忽略
engineVersion|String|引擎执行版本号|最大32位，当前版本号位1.0（来源由场景引擎提供，Portal入库需要提供该版本号）


### UserSceneDto（用户场景对象）
参数名|类型|说明|备注
:-|:-:|:-:|:-
id|String|长几个Id唯一|不填，UUID
sourceId|String|基础场景ID|选填，自建场景不关注
sourceVersion|String|基础场景版本号|选填，自建场景不关注
sceneName|String|场景名称（应用场景名称）|必填，最打长度255
cenenAlias|String|场景别名|选填，最大长度255
sceneDesc|String|场景描述|必填，最大长度600
userId|String|用户ID|选填
familyId|String|家庭ID|选填
type|String|场景类型|必填，最大长度32，用户自建场景忽略
rules|RuleTemplateDto[]|规则模板|必填，可以是多个规则片段组成场景整体规则；</br>一个场景最多支持10个片段；每个片段最多支持25个条件的运算；
tiggerType|String|是否支持App手动触发执行|选填，目前取值：platform：平台触发，manually：手动触发：timerTrigger：时间触发；</br>注释：该字段由app定义，app可根据该字段配合手动触发机制实现地图围栏，天气等业务
appId|String|appId|应用标识
isOpen|Integer|场景开启|1，开启；0，关闭
weight|Integer|用户场景权重|必填，按该字段进行排序
sortList|List<SceneSortDto>|用户场景分类列表|必填，最大长度256，多个可以分割，分类只能有一个
tagList|List<SceneTagDto>|用户场景标签编号|必填；最大长度256，多个可以分割;</br>1、如果场景类型为用户自建，则该标签可以被修改；</br>2、如果场景类型为模板下载，该标签不能被用户修改
createTime|TimeStamp|创建时间|
updateTime|TimeStamp|最后跟新时间|
taskInfo|TaskInfoDto|时间策略信息|有连个策略，周期生效（只支持平台触发类），定期执行（支支持手动触发类）
aiKeyWord|String|语音标签|最大长度256
banner|String|场景banner图URL|最大长度256，如果场景为下载类型，则来源于场景开发者相关信息，如果为用户自建，则需用户配置该字段，非必填
icon|String|场景图标URL|最大长度256
createUserNickname|String|场景开发者展示名称|最大长度64，场景开发者在海极网填写的昵称信息，用户自建场景忽略
createUserLogo|String|场景开发者LOGO|最大长度256，场景开发者在海极网配置的logo信息PNG或JPG格式128*128或64*64规则，用户自建场景忽略
engineVersion|String|引擎执行版本号|最大32位，该值为引擎赋值
createType|String|场景创建方式：</br>Basis，下载方式；User，自建方式|最大16位


### RuleTemplateDto（规则对象）

参数名|类型|说明|备注
:-|:-:|:-:|:-
id|String|规则Id|
rule|String|规则名称|选填，如果不填则默认值为场景名称+当前规则下标
desc|String|规则描述|必填，用户自建场景忽略
salience|Ingeger|规则执行优先级，数字越大则越先执行|选填，不填则默认按照规则下标顺序执行，用户自建场景忽略
when|RuleWhenDto|规则条件|选填，如果不填则说明是while（true）直接执行
then|RuleThenDto|规则触发|选填
isOpen|Integer|规则片段是否开启|1，开启；0，关闭


### RuleWhenDto（规则条件对象）

参数名|类型|说明|备注
:-|:-:|:-:|:-
conditions|RuleWhenCondition[]|条件对象数组|选填，标识when里边的条件组合
desc|String|标识when字段描述|选填，用户自建场景忽略


### RuleWhenCondition（条件对象）

参数名|类型|说明|备注
:-|:-:|:-:|:-
id|String|条件ID|UUID
object|StatusDesc|条件载体，如果条件为设备条件则该值为设备信息，value为设备mac;desc为设备昵称|
key|StatusDesc|标注模型name，OS需要录入StatusDEsc的ID；标识某个组建的属性ID；</br>OS查询条件返回组建苏醒描述（值和描述，还有组建属性的ID）|选填
operationSign|OperationSign|关系运算符（枚举类型）|选填，取值如下：</br>greaterThan(">"), greaterThanEqual(">="), equal("=="),lessThan("<"), lessThanEqual("<="),unequal(“!=)
value|StatusDesc|标准模型值|选填
logicalSign|LogicalSign|逻辑运算符（枚举类型）|该条件对应上一个平行条件的逻辑运算，取值如下</br>AND（“&&”）；OR("/|/|")
condition|RuleWhenCondition[]|子条件对象数组|选填，说明：</br>举例,if(a||b) 这种格式那么conditions为null；除了logicalSign其它必填；</br>if((a||b)&&(c||d))这种格式则conditions必填；其它都为null；
component|String|所属组件Id|必填，由组件区分是设备组件还是其他组件

### RuleThenDto（规则触发动作）

参数名|类型|说明|备注
:-|:-:|:-:|:-
action|RuleThenAction[]|需要触发的动作|选填
desc|String|标识then字段描述|选填，用户自建场景忽略

### RuleThenAction（规则触发动作详细描述）

参数名|类型|说明|备注
:-|:-:|:-:|:-
Id|String|行为ID|主键ID，UUID
desc|String|行为动作描述|必填
type|ActionEnum|需要触发的动作类型，枚举类|取值如下：</br>DeviceControl，设备控制；</br>MessagePush，消息推送</br>MessagePushWithControl：带小循环控制的消息
dealyTime|Integer|动作延迟执行时间|选填，取值为数字，单位秒</br>说明该动作需要在触发执行后多少秒后触发</br>如果不填或者0则认为不延迟，用户自建场景忽略
control|RuleThenDeviceControl|设备控制动作|选填，如果type值为：DeviceControl control必填，pushMessage为空；</br>如果type为MessagePushWithControl 则control和pushMessage都必填
pushMessage|RuleThenPushMessage|消息推送动作|选填，如果选择消息推送则control为空，pushMessage必填
isOpen|Integer|动作片段是否开启|1，开启；0，关闭

### RuleThenPushMessage（消息推送动作）

参数名|类型|说明|备注
:-|:-:|:-:|:-
PushType|PushType|推送类型，枚举|必填，取值如下：</br>单设备推送：device,</br>帐号全设备推送：user,</br>帐号部分设备推送：user_device,</br>按照家庭推送：family,</br>家庭下部分设备推送：family_device,</br>按照用户手机推送：user_mobile
pushConternt|PushContent|发送消息内容|必填
showTypes|Map<String，String>|终端显示类型|取值：</br>Key取值为：01,冰箱;09,烟机;0F,电视;phone,手机;tablet,平板;smc,smartCenter</br>Value取值：0:,toast;2, 弹框;3, push+弹框;4,红色感叹号显示;</br>Value具体定义遵循上一版本弹框格式
msgStragtegy|MessageStrategy|消息发送策略|选填
priority|Integer|消息优先级|具体和《ums3.0接口定义说明书-标准版》保持一致，示意如下:</br>0 –紧急消息;1 –一般消息，如果不填则默认为一般消息;</br>2 –中低消息;3 –低级消息

### MessageStrategy（消息推送策略）

参数名|类型|说明|备注
:-|:-:|:-:|:-
timeInterval|Interval|推送间隔时间|选填，单位：分钟
notWorkTimeStart|String|免打扰开始时间|HH:mm 选填
notWorkTimeEnd||String|免打扰结束时间|HH:mm 选填
macCount|Long|最大发送条数|选填

### PushContent（推送内容）

参数名|类型|说明|备注
:-|:-:|:-:|:-
msgName|String|消息名称|必填
expires|int|消息过期时间|单位为分钟，默认60min
msgTitle|String|消息标题|必填
msgcontent|String|消息内容|必填

### RuleThenDeviceControl（设备控制动作）

参数名|类型|说明|备注
:-|:-:|:-:|:-
object|StatusDesc|动作载体为设备信息，value为设备mac；desc为设备昵称|
args|DeviceControlArgs[]|控制命令集合|选填，如果为组命令则args有可能为空
controlBtnText|String|带控制消息的相关按钮信息|选填，消息推送带小循环控制时必填，长度最大为10
componentld|String|组件id|必填
operation|StatusDesc|操作名称|选填，单命令控制不需要设置该值；</br>组命令控制需要设置操作名称


### DevicecontrolArgs（设备控制命令参数）

参数名|类型|说明|备注
:-|:-:|:-:|:-
name|statusDesc|控制标准模型名称|必填
value|StatusDesc|控制标准模型值|选填，原始值放入value属性，递增递减值放入changeValue
changeValue|StatusDesc|标准模型的改变值（增加或减少相应单位量）|选填，原始值放入value属性，递增递减值放入changeValue


### ComponentDeviceDesc（设备描述）

参数名|类型|说明|备注
:-|:-:|:-:|:-
Id|String|设备主键|必填
bigClass|StatusDesc|设备大类|必填
middleClass|StatusDesc|设备中类|必填

### ComponentDto（组件信息）

参数名|类型|说明|备注
:-|:-:|:-:|:-
id|String|组件主键|必填
componentName|String|组件名称|必填
componentType|ComponentType|组件类型|枚举类型，设备组件：</br>中类组件：DEVICE("device")</br>typeId组件：TYPEID（“typeId”）</br>型号组件：MODEL(“model”)
deviceDesc|ComponentDeviceDesc|组件所属设备标准模型类型|选填，只用党组件类型为设备组件：DEVICE（“device”），该属性才有值
wifitypeList|String[]|组件支持的wifitype列表|如果该字段为空，则支持中类下所有typeid设备，如果不为空，则组件只支持返回的typeid列表
propList|List<PropOfComponentDto>|组件相关的属性列表|选填
typeId|String|设备类型|选填，只有当组件类型为typeId组件：TYPEID（“typeId”）该属性才有值
model|String|设备型号|选填，只有当组件类型为型号组件：MODEL(“model”)，该属性才有值

### PropOfComponentDto（组件的属性信息）

参数名|类型|说明|备注
:-|:-:|:-:|:-
id|String|组件主键|选填；最大长度32位
propName|String|标识名称|必填；硬件属性的标识名称，程序读的
propClass|String|属性类别|可取值：property(属性)、alarm（告警）、operation(操作类属性)、group（组命令）
functionName|String|功能标识名称|程序读的
description|String|显示名称|选填，人读的
functionDesc|String|功能显示名称|人读的
propValType|String|属性值类型|选填：</br>可取值：prop_class为property或者为operation时，</br>可取double,int,bool, string,enum；prop_class为alarm,该字段为null
readable|boolean|是否可读|选填
writable|boolean|死否可写|选填
variants|String|取值范围|存储取值范围的json字符串
splitFunc|int|拆分标识|拆分标识 0：不拆，1：拆分后，2：被拆分属性；

### FunctionDto（组件功能列表）

参数名|类型|说明|备注
:-|:-:|:-:|:-
sysProps|SysPropDto|app用系统属性|
conditions|List<ConditionOrActionDto>|条件|
action|List<ConditionOrActionDto>|动作|组命令仅用作动作

### SysPropDto(app用系统属性)

参数名|类型|说明|备注
:-|:-:|:-:|:-
id|String|主键|typeId或型号映射表的主键
componentId|String|组件ID|
componentType|ComponentType|组件类型|typeId组件：TYPEID（“typeId”）</br>型号组件：MODEL(“model”)

### ConditionOrActionDto（条件或者动作）

参数名|类型|说明|备注
:-|:-:|:-:|:-
propId|String|主健|单命令：属性主键；</br>组命令：组命令功能主键
propClass|String|属性类别|可取值： property(属性) alarm（告警） operation(操作类属性)，group（组命令）
desc|String|描述|单命令：属性标识显示名称；</br>组命令：组命令功能显示名称
fixer|String|属性修饰词|
props|List< ComponentFunctionPropDto |功能属性集合|
splitFun|int|拆分标识|拆分标识0：不拆，1：拆分

### ComponentFunctionPropDto（功能属性信息）

参数名|类型|说明|备注
:-|:-:|:-:|:-
propId|String|属性主键|
propName|String|标识名称|
fixer|String|属性修饰词|
functionName|String|功能标识名称|
desc|String|属性标识显示名称|
propValType|String|属性值类型|double,int,bool, string,enum
variants|String|取值范围|存储取值范围的json字符串。
defaultValue|String|默认值|预留字段，不维护任何值
defaultValueDesc|String|默认值描述|预留字段，不维护任何值

### SceneFunctionSupportDto（场景功能支持对象）

参数名|类型|说明|备注
:-|:-:|:-:|:-
model|String|设备型号|
TypeId|String|设备类型|
supportSceneStatus|Boolean|是否支持场景|true，支持场景</br>false,不知场景
sceneType|Int|支持场景的类型|1，只支持条件；</br>2:只支持动作;</br>3:既支持条件也支持动作

### StatusDesc（字段取值和描述）

参数名|类型|说明|备注
:-|:-:|:-:|:-
id|String|属性Id|选填；如果为条件的Key，则Id表示组件中的某个属性的ID
value|String|字段值|选填
desc|String|字段值中文描述|选填
scope|Map<String,String>|取值范围，跟标准同步|选填，数据结构：{“type”:”enum”,“value:”json”}</br>type类型遵循PropOfComponentDto中的PropValType字段描述本结构中value的取值范围，只有在具体的属性中才有值，表示这个属性的取值范围；不在属性的值中出现
required|Boolean|书香是否必填|如果为true表示app实例化需要填写此参数

### Pagination（分页对象）

参数名|类型|说明|备注
:-|:-:|:-:|:-
pageSize|int|每页显示的条数|必填；如果为游标类型，则pageSize表示向下加载多少
cursor|int|游标|如果分页类型为游标（App向下滑动）则必填；如果为普通分页则不需要；
recordCount|int|总记录数|必填
currentPage|int|当前页面|必填
totalPage|int|总页数|必填
list|List<T>|每页返回的对象列表|必填

### SceneSortDto（场景分类对象）

参数名|类型|说明|备注
:-|:-:|:-:|:-
id|Integer|id|必填，最大长度32
name|String|分类名称|必填，最大长度32

### SceneTagDto（场景标签对象）

参数名|类型|说明|备注
:-|:-:|:-:|:-
id|Integer|id|必填，最大长度32
name|String|标签名称|必填，最大长度32

### SceneTagDto（场景标签对象）

参数名|类型|说明|备注
:-|:-:|:-:|:-
type|Int|策略类型：</br>1，定时执行策略（针对手动触发类场景）；</br>2，生效时间策略（针对平台执行类场景）；|
cron|CronDto|定时表达式|
activeBeginTime|String|场景生效开始时间|最大长度64
activeEndTime|String|场景结束时间|最大长度64</br>说明：生效开始时间可以大于结束时间，该情况视为夸天执行，但是最多不能夸24小时;</br>并且生效时间段只对开关类场景起作用，手动触发场景不受限制
status|Boolean|定时状态，true开启；false，关闭

### SceneTagDto（场景标签对象）

参数名|类型|说明|备注
:-|:-:|:-:|:-
minute|String|必填，分钟|取值范围（0-59）；允许特殊字符（, - * //）
hours|String|必填，小时|取值范围（0-23）；允许特殊字符（, - * //）
day|String|必填，天|取值范围（1-31）；允许特殊字符（, - * ? // L W）
month|String|必填，月|取值范围（1-12 or JAN-DEC）；允许特殊字符（, - * //）
week|String|必填，周|取值范围（1-7 or SUN-SAT）；允许特殊字符（, - * ? // L #）
year|String|必填，年|可为空，取值范围（1970-2099）；允许特殊字符（, - * //）


## 接口清单

### 场景商店类

#### 从场景商店查询场景列表
> APP用户浏览场景store

##### 1、接口定义
?> **接入地址：** `/iftttscene/scene/store/list`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|最大长度|位置|必填|说明
:-|:-:|:-:|:-:|:-:|:-
limit|Int|N/A|Body|必填|每页显示的记录数
cursor|Int|N/A|Body|必填|从0开始

**输出参数:** 

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|必填|
retInfo|String|Body|必填|
data|Pagination<SceneDto>|Body|必填|显示场景Store中的描述信息，规则rules中带有规则Id和规则名称

#### 按应用标识查询场景列表（V2.4）


##### 1、接口定义
?> **接入地址：** `/iftttscene/scene/store/listByAppId`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|最大长度|位置|必填|说明
:-|:-:|:-:|:-:|:-:|:-
limit|Int|N/A|Body|必填|每页显示的记录数
cursor|Int|N/A|Body|必填|从0开始

**输出参数:** 

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|必填|
retInfo|String|Body|必填|
data|Pagination<SceneDto>|Body|必填|场景列表信息，其中每个场景详情中的规则rules中只带有规则Id和规则名称以及规则描述,同时场景记录按照创建时间倒序

#### 根据appSceneId查询场景基本信息（V2.4）

##### 1、接口定义
?> **接入地址：** `/iftttscene/scene/store/findBasicSceneInfo`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|最大长度|位置|必填|说明
:-|:-:|:-:|:-:|:-:|:-
appSceneId|String|32|Body|必填|应用场景Id

**输出参数:** 

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|必填|
retInfo|String|Body|必填|
data|String|Body|必填|应用场景详情，其中的规则rules中带有规则Id和规则名称以及规则描述

#### 根据关键字查询相关场景（本期不实现）

##### 1、接口定义
?> **接入地址：** `/iftttscene/scene/store/keyword`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|最大长度|位置|必填|说明
:-|:-:|:-:|:-:|:-:|:-
keyword|String|255|Body|必填|关键字，根据关键字模糊查询相关场景

**输出参数:** 

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|必填|
retInfo|String|Body|必填|
data|String|Body|必填|显示场景Stroe中的描述信息，不太有规则详情

#### 应用场景标签列表查询（v2.4）

##### 1、接口定义
?> **接入地址：** `/iftttscene/scene/store/getSceneTagListOfScene`</br>
**HTTP Method：** POST

**输入参数**

参数名|类型|最大长度|位置|必填|说明
:-|:-:|:-:|:-:|:-:|:-
sceneId|String|32|Body|必填|应用场景id

**输出参数:** 

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|必填|
retInfo|String|Body|必填|
Data|Object|Body|必填|应用场景相关标签列表

#### 根据应用标识查询标签列表（v2.4）

##### 1、接口定义
?> **接入地址：** `/iftttscene/scene/store/getSceneTagListByAppId`</br>
**HTTP Method：** POST

**输入参数** ： 无

**输出参数:** 

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|必填|
retInfo|String|Body|必填|
Data|Object|Body|必填|标签列表


#### 根据应用标识查询分类列表（v2.4）

##### 1、接口定义
?> **接入地址：** `/iftttscene/scene/store/getSceneSortListByAppId`</br>
**HTTP Method：** POST

**输入参数** ： 无

**输出参数:** 

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|必填|
retInfo|String|Body|必填|
Data|Object|Body|必填|分类列表


#### 查询应用场景的规则信息（v2.4）

##### 1、接口定义
?> **接入地址：** `/iftttscene/scene/store/app/rule/getById`</br>
**HTTP Method：** POST

**输入参数** 

参数名|类型|最大长度|位置|必填|说明
:-|:-:|:-:|:-:|:-:|:-
ruleId|String|32|Body|必填|规则id

**输出参数:** 

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|必填|
retInfo|String|Body|必填|
data|RuleTemplateDto|Body|必填|


#### 判断设备列表是否支持该场景

##### 1、接口定义
?> **接入地址：** `/iftttscene/scene/store/sceneUsable`</br>
**HTTP Method：** POST

**输入参数** 

参数名|类型|最大长度|位置|必填|说明
:-|:-:|:-:|:-:|:-:|:-
sceneId|String|32|Body|必填|应用场景id
typeId|String[]||Body|必填|typeIds:["typeId1"," typeId2"," typeId3"," typeId4"]

**输出参数:** 

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|必填|
retInfo|String|Body|必填|
Data|Object|Body|必填|true/false


### 组件类

#### 查询组件信息（V2.5兼容）

> 查询组件信息(增加业务组件为typeId组件时返回typeid,组件为型号组件时,返回设备型号)

##### 1、接口定义
?> **接入地址：** `/iftttscene/component/getById`</br>
**HTTP Method：** POST

**输入参数** 

参数名|类型|最大长度|位置|必填|说明
:-|:-:|:-:|:-:|:-:|:-
id|String|32|Body|必填|组件Id

**输出参数:** 

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|必填|
retInfo|String|Body|必填|
data|ComponentDto|Body|必填|

#### 根据设备型号查询功能列表（V2.新增）

> 根据设备型号查询型号对应的功能，如果该型号没有相关数据，则查询型号对应的TypeId对应的功能列表
> 设备的功能会按照海极网设备功能配置的顺序输出

##### 1、接口定义
?> **接入地址：** `/iftttscene/component/getPropByModel`</br>
**HTTP Method：** POST

**输入参数** 

参数名|类型|最大长度|位置|必填|说明
:-|:-:|:-:|:-:|:-:|:-
model|String|32|Body|如果app有型号则必填|设备型号
typeId|String|32|Body|必填|

**输出参数:** 

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|必填|
retInfo|String|Body|必填|
data|funsionsDto|Body|必填|

data 说明：
```
{
    "sysProps":{
	"id":"",//根据组件类型确定到底是什么id
"componentId":""，
"componentType":""//组件类型  device：中类组件，model:型号组件，typeId：组件
},//app取出sysProps字段直接
	"actions":[{
		"propId":"",//属性主键
		"desc":"",//组命令前端呈现
"propClass":"",//属性类别
“splitFunc”:””,//拆分标识 0：不拆，1：拆分；
           “fixer”:””,//定语，属性的修饰词：比如“设置为”，“执行”；
		"props":[]//类型为ComponentFunctionPropDto
	}],
	"conditions":[]//数据结构跟actions一致
}
ComponentFunctionPropDto结构如下：
{ 
          "propId":"",//属性主键
"propClass":"",//属性类别
		"desc":"",
          “fixer”:””,//定语，属性的修饰词：比如“设置为”，“执行”；
		"propName":"",//原始命令值，app需要给引擎赋该值
        "functionName":"",//功能标识名称，在基于场景模板创建应用场景时，使用该字段匹配模板中的functionName来确定是否支持目标场景模板（大部分情况下跟propName相同）
		"propValType":"",//取值类型
		"variants":"",//取值范围，为json字符串
		"defaultValue":"",//预留字段，不维护任何值 ，
		"defaultValueDesc":"",//预留字段，不维护任何
}
```

#### 查询是否支持场景功能

> 通过拿多个设备typeId（必填）、型号（选填） 通过接口查询是否有设备组件功能支持场景功能。同时判断每个支持场景功能的设备组件是否可以作为条件、是否可以作为动作。


##### 1、接口定义
?> **接入地址：** `/iftttscene/component/getSceneFunctionSupport`</br>
**HTTP Method：** POST

**输入参数** 

参数名|类型|最大长度|位置|必填|说明
:-|:-:|:-:|:-:|:-:|:-
sceneFunctionSupportDtos|sceneFunctionSupportDto[]||Body|必填|其中型号属性选填，typeId属性必填.其他属性不需要填写。


**输出参数:** 

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|必填|
retInfo|String|Body|必填|
data|SceneFunctionSupportDto[]|Body|必填|


data 说明：
```
{
	[{
		"model":"",//型号
		"typeId":"",//设备类型
          “supportSceneStatus”:””,//true : 支持场景,false : 不支持场景
		“sceneType”:””//1 :  只支持条件  2 :只支持动作  3 : 既支持条件也支持动作  
	}]
}

```

#### 根据中类组件属性ID查询属性信息


##### 1、接口定义
?> **接入地址：** `/iftttscene/component/getPropById`</br>
**HTTP Method：** POST

**输入参数** 

参数名|类型|最大长度|位置|必填|说明
:-|:-:|:-:|:-:|:-:|:-
propId|String||Body|必填|组件属性id


**输出参数:** 

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
retCode|String|Body|必填|
retInfo|String|Body|必填|
data|PropOfComponentDto|Body|必填|

data 说明：
```
{
"id":"",//组件主键
"propName"，"",//标识名称	必填；硬件属性的标识名称，程序读的
"propClass"，"",//属性类别	可取值： property(属性) alarm（告警） operation(操作类属性)，group（组命令）
"functionName"，"",//功能标识名称	程序读的
"description"，"",//显示名称	选填；人读的
"functionDesc"，"",//功能显示名称	人读的
"propValType"，"",//属性值类型 可取值：prop_class为property或者为operation时，可取double,int,bool, string,enum；prop_class为alarm,该字段为null
"readable"，"",是否可读	
"writable"，"",	是否可写
"variants"，"",取值范围	存储取值范围的json字符串
"splitFunc"，"",拆分标识   0：不拆，1：拆分后属性，2：被拆分属性；
}
```





## 使用方式

### 开通流程
![开通流程][DevicesStandard_liucheng]


[^-^]:常用图片注释
[DevicesStandard_type]:_media/_DevicesStandard/DevicesStandard_type.png
[DevicesStandard_liucheng]:_media/_DevicesStandard/DevicesStandard_liucheng.png