
>  **当前版本**：[UWS 场景引擎服务 V2.7.1](zh-cn/ChangeLog/IFTTT)   
 **更新时间**：{docsify-updated}  



## 简介
>为家庭添加场景模板，支持应用开发场景模板，并为应用用户提供场景模板，用户可以基于场景模板添加设备，让设备支持家庭下的场景调用；

![场景引擎图片][IFTTT_type]

1、设备间场景调用，设备状态变化触发其他设备执行相关命令；</br>
2、设备状态告警推送，设备状态定义为特定值后给APP用户推送相关定义的告警信息；</br>
3、设备解绑消息，设备必须被拥有者分享到家庭，并且绑定了有效场景，则无论场景是否开启，一旦解绑设备都会推出设备取消分享家庭的消息并含带场景信息；</br>
4、设备取消分享家庭，如果设备已经绑定了有效场景，无论场景是否开启，一旦设备从家庭退出都会推出设备取消分享家庭的消息并含带场景信息；</br>
5、删除家庭成员，删除的家庭成员如果之前绑定了设备，并且已经将绑定的设备分享到家庭中，而且根据这些设备创建了场景，则无论场景是否开启，一旦家庭成员被删除都会推出删除家庭成员的消息并含带场景信息；</br>
6、用户退出家庭操作，退出家庭的用户如果之前绑定了设备，并且已经将绑定的设备分享到家庭中，而且根据这些设备创建了场景，则无论场景是否开启，一旦用户退出家庭都会推出用户退出家庭的消息并含带场景信息；</br>
7、删除家庭操作，根据家庭必须有可用的场景信息，一旦家庭被删除都会推出删除家庭的消息，并含带场景信息；</br>


## 应用场景
本服务适用于家庭组下的场景应用;  

应用接入场景引擎API，并且配合使用消息推送SDK（消息推送类场景需APP内置SDK);  

**自定义模板方式：** 

海极网—开通场景引擎功能—场景开发portal定义场景模板—提交审核模板—海极网运营审核通过场景模板—在海极网为APP订阅场景模板—APP集成API开发—上线发布使用；

**使用现成模板方式：** 

海极网—开通场景引擎功能—选取已发布上线的场景模板—APP集成API开发—上线发布使用；

## 公共结构  

### SceneDto  
向前兼容场景对象，不能再改。   
  
| **名称** | 场景对象 |&emsp;| SceneDto |   
| ------------- |:----------:|:-----:|:--------:|
|**字段名**|**类型**|**说明**|**备注**|  
|Id| String |场景Id唯一 |不填；UUID|  
|sourceId| String | 场景来源ID |选填；用户自建场景忽略|  
|basisSceneId| String | 基础场景Id |基础场景Id+基础版本号来区分是否需要升级|    
|sceneName| String | 场景名称(应用场景名称) |必填；最大长度255| 
|appSceneAlias| String | 应用场景别名 |选填；最大长度255 用户自建场景忽略| 
|basicSceneAlias| String | 公共场景别名 |选填；最大长度255 用户自建场景忽略| 
|sceneDesc| String | 场景描述 |必填；最大长度600| 
|userId| String | 用户ID |必填；最大长度32| 
|familyId| String | 家庭ID |选填；| 
|type| String| 场景类型|必填；最大长度32 用户自建场景忽略| 
|rules| RuleTemplateDto[] | 规则模板 |必填；可以是多个规则片段组成场景整体规则；一个场景最多支持10个片段；每个片段最多支持25个条件的运算；| 
|auto| boolean | 是否支持自动实例化 |必填；默认为false 用户自建场景忽略 | 
|canAppTrigger| boolean | 是否支持App手动触发执行 |选填，默认是不支持 0:开启后自动执行  1：一键离家| 
|status| String | 基础场景当前状态 |选填；目前只关注三个状态，默认不填为草稿：draft；已发布：publish;已删除:deleted；用户不关注（场景测试app可以根据该字段显示场景模板开发状态）| 
|appId| String | appId |应用标识|  
|isOpen| Integer | 场景是否开启 |1开启，0关闭| 
|weight| Integer | 应用场景权重 |必填 用户自建场景忽略| 
|appSceneStatus| Integer | 应用场景状态 |必填；0:下架  1:上架 用户自建场景忽略| 
|version| String | 应用场景版本 |最大长度32 用户自建场景忽略，但是需要app显示| 
|sortList| List<SceneSortDto> | 应用场景分类列表 |选填；最大长度256 多个以,分割 用户自建场景忽略| 
|tagList| List<SceneTagDto> | 应用场景标签编号 |选填；最大长度256 多个以,分割  用户自建场景忽略| 
|prompt| String | 用户填写的“提示”信息 |最大长度256| 
|createTime| Timestamp | 创建时间 |&emsp;| 
|updateTime| Timestamp | 最后更新时间 |&emsp;| 
|activeBeginTime| Timestamp | 场景生效开始时间 |最大长度64| 
|activeEndTime| Timestamp | 场景生效结束时间 |最大长度64 </br>说明：生效开始时间可以大于结束时间，该情况视为夸天执行，但是最多不能夸24小时</br> 并且生效时间段只对开关类场景起作用，手动触发场景不受限制| 
|recommend| String | 推荐 |最大长度64 用户自建场景忽略| 
|collect| collect | 收藏 |最大长度64 用户自建场景忽略| 
|aiKeyWord| String | 语音标签 |最大长度256| 
|banner| String | 场景banner图URL |最大长度256| 
|icon| String | 场景图标URL |最大长度256| 
|inoutSide| Integer | 场景标识 |0:外部-非官网  1:内部-官网 用户自建场景忽略| 
|basisSceneVersion| String | 系统场景版本 |最大长度32 用户自建场景忽略| 
|basisSceneName| String | 系统场景名称 |最大长度64 用户自建场景忽略| 
|basisSceneDescription| String | 系统场景描述 |最大长度1800 用户自建场景忽略| 
|basisSceneTags| String | 系统场景标签 |最大长度256 平台选择项（空调、新风机等）用户自建场景忽略| 
|createUserNickname| String | 场景开发者展示名称 |最大长度64 场景开发者在海极网填写的昵称信息 用户自建场景忽略| 
|createUserLogo| String | 场景开发者展示LOGO |最大长度256 场景开发者在海极网配置的logo信息PNG或JPG格式，128*128或64*64规则 用户自建场景忽略 |
|taskInfoList| List<TaskInfoDto>| 定时策略信息 |&emsp;|
|eVersion| String | 引擎执行版本号 |最大32位，不需要app前端关注|
|aVersion| String | 应用场景版本号 |&emsp;|
|uVersion| String | 用户场景版本号 |最大32位|
|bVersion| String | 基础场景版本号 |&emsp;|
|dType| String | 下载类型</br>Basis：基础场景；</br>App：应用场景；</br>User：用户自建 |最大16位|
|triggerType| String | 是否支持App手动触发执行 |选填，用户自建场景忽略 目前取值：</br>platform：平台触发，manually：手动触发，timerTrigger：时间触发；</br>注释：该字段由app定义，app可根据该字段配合手动触发机制实现地图围栏，天气等业务|

### BasisSceneDto  
基础场景对象  
  
| **名称** | 基础场景对象 |&emsp;| BasisSceneDto |   
| ------------- |:----------:|:-----:|:--------:|
|**字段名**|**类型**|**说明**|**备注**|  
| Id  |  String  |  场景Id唯一   |基础场景Id+基础版本号来区分是否需要升级|
|  sceneName |  String  |  场景名称(应用场景名称)  |必填；最大长度255|
| sceneAlias  |  String  |  场景别名  |选填；最大长度255 用户自建场景忽略|
| sceneDesc  |  String  | 场景描述    |必填；最大长度600|
| type  |  String  |  场景类型   |必填；最大长度32 用户自建场景忽略|
| rules  | RuleTemplateDto[]  |  规则模板   |必填；可以是多个规则片段组成场景整体规则；一个场景最多支持10个片段；每个片段最多支持25个条件的运算；|
|  auto |  Boolean  |  是否支持自动实例化   |&emsp;|
|  triggerType |  Boolean  |   是否支持App手动触发执行  |选填，默认是不支持 false:平台触发类场景  true：手动触发场景；空类型为平台类场景 对应之前的canAppTrigger|
|  status |  String  |   基础场景当前状态  |选填；目前只关注三个状态，默认不填为草稿：draft；已发布：publish;已删除:deleted；用户不关注（场景测试app可以根据该字段显示场景模板开发状态）|
| version  |  String  |   场景版本  |最大长度32（场景模板Id+版本号唯一标识规则信息）|
|  createTime |  Timestamp  |   创建时间  |&emsp;|
|  updateTime |  Timestamp  |   最后更新时间  |&emsp;|
|  taskInfo |  TaskInfoDto  |  时间策略信息   |有俩策略，周期生效（只支持平台触发类），定期执行（只支持手动触发类）同一个场景只能支持一个策略|
|  aiKeyWord |  String  |   语音标签  |最大长度256|
|  banner |  String  |  场景banner图URL   |最大长度256|
| icon  |  String  |   场景图标URL  |最大长度256|
| createUserId  |  String  |   场景创建者Id  |最大64|
|  createUserNickname |  String  |   场景开发者展示名称  |最大长度64 场景开发者在海极网填写的昵称信息 用户自建场景忽略|
|  createUserLogo |  String  |   场景开发者展示LOGO  |最大长度256 场景开发者在海极网配置的logo信息PNG或JPG格式，128*128或64*64规则 用户自建场景忽略|
|  engineVersion |  String  |   引擎执行版本号  |最大32位 当前版本号为1.0（来源由场景引擎提供，Portal入库需要提供该版本号）|
 

### AppSceneDto  
应用场景对象  
  
| **名称** | 应用场景对象 |&emsp;| AppSceneDto |   
| ------------- |:----------:|:-----:|:--------:|
|**字段名**|**类型**|**说明**|**备注**|  
| Id | String  |  场景Id唯一 |基础场景Id+基础版本号来区分是否需要升级|
| sceneName | String  |  场景名称(应用场景名称) |必填；最大长度255|
| sceneAlias | String  |  场景别名 |选填；最大长度255 用户自建场景忽略|
| sceneDesc | String  | 场景描述  |必填；最大长度600|
| type | String  | 场景类型  |必填；最大长度32|
| rules | RuleTemplateDto[]  |  规则模板 |必填；可以是多个规则片段组成场景整体规则；一个场景最多支持10个片段；每个片段最多支持25个条件的运算；|
| auto | Boolean  | 是否支持自动实例化  |&emsp;|
| triggerType | Boolean  | 是否支持App手动触发执行  |选填，默认是不支持 false:平台触发类场景  true：手动触发场景；空类型为平台类场景 对应之前的canAppTrigger|
| status | String  | 场景当前状态  |选填；目前只关注三个状态，默认不填为草稿：draft；已发布：publish;已删除:deleted；用户不关注（场景测试app可以根据该字段显示场景模板开发状态）|
| version | String  |  基础场景版本 |最大长度32（基础场景模板Id+版本号唯一标识规则信息）|
| weight | Integer  | 用户场景权重  |选填 按该字段进行排序|
| createTime | Timestamp  | 创建时间  |选填|
| updateTime | Timestamp  |  最后更新时间 |选填|
| taskInfo | TaskInfoDto[]  |  时间策略信息 |选填,有俩策略，周期生效（只支持平台触发类），定期执行（只支持手动触发类）同一个场景只能支持一个时间策略|
| aiKeyWord | String  |  语音标签 |最大长度256|
| banner | String  |  场景banner图URL |最大长度256|
| icon | String  |  场景图标URL |最大长度256|
| createUserId | String  |  场景创建者Id |最大64|
| createUserNickname | String  |  场景开发者展示名称 |最大长度64 场景开发者在海极网填写的昵称信息 用户自建场景忽略|
| createUserLogo | String  |  场景开发者展示LOGO |最大长度256 场景开发者在海极网配置的logo信息PNG或JPG格式，128*128或64*64规则 用户自建场景忽略|
| engineVersion | String  |  引擎执行版本号 |最大32位 当前版本号为1.0（来源由场景引擎提供，Portal入库需要提供该版本号）|

### UserSceneDto  
用户场景对象  
  
| **名称** | 用户场景对象 |&emsp;| UserSceneDto |   
| ------------- |:----------:|:-----:|:--------:|
|**字段名**|**类型**|**说明**|**备注**|  
|id                   |   String           |  场景Id唯一|                                   不填；UUID|
|sourceId             |   String           |  基础场景ID|                                   选填，用户自建场景忽略|
|sourceVersion        |   String           |  基础场景版本号|                               选填，用户自建场景忽略|
|sceneName            |   String           |  场景名称(应用场景名称)|                       必填；最大长度255|
|sceneAlias           |   String           |  场景别名|                                     选填；最大长度255 |
|sceneDesc            |   String           |  场景描述|                                     必填；最大长度600|
|userId               |   String           |  用户ID|                                       必填；最大长度32|
|familyId             |   String           |  家庭ID|                                       必填；|
|type                 |   String           |  场景类型|                                     必填；最大长度32 用户自建场景忽略|
|rules                |   RuleTemplateDto[]|  规则模板|                                     必填；可以是多个规则片段组成场景整体规则；一个场景最多支持10个片段；每个片段最多支持25个条件的运算；|
|triggerType          |   String           |  是否支持App手动触发执行|                      选填，用户自建场景忽略 目前取值：platform：平台触发，manually：手动触发：timerTrigger：时间触发；注释：该字段由app定义，app可根据该字段配合手动触发机制实现地图围栏，天气等业务|
|appId                |   String           |  appId|                                        应用标识|
|isOpen               |   Integer          |  场景是否开启|                                 1开启，0关闭;选填|
|weight               |   Integer          |  用户场景权重|                                 选填 按该字段进行排序|
|sortList             |   List<SceneSortDto|  用户场景分类列表|                             选填；最大长度256 多个以,分割 分类只能有一个|
|tagList              |   List<SceneTagDto>|  用户场景标签编号|                             选填；最大长度256 多个以,分割 （1、如果场景类型为用户自建，则该标签可以被修改；2、如果场景类型为模板下载，该标签不能被用户修改）|
|createTime           |   Timestamp        |  创建时间|                                     用户自建场景忽略|
|updateTime           |   Timestamp        |  最后更新时间|                                 用户自建场景忽略|
|taskInfoList | List<TaskInfoDto>|  时间策略信息|                                 选填；有俩策略，周期生效（只支持平台触发类），定期执行（只支持手动触发类）如果是定时执行类场景则必填 同一个场景只能支持一个策略|  
|aiKeyWord            |   String           |  语音标签|                                     选填；最大长度256|
|banner               |   String           |  场景banner图URL|                              最大长度256 如果场景为下载类型，则来源于场景开发者相关信息，如果为用户自建，则需用户配置该字段；|
|icon                 |   String           |  场景图标URL|                                  选填；最大长度256|
|createUserNickname   |   String           |  场景开发者展示名称|                           最大长度64 场景开发者在海极网填写的昵称信息 用户自建场景忽略|
|createUserLogo       |   String           |  场景开发者展示LOGO|                           最大长度256 场景开发者在海极网配置的logo信息PNG或JPG格式，128*128或64*64规则 用户自建场景忽略|
|engineVersion        |   String           |  引擎执行版本号|                               最大32位，该值为引擎赋值 用户自建场景忽略|
|createType           |   String           |  场景创建方式 Basis：下载方式； User：自建方式|最大16位 用户自建场景忽略|  
|version     |   String   |  引擎数据版本号|此值来源于接口header公共部分version字段值|  


### RuleTemplateDto  
规则对象  
  
| **名称** | 规则对象 |&emsp;| RuleTemplateDto |   
| ------------- |:----------:|:-----:|:--------:|
|**字段名**|**类型**|**说明**|**备注**|  
|id       |String      |规则Id                          |&emsp;                                                     |
|rule     |String      |规则名称                        |选填；如果不填则默认值为场景名称+当前规则下标              |
|desc     |String      |规则描述                        |必填；用户自建场景忽                                       |
|salience |Integer     |规则执行优先级数字越大则越先执行|选填；如果不填则默认为按照规则下标顺序执行 用户自建场景忽略|
|when     |RuleWhenDto |规则条件                        |选填；如果不填说明是while(true)直接执行                    |
|then     |RuleThenDto |规则触发                        |选填；                                                     |
|isOpen   |Integer     |规则片段是否开启                |1开启，0关闭                                               |


### RuleWhenDto  
规则条件对象  
  
| **名称** | 规则条件对象 |&emsp;| RuleWhenDto |   
| ------------- |:----------:|:-----:|:--------:|
|**字段名**|**类型**|**说明**|**备注**|  
|conditions|RuleWhenCondition[]|条件对象数组     |选填；表示when里边的条件组合   |
|desc      |String             |表示when字段描述 |选填；用户自建场景忽略         |


### RuleWhenCondition  
条件对象  
  
| **名称** | 条件对象 |&emsp;| RuleWhenCondition |   
| ------------- |:----------:|:-----:|:--------:|
|**字段名**|**类型**|**说明**|**备注**|  
|id             | String              |条件ID                                                                                                                 |  UUID                                                                                                                                                          |
|object  | StatusDesc   |条件载体 如果条件为设备条件则该值为设备信息，value为设备mac；desc为设备昵称 。如果条件为天气信息，value为天气信息|  详见下方|                                                                                                                                          
|desc           | String              |条件描述                                                                                                               |  目前是程序组装，只有叶子节点条件才会组装，上层结构不会 用户自建场景忽略                                                                                       |
|key            | StatusDesc          |标准模型name；OS需要录入StatusDesc的ID；标识某个组件的属性ID；OS查询条件返回组件属性的描述（值和描述，还有组件属性的ID）| 选填；                                                                                                                                                        |
|operationSign  | OperationSign       |关系运算符（枚举类型）                                                                                                 |  选填；取值如下：</br>`greaterThan(">"),`</br>`greaterThanEqual(">="),`</br>`equal("=="),`</br>`lessThan("<"), `</br>`lessThanEqual("<="),`</br>`unequal(“!=)` |
|value          | StatusDesc          |标准模型值 |  选填；  |                                                                                                                                                                                                                                                                 
|logicalSign    | LogicalSign         |逻辑运算符（枚举类型）|该条件对应上一个平行条件的逻辑运算，取值如下</br>`AND(”&&”) OR("`&#124;&#124;`")` |                                                                                                                                                                                      
|conditions     | RuleWhenCondition[] |子条件对象数组|选填；说明：举例</br>`if(a`&#124;&#124;`b)` 这种格式那么`conditions`为`null`；</br>除了`logicalSign`其它必填；</br>`if((a`&#124;&#124;`b)&&(c`&#124;&#124;`d))`这种格式则`conditions`必填；</br>其它都为`null`；      |                                                                                                        
|componentId    | String              |所属组件Id  |  必填；由组件区分是设备组件还是其它组件   |                                                                                                                                                                                                                                                                                                                         

object字段说明： </br>  
  
```     
备注：该字段兼容新老数据  
1.	条件为设备条件则该值为设备信息，value为设备{"mac":"DC330DC51955","clazz":"CAS362VBA(A1)U1套机"}；   
2.	如果为天气条件，则value为：   
{"id":"城市code编码","type":"","name":"城市名字"}；   

```


### RuleThenDto  
规则触发动作  
  
| **名称** | 规则触发动作 |&emsp;| RuleThenDto |   
| ------------- |:----------:|:-----:|:--------:|
|**字段名**|**类型**|**说明**|**备注**|  
|conditions|RuleThenAction[]|需要触发的动作     |选填；表示when里边的条件组合   |
|desc      |String             |表示when字段描述 |选填；用户自建场景忽略         |

### RuleThenAction  
规则触发动作详细描述  
  
| **名称** | 规则触发动作详细描述 |&emsp;| RuleThenAction |   
| ------------- |:----------:|:-----:|:--------:|
|**字段名**|**类型**|**说明**|**备注**|  
|id          |String                |行为ID                   |主键ID      UUID|                                                                              
|desc        |String                |行为动作描述             |选填|
|type        |ActionEnum            |需要触发的动作类型枚举类 |取值如下：DeviceControl：设备控制 MessagePush：消息推送 MessagePushWithControl：带小循环控制的消息|
|dealyTime   |Integer               |动作延迟执行时间         |选填；取值为数字 单位为秒 说明该动作需要在触发执行后多少秒后触发 如果不填或者0则认为不延迟 用户自建场景忽略|
|control     |RuleThenDeviceControl |设备控制动作             |选填；如果type值为：DeviceControl control必填，pushMessage为空；如果type为MessagePushWithControl 则control和pushMessage都必填|
|pushMessage |RuleThenPushMessage   |消息推送动作             |选填；如果选择消息推送则control为空，pushMessage必填|
|isOpen      |Integer               |动作片段是否开启         |1开启，0关闭|


### RuleThenPushMessage  
消息推送动作  
  
| **名称** | 消息推送动作 |&emsp;| RuleThenPushMessage | 
| ------------- |:----------:|:-----:|:--------:|
|**字段名**|**类型**|**说明**|**备注**|  
|pushType    | PushType          |推送类型 ，枚举 |必填;取值：</br>单设备推送：device,</br>帐号全设备推送：user,</br>帐号部分设备推送：user_device,</br>按照家庭推送：family,</br>家庭下部分设备推送：family_device,</br>按照用户手机推送：user_mobile|
|pushContent | PushContent       |发送消息内容dto |必填|
|showTypes   | Map<String,String>|终端显示类型    |取值：</br>Key取值为：</br>01：冰箱 09：烟机</br>0F:电视 phone:手机</br>tablet:平板 smc:smartCenter</br>Value取值：</br>0: toast 2: 弹框</br>3: push+弹框4: 红色感叹号显示;</br>Value具体定义遵循上一版本弹框格式|
|msgStrategy | MessageStrategy   |消息发送策略    |选填|
|priority    | Integer           |消息优先级      |具体和《ums3.0接口定义说明书-标准版》保持一致，示意如下:</br>0 – 紧急消息</br>1 – 一般消息，如果不填则默认为一般消息</br>2 – 中低消息</br>3 – 低级消息|



### MessageStrategy  
消息推送策略  
  
| **名称** | 消息推送策略 |&emsp;| MessageStrategy |   
| ------------- |:----------:|:-----:|:--------:| 
|**字段名**|**类型**|**说明**|**备注**|   
|timeInterval     |Integer |推送间隔时间   |选填；单位：分钟|
|notWorkTimeStart |String  |免打扰开始时间 |HH:mm选填；     |
|notWorkTimeEnd   |String  |免打扰结束时间 |HH:mm选填；     |
|maxCount         |Long    |最大发送条数   |选填；          |


### PushContent  
推送内容  
  
| **名称** | 推送内容 |&emsp;| PushContent |   
| ------------- |:----------:|:-----:|:--------:| 
|**字段名**|**类型**|**说明**|**备注**|   
|msgName    |String |消息名称     |必填                    |
|expires    |int    |消息过期时间 |单位为分钟 ；默认60分钟 |
|msgTitle   |String |消息标题     |必填                    |
|msgContent |String |消息内容     |必填                    |  


### RuleThenDeviceControl  
设备控制动作  
  
| **名称** | 设备控制动作 |&emsp;| RuleThenDeviceControl |   
| ------------- |:----------:|:-----:|:--------:| 
|**字段名**|**类型**|**说明**|**备注**|   
|object        |StatusDesc         |动作载体 为设备信息，value为设备mac；desc为设备昵称| &emsp;                                                                                  |
|args          |DeviceControlArgs[]|控制命令集合                                      |  选填；如果为组命名则args有可能为空                                                      |
|controlBtnText|String             |带控制的消息的相关按钮信息                        |  选填；消息推送带小循环控制 时必填 长度最大为10                                          |
|componentId   |String             |组件id                                            |  必填；                                                                                  |
|operation     |StatusDesc         |操作名称                                          |  选填；详见下方|

operation字段说明：</br>
```    
选填；  
V2.2版本args跟operation；   
单命令控制不需要设置该值；   
组命令控制需要设置操作名称。  
备注：    
"operation": {
    "id": "f313ddbcb49d11e798b8fa163eb273a5_0",
    "value": "空调参数设置(组命令)_分体空调1.0净化模式组命令",
    "desc": "分体空调1.0净化模式组命令",
    "required": false
  }


```

### DeviceControlArgs  
设备控制命令参数  
  
| **名称** | 设备控制命令参数 |&emsp;| DeviceControlArgs |   
| ------------- |:----------:|:-----:|:--------:| 
|**字段名**|**类型**|**说明**|**备注**|   
|name  |StatusDesc |控制标准模型名称     |必填        |
|value |StatusDesc    |控制标准模型值 |选填；原始值放入value属性，递增递减值放入changeValue |
|changeValue|StatusDesc |标准模型的改变值（增加或减少相应单位量）|选填；原始值放入value属性，递增递减值放入changeValue |


### RuleDeviceDesc  
设备描述  
  
| **名称** | 设备描述 |&emsp;| RuleDeviceDesc |   
| ------------- |:----------:|:-----:|:--------:| 
|**字段名**|**类型**|**说明**|**备注**|  
|id  |String |设备主键     |必填        |
|bigClass |StatusDesc    |设备大类 |必填 |
|middleClass|StatusDesc |设备中类|必填|


### ComponentDto  
组件信息  
  
| **名称** | 组件信息 |&emsp;| ComponentDto |   
| ------------- |:----------:|:-----:|:--------:| 
|**字段名**|**类型**|**说明**|**备注**| 
|id           |String                   | 组件主键                  |必填                                                                                                               |
|componentName|String                   | 组件名称                  |必填                                                                                                               |
|componentDesc|String                   | 组件描述                  |必填                                                                                                               |
|componentType|ComponentType            | 组件类型                  |枚举类型 设备组件：</br>中类组件：DEVICE("device")</br>typeId组件：TYPEID（“typeId”）</br>型号组件：MODEL(“model”) |
|deviceDesc   |ComponentDeviceDesc      | 组件所属设备标准模型类型  |选填，只有当组件类型为 设备组件：DEVICE("device") 该属性才有值                                                     |
|wifitypeList |String[]                 | 组件支持的wifitype列表    |如果该字段为空，则支持中类下所有typeid设备，如果不为空，则组件只支持返回的typeid列表                               |
|propList     |List<PropOfComponentDto> | 组件相关的属性列表        |选填                                                                                                               |

### PropOfComponentDto  
组件下的属性信息  
  
| **名称** | 组件下的属性信息 |&emsp;| PropOfComponentDto |   
| ------------- |:----------:|:-----:|:--------:| 
|**字段名**|**类型**|**说明**|**备注**| 
|id          |String   | 组件主键     |选填；最大长度32位                                                                                           |
|propName    |String   | 标识名称     |必填；硬件属性的标识名称，程序读的                                                                           |
|propClass   |String   | 属性类别     |可取值： property(属性) alarm（告警） operation(操作类属性)，group（组命令）                                 |
|functionName|String   | 功能标识名称 |程序读的                                                                                                     |
|description |String   | 显示名称     |选填；人读的                                                                                                 |
|functionDesc|String   | 功能显示名称 |人读的                                                                                                       |
|propValType |String   | 属性值类型   |选填：可取值：prop_class为property或者为operation时，可取double,int,bool,enum；prop_class为alarm,该字段为null|
|readable    |boolean  | 是否可读     |选填                                                                                                         |
|writable    |boolean  | 是否可写     |选填                                                                                                         |
|variants    |String   | 取值范围     |存储取值范围的json字符串                                                                                     |

### FunctionsDto  
组件功能列表  
  
| **名称** | 组件功能列表 |&emsp;| FunctionsDto |   
| ------------- |:----------:|:-----:|:--------:| 
|**字段名**|**类型**|**说明**|**备注**| 
|sysProps  |SysPropDto |app用系统属性     |&emsp;        |
|conditions |List<ConditionOrActionDto>    |条件 |&emsp; |
|actions|List<ConditionOrActionDto> |动作|组命令仅用作动作|


### SysPropDto  
app用系统属性  
  
| **名称** | app用系统属性 |&emsp;| SysPropDto |   
| ------------- |:----------:|:-----:|:--------:| 
|**字段名**|**类型**|**说明**|**备注**| 
|id  |String |主键     |typeId或型号映射表的主键  |
|componentId |String   |组件ID |&emsp; |
|componentType|ComponentType |组件类型|typeId组件：TYPEID（“typeId”）</br>型号组件：MODEL(“model”)|

### ConditionOrActionDto  
条件或者动作  
  
| **名称** | 条件或者动作 |&emsp;| ConditionOrActionDto |   
| ------------- |:----------:|:-----:|:--------:| 
|**字段名**|**类型**|**说明**|**备注**| 
|propId |String                            |主键         |单命令：属性主键；</br>组命令：组命令功能主键             |
|desc   |String                            |描述         |单命令：属性标识显示名称；</br>组命令：组命令功能显示名称 |
|fixer  |String                            |属性修饰词   |&emsp;                                                    |
|props  |List< ComponentFunctionPropDto >  |功能属性集合 |&emsp;                                                    |

### ComponentFunctionPropDto
功能属性信息  
  
| **名称** | 功能属性信息 |&emsp;| ComponentFunctionPropDto |   
| ------------- |:----------:|:-----:|:--------:| 
|**字段名**|**类型**|**说明**|**备注**| 
|propId           |String |属性主键          |&emsp;                  | 
|propName         |String |标识名称          |&emsp;                  |
|fixer            |String |属性修饰词        |&emsp;                  |
|functionName     |String |功能标识名称      |&emsp;                  |
|desc             |String |属性标识显示名称  |&emsp;                  |
|propValType      |String |属性值类型        |&emsp;                  |
|variants         |String |取值范围          |存储取值范围的json字符串|
|defaultValue     |String |默认值            |预留字段，不维护任何值  |
|defaultValueDesc |String |默认值描述        |预留字段，不维护任何值  |




### SceneFunctionSupportDto
场景功能支持对象  
  
| **名称** | 场景功能支持对象 |&emsp;| SceneFunctionSupportDto |   
| ------------- |:----------:|:-----:|:--------:| 
|**字段名**|**类型**|**说明**|**备注**|
|model  |String |设备型号     |&emsp;        |
|typeId |String   |设备类型 |&emsp; |
|isSupportScene|Boolean |是否支持场景|true : 支持场景</br>false：不支持场景|
|sceneType  |Int |支持场景的类型     |1. 只支持行为</br> 2. 只支持动作  </br>3. 既支持行为也支持动作  |  




### StatusDesc
字段取值和描述  
  
| **名称** | 字段取值和描述 |&emsp;| StatusDesc |   
| ------------- |:----------:|:-----:|:-----:| 
|**字段名**|**类型**|**说明**|**备注**|
|id        |String             |属性Id               |选填；如果为条件的Key，则Id表示组件中的某个属性的ID|
|value     |String             |字段值               |选填；|
|desc      |String             |字段值中文描述       |选填；|
|scope     |Map<String,String> |取值范围，跟标准同步 |选填；数据结构：</br>`{`</br>`“type”:”enum”,`</br>`“value:”json”`</br>`}`</br>type类型遵循PropOfComponentDto中的PropValType </br>字段描述本结构中value的取值范围，</br>只有在具体的属性中才有值，</br>表示这个属性的取值范围；不在属性的值中出现|
|required  |Boolean            |属性是否必填         |如果为true表示app实例化需要填写此参数|



### Pagination
分页对象  
  
| **名称** | 分页对象 |&emsp;| Pagination |   
| ------------- |:----------:|:-----:|:-----:|  
|**字段名**|**类型**|**说明**|**备注**|
|pageSize    |int     |每页显示的条数    |必填；如果为游标类型，则pageSize表示向下加载多少                |
|cursor      |int     |游标              |如果分页类型为游标（App向下滑动）则必填；如果为普通分页则不需要；|
|recordCount |int     |总记录数          |必填；                                                          |
|currentPage |int     |当前页面          |必填；                                                          |
|totalPage   |int     |总页数            |必填；                                                          |
|list        |`List<T>` |每页返回的对象列表|必填；                                                          |


### RuleSettings
规则配置参数  
  
| **名称** | 规则配置参数 |&emsp;| RuleSettings |   
| ------------- |:----------:|:-----:|:-----:|  
|**字段名**|**类型**|**说明**|**备注**|
|type  |String |条件或者行为    |必填；枚举类型可选值condition或者action|
|id |String   |条件或者行为Id |必填；取决于type |
|value  |Map<String,String> |条件或者行为中的取值    |选填；数据结构定义：</br>`{`</br>`“value”:”open”, //具体的值；`</br>`“desc”:”开机”   //具体值的描述`</br> `}`|   
|object |Map<String,String>   |设备参数或者天气参数保存 |必填；表示设备参数或者天气参数 | 
|userSettingGroupPropDtoList |List<UserSettingGroupPropDto>|用户设置的参数 |选填；备注：动作为组命令，用户设置的参数信息 | 
|dealyTime |Integer|延迟时间 |选填；取值为数字 单位为秒 说明该动作需要在触发执行后多少秒后触发如果不填或者0则认为不延迟|  
|sceneId |String   |场景id |选填| 

object字段说明：</br>   
```  
数据结构定义：
{
   “id”: “mac1”,		//存放设备mac或者城市code编码
   “name”:”城市名字”,	//存放城市名字
   “type”:"03012"		//存放设备的大类中类,model，如果为天气类型，则默认为天气组件类型
}
如果同一型号需要传入多个设备，数据格式如下：
{"mac":"mac1,mac2,mac3","clazz":"02012"}
备注：之前版本存入device字段的信息，现在都存储在该字段，具体结构如上。
```
### SceneSortDto
场景分类对象  
  
| **名称** | 场景分类对象 |&emsp;| SceneSortDto |   
| ------------- |:----------:|:-----:|:-----:|  
|**字段名**|**类型**|**说明**|**备注**|
|id |Integer   |id |必填；最大长度32 |
|name |String   |分类名称 |必填；最大长度32 |


### SceneTagDto
场景标签对象  
  
| **名称** | 场景标签对象 |&emsp;| SceneTagDto |   
| ------------- |:----------:|:-----:|:-----:|  
|**字段名**|**类型**|**说明**|**备注**|
|id |Integer   |id |必填；最大长度32 |
|name |String   |标签 |必填；最大长度32 |

### TaskInfoDto
定时策略详情  
  
| **名称** | 定时策略详情 |&emsp;| TaskInfoDto |   
| ------------- |:----------:|:-----:|:-----:|  
|**字段名**|**类型**|**说明**|**备注**|
|type            |Int      |策略类型：</br>1：定时执行策略（针对手动触发类场景）；</br>2：生效时间策略（针对平台执行类场景）；| &emsp;|
|cron            |CronDto  |定时表达式                                                                                       |  &emsp;|
|activeBeginTime |String   |场景生效开始时间                                                                                 |  最大长度64 采用24小时时间格式；</br>即早上9点表示为“`09:00:00`”，晚上9点表示为“`21:00:00`”，</br> 配置时间的颗粒度到具体秒级别，</br>即“小时-分钟-秒”格式；如 “`08:00:02`”代表早8点0分2秒</br>activeEndTime与activeBeginTime要求一致|
|activeEndTime   |String   |场景生效结束时间                                                                                 |  最大长度64 说明：</br>生效开始时间可以大于结束时间，该情况视为夸天执行，</br>但是最多不能夸24小时 并且生效时间段只对开关类场景起作用，手动触发场景不受限制|
|status          |Boolean  |定时状态，true开启，false关闭                                                                    |  &emsp;|

### CronDto
Cron表达式说明  
  
| **名称** | Cron表达式说明 |&emsp;| CronDto |   
| ------------- |:----------:|:-----:|:-----:|  
|**字段名**|**类型**|**说明**|**备注**|
|minutes |String|必填，分钟；| 取值范围`（0-59`）；允许特殊字符`（, - * /）`                 |
|hours   |String|必填，小时 |  取值范围`（0-23）`；允许特殊字符`（, - * /）`                 |
|day     |String|必填，天   |  取值范围`（1-31）`；允许特殊字符`（, - * ? / L W）`           |
|month   |String|必填，月   |  取值范围`（1-12 or JAN-DEC）`；允许特殊字符`（, - * /）`      |
|week    |String|必填，周   |  取值范围（`1-7 or SUN-SAT）`；允许特殊字符`（, - * ? / L #）` |
|year    |String|选填，年   |  可为空，取值范围`（1970-2099）`；允许特殊字符`（, - * /）`    |


### SceneExecuteLogDto
场景执行日志  
  
| **名称** | 场景执行日志 |&emsp;| SceneExecuteLogDto |   
| ------------- |:----------:|:-----:|:-----:|  
|**字段名**|**类型**|**说明**|**备注**|
|id          |Long   |自增主键                     |必填；                                                                                                           |
|sn          |String |sn号                         |必填                                                                                                             |
|sceneId     |String |场景Id                       |选填：最大长度32                                                                                                 |
|sceneName   |String |场景名称                     |选填：最大场景225                                                                                                |
|sceneAlias  |String |场景别名                     |选填：最大长度225                                                                                                |
|sceneDesc   |String |场景描述                     |选填：最大长度225                                                                                                |
|instanceId  |String |实例Id                       |必填：最大长度32                                                                                                 |
|familyId    |String |家庭Id                       |必填：最大长度32                                                                                                 |
|logType     |String |日志类型                     |必填：最大长度32，日志类型分三种：</br>messagePush消息推送</br>deviceControl设备控制</br>reservationTime预约定时 |
|triggerMac  |String |触发场景的设备mac            |必填：最大长度32                                                                                                 |
|deviceId    |String |设备控制时，需要控制的设备Id |必填：最大长度32                                                                                                 |
|Business    |String |业务信息json结构             |必填：最大长度600                                                                                                |
|retCode     |String |请求返回码                   |必填：最大长度32                                                                                                 |
|retInfo     |String |请求结果                     |必填：最大长度32                                                                                                 |
|executeTime |String |请求时间                     |选填：最大长度64                                                                                                 |

### SceneOperationLogDto
场景操作日志  
  
| **名称** | 场景操作日志 |&emsp;| SceneOperationLogDto |   
| ------------- |:----------:|:-----:|:-----:|  
|**字段名**|**类型**|**说明**|**备注**|
|id              |Long                 |自增主键          |必填；                                                                                      |
|sceneId         |String               |场景Id唯一        |必填; 最大长度32                                                                            |
|sceneName       |String               |场景名称          |必填；最大长度255                                                                           |
|sceneAlias      |String               |场景别名          |选填；最大长度255                                                                           |
|sceneDesc       |String             |场景描述          |必填；最大长度600                                                                           |
|userId          |String               |用户ID            |选填；最大长度32                                                                            |
|familyId        |String               |家庭ID            |必填；最大长度32                                                                            |
|operationType<font color="#FF0000">(弃用)</font>   |Int                  |操作类型          |必填；</br>1： 开启场景</br>2： 关闭场景</br>3： 触发场景                                   |
|operationStatus<font color="#FF0000">(弃用)</font> |Boolean              |操作状态          |必填；true : 成功   false: 失败                                                             |
|operationResult |String               |执行结果       |必填；最大长度32</br>执行成功 </br>执行中 </br>执行失败          |
|time            |String                |操作时间          |必填；时间戳</br>例如： 1530467982731                                                       |
|sn              |String               |sn号              |选填；默认值为0                                                                             |
|status          |Int                  |场景状态          |场景分三个状态:（必填）</br>0:失败</br>1:成功</br>2:执行中                                  |
|actionResultList|List<ActionResultDto>|动作执行结果信息  |场景动作执行结果列表（选填）</br>执行成功：显示具体动作执行成功的信息；</br>执行失败：显示具体动作执行失败的信息及失败的原因；</br>执行中：显示已经处理完毕的动作（成功或者失败）动作未处理完毕的动作不显示 |



### ActionResultDto
动作执行结果信息  
  
| **名称** | 动作执行结果信息 |&emsp;| ActionResultDto |   
| ------------- |:----------:|:-----:|:-----:|  
|**字段名**|**类型**|**说明**|**备注**|
|code |String   |动作执行状态 |必填：</br>场景动作执行返回的结果状态:</br>00000:执行成功</br>00001:执行失败</br>00002:执行中|
|info |Object   |动作执行结果信息 |必填; 场景动作执行失败原因 |
|param |RuleThenAction   |动作详细信息 |必填；动作的详细信息</br>param参数跟动作详情RuleThenAction的结构一致 |

`info`字段说明：</br>  
场景动作执行结果信息</br>  
如果动作是执行中.返回处理中的”执行中”字符串信息,如果动作处理有结果,显示动作处理结果信息，具体结构如下：</br>  
```
[{
        “execResult”:false,
		"execResultCode": "S03002", 
		"execResultInfo": "设备离线",
		"deviceId": DC330DC51955
}]
```
目前领域模型提供的设备控制返回的错误码和错误信息有四类：</br> 
00000（成功）、S03002（设备离线）、S03001（不支持的命令）、S00001（未知异常）</br> 
一个动作下单条指令执行结果由字段execResult判断；</br> 
成功为true，对应的execResultCode：00000，execResultInfo：“成功”；</br> 
失败为false；对应的execResultCode为S03002,S03001,S00001（目前就这三种）</br> 
execResultinfo在上面对应</br> 
如果动作为消息推送，则成功码为00000，其它对应详细失败吗，码表由UMS统一提供，后续会附带；</br> 
动作是何种类型取决于下面的param字段的动作类型</br>  

### UserSettingGroupPropDto(组命令属性参数)


| **名称** | UserSettingGroupPropDto(组命令属性参数) |&emsp;| UserSettingGroupPropDto |   
| ------------- |:----------:|:-----:|:-----:|  
|**字段名**|**类型**|**说明**|**备注**|
|id |String   |id |必填|
|value |StatusDesc   |用户设置的属性值和属性值描述 |必填 |
|propId |String   |属性id |必填 |

## 接口清单

### 场景类
> API接口总览

| API名称        | 作用          | 是否开放  | 特别说明|
| ------------- |:-------------:|:-----:|:-------------:|
| 批量下载场景 | 从场景Store中批量下载场景 | 是| 无|  
| 批量下载基础场景| 从场景Store中下载基础场景 | 是| 无|  
| 批量下载应用场景 | 批量下载应用场景 | 是| 无|  
| 获取家庭下场景列表| APP获取家庭下场景列表 | 是| 无|  
| 查询家庭下场景列表| 包括用户自建场景和通过模板下载的场景 | 是| 无|  
| 获取家庭下应用场景列表| 需要区分具体的终端(AppId区分) | 是| 无|  
| 根据家庭下的场景Id批量查询场景列表| 根据家庭下的场景Id批量查询场景列表 | 是| 无| 
| 修改场景昵称| 修改场景昵称（别名） | 是| 无| 
| 修改场景生效时间段| 修改场景生效时间段 | 是| 无| 
| 删除用户下载的场景| 删除用户下载的场景 | 是| 无| 
| 用户创建平台触发类场景| 根据用户填写的参数保存场景 | 是| 无| 
| 用户创建手动触发类场景| 根据用户填写的参数保存场景 | 是| 无| 
| 用户创建定时执行类场景| 根据用户填写的参数保存场景 | 是| 无| 
| 修改用户平台触发类场景| 修改用户平台触发类场景 | 是| 无| 
| 修改用户手动触发类场景| 修改用户手动触发类场景 | 是| 无| 
| 修改用户定时执行类场景| 修改用户定时执行类场景 | 是| 无| 
| 规则类获取规则详情| 获取规则详情 | 是| 无| 
| 修改规则名称| 修改规则名称 | 是| 无| 
| 修改规则描述| 修改规则描述 | 是| 无| 
| 修改规则开关状态| 修改规则开关状态接 | 是| 无| 
| 修改行为开关状态| 修改行为开关状态 | 是| 无| 
| 修改消息推送行为中消息内容| 修改消息推送行为中消息内容 | 是| 无| 
| 修改条件描述| 修改条件描述 | 是| 无| 
| 修改动作描述| 修改动作描述 | 是| 无| 
| 查询支持的关系表达式| 查询支持的关系表达式 | 是| 无| 
| 获取场景最新操作日志| APP下拉刷新，获取家庭下场景的操作日志 | 是| 无| 
| 获取场景历史操作日志| 上拉加载，获取家庭下场景的操作日志 | 是| 无|  
| 通过来源(模板id)查询家庭下场景列表| 通过来源(模板id)查询家庭下场景列表（模板下载的场景）| 是| 无|  


#### 批量下载场景
>APP从场景Store中批量下载场景。</br>
注：同一个家庭同一个场景只能下载一次

##### 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/store/download`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|
| storeSceneIds| String[] |32| Body| 必填|需要下载的场景|  
| familyId     | String |32| Body| 必填|家庭Id|  


**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  |  String[] | Body  |  必填 | 下载后的新场景Id   |

##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 3.3.0
clientId: 123456
sequenceId: 20161020153428000015
accessToken: TGT1IIOXZJHLWOXF2FED4YZTGIQ3B0
sign: 3d0b88109d784e68e49651d91e2b95b79452d40b103e3bcc443d53b5feefd6d9
timestamp: 1542181278723 
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
	"data": ["4cb2a10f777b4bbd82b18c107550d1ba",
	"e9021da6f06c4a998f583c8dd322e36e"]
}
```

##### 3、错误码  
> 见 快速开始——常用信息——平台公共错误码 


#### 批量下载基础场景
>APP从场景Store中下载基础场景。</br>
注：同一个家庭可以多次下载同一个场景

##### 1、接口定义

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
|  data  |  String[] | Body  |  必填 | 下载后的新场景Id   |


##### 2、请求样例  

**用户请求**
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

##### 3、错误码  
> 见 快速开始——常用信息——平台公共错误码 

#### 批量下载应用场景
>批量下载应用场景。</br>
注：同一个家庭同一个场景允许下载多次

##### 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/store/downloadAppScene`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|
| storeSceneIds| String[] |32| Body| 必填|需要下载的场景|  
| familyId     | String |32| Body| 必填|家庭Id|  


**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  |  String[] | Body  |  必填 | 下载后的新场景Id   |

##### 2、请求样例  

**用户请求**
```java  
Header：
appId: DC330DD270F6
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

##### 3、错误码  
> 见 快速开始——常用信息——平台公共错误码 

#### 获取家庭下场景列表
> 根据家庭查询家庭下的场景列表。APP获取家庭下场景列表。


##### 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/listByFamily`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|
| familyId| String |32| Body| 必填|家庭Id|   
| limit| Int|N/A| Body| 必填|每页显示的记录数最多20条，大于20条，默认20条| 
| cursor| Int|N/A| Body| 必填|从0开始| 

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  |Pagination<SceneDto>| Body  |  必填 |显示场景中的描述信息,其中的规则rules中带有规则Id和规则名称以及规则描述,同时记录按照创建时间倒序，返回为null的字段被过滤掉，不显示。|

##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 3.3.0
clientId: 123456
sequenceId: 20161020153428000015
accessToken: TGT1IIOXZJHLWOXF2FED4YZTGIQ3B0
sign: 89998291f2d490e3339ed7f08247f85b8fc8185179ab0937b240c2aaceba9d76
timestamp: 1542184530374 
language: zh-cn
timezone: +8
appKey: f50c76fbc8271d361e1f6b5973f54585
Content-Encoding: utf-8
Content-type: application/json
Body
{
	"familyId": "385062139898000000",
	"limit": 10,
	"cursor": 0
}

```  

**请求应答**

```java
{
	"retCode": "00000",
	"retInfo": "成功",
	"data": {
		"pageSize": 10,
		"recordCount": 13,
		"currentPage": 1,
		"totalPage": 2,
		"cursor": 10,
		"list": [{
			"id": "98c2af1901b84eb1b4be99cedb53e96b",
			"userId": "100013957366169381",
			"familyId": "385062139898000000",
			"sceneName": "回家模式",
			"sceneDesc": "回家后可一键实现开灯、关窗帘、安防设备撤防、打开空调等。也可通过语音来实现上述功能。",
			"isOpen": 1,
			"rules": [{
				"id": "d8c4d6fad4854aac9c23e2aaaf6a9ebd",
				"rule": "点击即可执行"
			}],
			"createTime": "2018-09-12 14:58:46",
			"updateTime": "2018-09-12 14:58:46",
			"sourceId": "1c10d415fa3444dd9f900a53a70964cb",
			"type": "deviceFamily",
			"triggerType": "manually",
			"appId": "MB-****-0000",
			"taskInfo": {
				"type": 1,
				"cron": {
					"minutes": "12",
					"hours": "15",
					"day": "?",
					"month": "*",
					"week": "6,7",
					"year": "*",
					"cronExp": "* 12 15 ? * 6,7 *",
					"weekCronExp": "* * * ? * 6,7 *"
				},
				"status": true
			},
			"aiKeyword": "一键回家",
			"banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E5%9B%9E%E5%AE%B6-0815_20180815131530653.png",
			"icon": "http://resource.haier.net:80/download/resource/uzhsh/00001/one_key_20180314.png",
			"createType": "Basis",
			"status": "publish",
			"auto": false
		},
		{
			"id": "642fe4695da84edb8e70d3dd241ce952",
			"userId": "100013957366169381",
			"familyId": "385062139898000000",
			"sceneName": "厨房自动控温",
			"sceneDesc": "厨房温度超过设定值，空调启动制冷模式。",
			"isOpen": 0,
			"rules": [{
				"id": "62da131f1ea94f78918eabfa228e334d",
				"rule": "厨房空调关闭烟机打开，厨房温度过高"
			}],
			"createTime": "2018-09-12 15:23:00",
			"updateTime": "2018-09-17 17:56:10",
			"engineVersion": "V24",
			"sourceId": "78f95e4f59854dd1aea0b5d4dea281be",
			"type": "deviceFamily",
			"triggerType": "platform",
			"appId": "MB-****-0000",
			"taskInfo": {
				"type": 2,
				"cron": {
					"minutes": "*",
					"hours": "*",
					"day": "?",
					"month": "*",
					"week": "*",
					"year": "*",
					"cronExp": "* * * ? * * *",
					"weekCronExp": "* * * ? * * *"
				},
				"status": true,
				"activeBeginTime": "01:08:00",
				"activeEndTime": "02:10:00"
			},
			"banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E5%8E%A8%E6%88%BF%E6%8E%A7%E6%B8%A9-0815_20180815131421475.png",
			"icon": "http://resource.haier.net:80/download/resource/uzhsh/00001/one_key_20180314.png",
			"createType": "Basis",
			"status": "publish",
			"auto": false
		},
		{
			"id": "30d0ed93a24941d985ccfc39bbb47074",
			"userId": "100013957366169381",
			"familyId": "385062139898000000",
			"sceneName": "厨房自动控温",
			"sceneDesc": "厨房温度超过设定值，空调启动制冷模式。",
			"isOpen": 0,
			"rules": [{
				"id": "49b2151037944d8c98da9fb07afdcadb",
				"rule": "厨房空调关闭烟机打开，厨房温度过高"
			}],
			"createTime": "2018-09-03 16:05:44",
			"updateTime": "2018-09-07 13:04:55",
			"engineVersion": "V24",
			"sourceId": "78f95e4f59854dd1aea0b5d4dea281be",
			"type": "deviceFamily",
			"triggerType": "platform",
			"appId": "MB-****-0000",
			"taskInfo": {
				"type": 2,
				"cron": {
					"minutes": "*",
					"hours": "*",
					"day": "?",
					"month": "*",
					"week": "1,2,3,4,5",
					"year": "*",
					"cronExp": "* * * ? * 1,2,3,4,5 *",
					"weekCronExp": "* * * ? * 1,2,3,4,5 *"
				},
				"status": false,
				"activeBeginTime": "13:04:00",
				"activeEndTime": "13:04:00"
			},
			"banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E5%8E%A8%E6%88%BF%E6%8E%A7%E6%B8%A9-0815_20180815131421475.png",
			"icon": "http://resource.haier.net:80/download/resource/uzhsh/00001/one_key_20180314.png",
			"createType": "Basis",
			"status": "publish",
			"auto": false
		},
		{
			"id": "aed9381c28c84008b2dc73ac0e6ab9ec",
			"userId": "100013957366169381",
			"familyId": "385062139898000000",
			"sceneName": "空净净化",
			"sceneDesc": "当空气质量差时，开启空气净化器净化功能。",
			"isOpen": 1,
			"rules": [{
				"id": "dd1a4d8e14084ed5bbeaa81dc862f531",
				"rule": "空气净化器净化功能"
			}],
			"createTime": "2018-09-25 16:54:28",
			"updateTime": "2018-09-25 16:54:28",
			"engineVersion": "V24",
			"sourceId": "a6d833905bd64493ac35ec04be65ad3d",
			"type": "deviceFamily",
			"triggerType": "platform",
			"appId": "MB-****-0000",
			"banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E5%87%80-0815_20180815131434035.png",
			"icon": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E5%87%802_20180730134743357.png",
			"createType": "Basis",
			"status": "publish",
			"auto": false
		},
		{
			"id": "aa93a3a938834748a9060a177669b606",
			"userId": "100013957366169381",
			"familyId": "385062139898000000",
			"sceneName": "空净净化",
			"sceneDesc": "当空气质量差时，开启空气净化器净化功能。",
			"isOpen": 0,
			"rules": [{
				"id": "1d034095b1a747c6a683abf8aa552930",
				"rule": "空气净化器净化功能"
			}],
			"createTime": "2018-09-17 14:10:59",
			"updateTime": "2018-09-17 14:10:59",
			"engineVersion": "V24",
			"sourceId": "a6d833905bd64493ac35ec04be65ad3d",
			"type": "deviceFamily",
			"triggerType": "platform",
			"appId": "MB-UZHSH-0001",
			"createType": "Basis",
			"status": "publish",
			"auto": false
		},
		{
			"id": "935d7d4c0a5647b29461c631de3f0b0d",
			"userId": "100013957366169381",
			"familyId": "385062139898000000",
			"sceneName": "空净净化",
			"sceneDesc": "当空气质量差时，开启空气净化器净化功能。",
			"isOpen": 0,
			"rules": [{
				"id": "61e8fd04d8ac4aa7a0b1a81065288a16",
				"rule": "空气净化器净化功能"
			}],
			"createTime": "2018-09-15 15:04:34",
			"updateTime": "2018-09-15 15:04:34",
			"engineVersion": "V24",
			"sourceId": "a6d833905bd64493ac35ec04be65ad3d",
			"type": "deviceFamily",
			"triggerType": "platform",
			"appId": "MB-UZHSH-0001",
			"createType": "Basis",
			"status": "publish",
			"auto": false
		},
		{
			"id": "2bcbb7186a0440ee9aab1392e7018fe9",
			"userId": "100013957366169381",
			"familyId": "385062139898000000",
			"sceneName": "空净净化",
			"sceneDesc": "当空气质量差时，开启空气净化器净化功能。",
			"isOpen": 0,
			"rules": [{
				"id": "3ac637309eb14c41b777f2f85e9c80e2",
				"rule": "空气净化器净化功能"
			}],
			"createTime": "2018-09-14 09:21:27",
			"updateTime": "2018-09-14 09:21:27",
			"engineVersion": "V24",
			"sourceId": "a6d833905bd64493ac35ec04be65ad3d",
			"type": "deviceFamily",
			"triggerType": "platform",
			"appId": "MB-UZHSH-0001",
			"createType": "Basis",
			"status": "publish",
			"auto": false
		},
		{
			"id": "ac57eb2f1ea543dc9029e3bb4003e798",
			"userId": "100013957366168786",
			"familyId": "385062139898000000",
			"sceneName": "内测-满足所有条件（不同设备）",
			"sceneDesc": "空调开机时执行恒氧",
			"isOpen": 0,
			"rules": [{
				"id": "aec688187f6346edbdce48c43667f697",
				"rule": "空调挂机/柜机同时关机时新风机恒氧"
			}],
			"createTime": "2018-09-07 09:44:03",
			"updateTime": "2018-09-07 13:05:10",
			"engineVersion": "V24",
			"sourceId": "ddbad81f41a64fdcb7d787c3ebb12a10",
			"type": "deviceFamily",
			"triggerType": "platform",
			"appId": "MB-****-0000",
			"taskInfo": {
				"type": 2,
				"status": true,
				"activeBeginTime": "00:00:00",
				"activeEndTime": "13:59:00"
			},
			"banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E5%9B%9E%E5%AE%B6-0815_20180815131530653.png",
			"icon": "http://resource.haier.net:80/download/resource/uzhsh/00001/one_key_20180314.png",
			"createType": "Basis",
			"status": "publish",
			"auto": false
		},
		{
			"id": "484eca88bc6344f18ead3fb3e90f0471",
			"userId": "100013957366169635",
			"familyId": "385062139898000000",
			"sceneName": "恒温",
			"sceneDesc": "温度超标开启PMV",
			"isOpen": 0,
			"rules": [{
				"id": "330eccefe2b6434ea5fcea2821516df6",
				"rule": "挂机室内温度过低"
			},
			{
				"id": "56b04883b62249e2b94b7d50c1c45044",
				"rule": "柜机室内温度过低"
			},
			{
				"id": "82a7dcb10766450697aa032163123de4",
				"rule": "挂机室内温度过高"
			},
			{
				"id": "c150dfaa249b4781b825d695621f6e97",
				"rule": "中央空调温度过高"
			},
			{
				"id": "e5cc8ff3c8a946d198a78e0471f387a1",
				"rule": "中央空调温度过低"
			},
			{
				"id": "efc6e8636f05494f98ddb7f51a6eafbb",
				"rule": "柜机室内温度过高"
			}],
			"createTime": "2018-09-11 11:15:03",
			"updateTime": "2018-09-11 11:15:03",
			"engineVersion": "V24",
			"sourceId": "f4e402aa72244ce9a6dc165e64e28e17",
			"type": "deviceFamily",
			"triggerType": "platform",
			"appId": "MB-****-0000",
			"taskInfo": {
				"type": 2,
				"cron": {
					"minutes": "*",
					"hours": "*",
					"day": "?",
					"month": "*",
					"week": "*",
					"year": "*",
					"cronExp": "* * * ? * * *",
					"weekCronExp": "* * * ? * * *"
				},
				"status": false,
				"activeBeginTime": "18:09:00",
				"activeEndTime": "12:00:00"
			},
			"banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B8%A9-0815_20180815131504944.png",
			"icon": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B8%A92_20180730134901229.png",
			"createType": "Basis",
			"status": "publish",
			"auto": false
		},
		{
			"id": "77cd027bd6814b87a558778a5367a714",
			"userId": "100013957366169635",
			"familyId": "385062139898000000",
			"sceneName": "恒温",
			"sceneDesc": "温度超标开启PMV",
			"isOpen": 0,
			"rules": [{
				"id": "0907c3c943bb4649bb5265287b0b47db",
				"rule": "中央空调温度过低"
			},
			{
				"id": "2475174255934a67b5ce6ae6dc5f11a8",
				"rule": "中央空调温度过高"
			},
			{
				"id": "67f088f060e04cc18412dc700c207245",
				"rule": "挂机室内温度过高"
			},
			{
				"id": "84ffc0b731414e7aa000155b8250ded3",
				"rule": "柜机室内温度过低"
			},
			{
				"id": "ca95580592614382b37b0777a98be850",
				"rule": "挂机室内温度过低"
			},
			{
				"id": "e3c5580edd4c4bb295f049cdee3e9dfc",
				"rule": "柜机室内温度过高"
			}],
			"createTime": "2018-09-11 11:14:05",
			"updateTime": "2018-09-11 11:14:05",
			"engineVersion": "V24",
			"sourceId": "f4e402aa72244ce9a6dc165e64e28e17",
			"type": "deviceFamily",
			"triggerType": "platform",
			"appId": "MB-****-0000",
			"banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B8%A9-0815_20180815131504944.png",
			"icon": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B8%A92_20180730134901229.png",
			"createType": "Basis",
			"status": "publish",
			"auto": false
		}],
		"fromIndex": 0,
		"toIndex": 10
	}
}
```

##### 3、错误码  
> 见 快速开始——常用信息——平台公共错误码 




#### 查询家庭下场景列表
>获取家庭下场景列表（包括用户自建场景和通过模板下载的场景）。


##### 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/listSceneByFamily`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|
| familyId| String |32| Body| 必填|家庭Id|   
| limit| Int|N/A| Body| 必填|每页显示的记录数最多20条，大于20条，默认20条| 
| cursor| Int|N/A| Body| 必填|从0开始| 

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  |Pagination<SceneDto>| Body  |  必填 |显示场景中的描述信息,其中的规则rules中带有规则Id和规则名称以及规则描述,同时记录按照创建时间倒序|

##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 3.3.0
clientId: 123456
sequenceId: 20161020153428000015
accessToken: TGT1IIOXZJHLWOXF2FED4YZTGIQ3B0
sign: 89998291f2d490e3339ed7f08247f85b8fc8185179ab0937b240c2aaceba9d76
timestamp: 1542184530374 
language: zh-cn
timezone: +8
appKey: f50c76fbc8271d361e1f6b5973f54585
Content-Encoding: utf-8
Content-type: application/json
Body
{
	"familyId": "385062139898000000",
	"limit": 10,
	"cursor": 0
}

```  

**请求应答**

```java
{
	"retCode": "00000",
	"retInfo": "成功",
	"data": {
		"pageSize": 10,
		"recordCount": 13,
		"currentPage": 1,
		"totalPage": 2,
		"cursor": 10,
		"list": [{
			"id": "98c2af1901b84eb1b4be99cedb53e96b",
			"userId": "100013957366169381",
			"familyId": "385062139898000000",
			"sceneName": "回家模式",
			"sceneDesc": "回家后可一键实现开灯、关窗帘、安防设备撤防、打开空调等。也可通过语音来实现上述功能。",
			"isOpen": 1,
			"rules": [{
				"id": "d8c4d6fad4854aac9c23e2aaaf6a9ebd",
				"rule": "点击即可执行"
			}],
			"createTime": "2018-09-12 14:58:46",
			"updateTime": "2018-09-12 14:58:46",
			"sourceId": "1c10d415fa3444dd9f900a53a70964cb",
			"type": "deviceFamily",
			"triggerType": "manually",
			"appId": "MB-****-0000",
			"taskInfo": {
				"type": 1,
				"cron": {
					"minutes": "12",
					"hours": "15",
					"day": "?",
					"month": "*",
					"week": "6,7",
					"year": "*",
					"cronExp": "* 12 15 ? * 6,7 *",
					"weekCronExp": "* * * ? * 6,7 *"
				},
				"status": true
			},
			"aiKeyword": "一键回家",
			"banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E5%9B%9E%E5%AE%B6-0815_20180815131530653.png",
			"icon": "http://resource.haier.net:80/download/resource/uzhsh/00001/one_key_20180314.png",
			"createType": "Basis",
			"status": "publish",
			"auto": false
		},
		{
			"id": "642fe4695da84edb8e70d3dd241ce952",
			"userId": "100013957366169381",
			"familyId": "385062139898000000",
			"sceneName": "厨房自动控温",
			"sceneDesc": "厨房温度超过设定值，空调启动制冷模式。",
			"isOpen": 0,
			"rules": [{
				"id": "62da131f1ea94f78918eabfa228e334d",
				"rule": "厨房空调关闭烟机打开，厨房温度过高"
			}],
			"createTime": "2018-09-12 15:23:00",
			"updateTime": "2018-09-17 17:56:10",
			"engineVersion": "V24",
			"sourceId": "78f95e4f59854dd1aea0b5d4dea281be",
			"type": "deviceFamily",
			"triggerType": "platform",
			"appId": "MB-****-0000",
			"taskInfo": {
				"type": 2,
				"cron": {
					"minutes": "*",
					"hours": "*",
					"day": "?",
					"month": "*",
					"week": "*",
					"year": "*",
					"cronExp": "* * * ? * * *",
					"weekCronExp": "* * * ? * * *"
				},
				"status": true,
				"activeBeginTime": "01:08:00",
				"activeEndTime": "02:10:00"
			},
			"banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E5%8E%A8%E6%88%BF%E6%8E%A7%E6%B8%A9-0815_20180815131421475.png",
			"icon": "http://resource.haier.net:80/download/resource/uzhsh/00001/one_key_20180314.png",
			"createType": "Basis",
			"status": "publish",
			"auto": false
		},
		{
			"id": "30d0ed93a24941d985ccfc39bbb47074",
			"userId": "100013957366169381",
			"familyId": "385062139898000000",
			"sceneName": "厨房自动控温",
			"sceneDesc": "厨房温度超过设定值，空调启动制冷模式。",
			"isOpen": 0,
			"rules": [{
				"id": "49b2151037944d8c98da9fb07afdcadb",
				"rule": "厨房空调关闭烟机打开，厨房温度过高"
			}],
			"createTime": "2018-09-03 16:05:44",
			"updateTime": "2018-09-07 13:04:55",
			"engineVersion": "V24",
			"sourceId": "78f95e4f59854dd1aea0b5d4dea281be",
			"type": "deviceFamily",
			"triggerType": "platform",
			"appId": "MB-****-0000",
			"taskInfo": {
				"type": 2,
				"cron": {
					"minutes": "*",
					"hours": "*",
					"day": "?",
					"month": "*",
					"week": "1,2,3,4,5",
					"year": "*",
					"cronExp": "* * * ? * 1,2,3,4,5 *",
					"weekCronExp": "* * * ? * 1,2,3,4,5 *"
				},
				"status": false,
				"activeBeginTime": "13:04:00",
				"activeEndTime": "13:04:00"
			},
			"banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E5%8E%A8%E6%88%BF%E6%8E%A7%E6%B8%A9-0815_20180815131421475.png",
			"icon": "http://resource.haier.net:80/download/resource/uzhsh/00001/one_key_20180314.png",
			"createType": "Basis",
			"status": "publish",
			"auto": false
		},
		{
			"id": "aed9381c28c84008b2dc73ac0e6ab9ec",
			"userId": "100013957366169381",
			"familyId": "385062139898000000",
			"sceneName": "空净净化",
			"sceneDesc": "当空气质量差时，开启空气净化器净化功能。",
			"isOpen": 1,
			"rules": [{
				"id": "dd1a4d8e14084ed5bbeaa81dc862f531",
				"rule": "空气净化器净化功能"
			}],
			"createTime": "2018-09-25 16:54:28",
			"updateTime": "2018-09-25 16:54:28",
			"engineVersion": "V24",
			"sourceId": "a6d833905bd64493ac35ec04be65ad3d",
			"type": "deviceFamily",
			"triggerType": "platform",
			"appId": "MB-****-0000",
			"banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E5%87%80-0815_20180815131434035.png",
			"icon": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E5%87%802_20180730134743357.png",
			"createType": "Basis",
			"status": "publish",
			"auto": false
		},
		{
			"id": "aa93a3a938834748a9060a177669b606",
			"userId": "100013957366169381",
			"familyId": "385062139898000000",
			"sceneName": "空净净化",
			"sceneDesc": "当空气质量差时，开启空气净化器净化功能。",
			"isOpen": 0,
			"rules": [{
				"id": "1d034095b1a747c6a683abf8aa552930",
				"rule": "空气净化器净化功能"
			}],
			"createTime": "2018-09-17 14:10:59",
			"updateTime": "2018-09-17 14:10:59",
			"engineVersion": "V24",
			"sourceId": "a6d833905bd64493ac35ec04be65ad3d",
			"type": "deviceFamily",
			"triggerType": "platform",
			"appId": "MB-UZHSH-0001",
			"createType": "Basis",
			"status": "publish",
			"auto": false
		},
		{
			"id": "935d7d4c0a5647b29461c631de3f0b0d",
			"userId": "100013957366169381",
			"familyId": "385062139898000000",
			"sceneName": "空净净化",
			"sceneDesc": "当空气质量差时，开启空气净化器净化功能。",
			"isOpen": 0,
			"rules": [{
				"id": "61e8fd04d8ac4aa7a0b1a81065288a16",
				"rule": "空气净化器净化功能"
			}],
			"createTime": "2018-09-15 15:04:34",
			"updateTime": "2018-09-15 15:04:34",
			"engineVersion": "V24",
			"sourceId": "a6d833905bd64493ac35ec04be65ad3d",
			"type": "deviceFamily",
			"triggerType": "platform",
			"appId": "MB-UZHSH-0001",
			"createType": "Basis",
			"status": "publish",
			"auto": false
		},
		{
			"id": "2bcbb7186a0440ee9aab1392e7018fe9",
			"userId": "100013957366169381",
			"familyId": "385062139898000000",
			"sceneName": "空净净化",
			"sceneDesc": "当空气质量差时，开启空气净化器净化功能。",
			"isOpen": 0,
			"rules": [{
				"id": "3ac637309eb14c41b777f2f85e9c80e2",
				"rule": "空气净化器净化功能"
			}],
			"createTime": "2018-09-14 09:21:27",
			"updateTime": "2018-09-14 09:21:27",
			"engineVersion": "V24",
			"sourceId": "a6d833905bd64493ac35ec04be65ad3d",
			"type": "deviceFamily",
			"triggerType": "platform",
			"appId": "MB-UZHSH-0001",
			"createType": "Basis",
			"status": "publish",
			"auto": false
		},
		{
			"id": "ac57eb2f1ea543dc9029e3bb4003e798",
			"userId": "100013957366168786",
			"familyId": "385062139898000000",
			"sceneName": "内测-满足所有条件（不同设备）",
			"sceneDesc": "空调开机时执行恒氧",
			"isOpen": 0,
			"rules": [{
				"id": "aec688187f6346edbdce48c43667f697",
				"rule": "空调挂机/柜机同时关机时新风机恒氧"
			}],
			"createTime": "2018-09-07 09:44:03",
			"updateTime": "2018-09-07 13:05:10",
			"engineVersion": "V24",
			"sourceId": "ddbad81f41a64fdcb7d787c3ebb12a10",
			"type": "deviceFamily",
			"triggerType": "platform",
			"appId": "MB-****-0000",
			"taskInfo": {
				"type": 2,
				"status": true,
				"activeBeginTime": "00:00:00",
				"activeEndTime": "13:59:00"
			},
			"banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E5%9B%9E%E5%AE%B6-0815_20180815131530653.png",
			"icon": "http://resource.haier.net:80/download/resource/uzhsh/00001/one_key_20180314.png",
			"createType": "Basis",
			"status": "publish",
			"auto": false
		},
		{
			"id": "484eca88bc6344f18ead3fb3e90f0471",
			"userId": "100013957366169635",
			"familyId": "385062139898000000",
			"sceneName": "恒温",
			"sceneDesc": "温度超标开启PMV",
			"isOpen": 0,
			"rules": [{
				"id": "330eccefe2b6434ea5fcea2821516df6",
				"rule": "挂机室内温度过低"
			},
			{
				"id": "56b04883b62249e2b94b7d50c1c45044",
				"rule": "柜机室内温度过低"
			},
			{
				"id": "82a7dcb10766450697aa032163123de4",
				"rule": "挂机室内温度过高"
			},
			{
				"id": "c150dfaa249b4781b825d695621f6e97",
				"rule": "中央空调温度过高"
			},
			{
				"id": "e5cc8ff3c8a946d198a78e0471f387a1",
				"rule": "中央空调温度过低"
			},
			{
				"id": "efc6e8636f05494f98ddb7f51a6eafbb",
				"rule": "柜机室内温度过高"
			}],
			"createTime": "2018-09-11 11:15:03",
			"updateTime": "2018-09-11 11:15:03",
			"engineVersion": "V24",
			"sourceId": "f4e402aa72244ce9a6dc165e64e28e17",
			"type": "deviceFamily",
			"triggerType": "platform",
			"appId": "MB-****-0000",
			"taskInfo": {
				"type": 2,
				"cron": {
					"minutes": "*",
					"hours": "*",
					"day": "?",
					"month": "*",
					"week": "*",
					"year": "*",
					"cronExp": "* * * ? * * *",
					"weekCronExp": "* * * ? * * *"
				},
				"status": false,
				"activeBeginTime": "18:09:00",
				"activeEndTime": "12:00:00"
			},
			"banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B8%A9-0815_20180815131504944.png",
			"icon": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B8%A92_20180730134901229.png",
			"createType": "Basis",
			"status": "publish",
			"auto": false
		},
		{
			"id": "77cd027bd6814b87a558778a5367a714",
			"userId": "100013957366169635",
			"familyId": "385062139898000000",
			"sceneName": "恒温",
			"sceneDesc": "温度超标开启PMV",
			"isOpen": 0,
			"rules": [{
				"id": "0907c3c943bb4649bb5265287b0b47db",
				"rule": "中央空调温度过低"
			},
			{
				"id": "2475174255934a67b5ce6ae6dc5f11a8",
				"rule": "中央空调温度过高"
			},
			{
				"id": "67f088f060e04cc18412dc700c207245",
				"rule": "挂机室内温度过高"
			},
			{
				"id": "84ffc0b731414e7aa000155b8250ded3",
				"rule": "柜机室内温度过低"
			},
			{
				"id": "ca95580592614382b37b0777a98be850",
				"rule": "挂机室内温度过低"
			},
			{
				"id": "e3c5580edd4c4bb295f049cdee3e9dfc",
				"rule": "柜机室内温度过高"
			}],
			"createTime": "2018-09-11 11:14:05",
			"updateTime": "2018-09-11 11:14:05",
			"engineVersion": "V24",
			"sourceId": "f4e402aa72244ce9a6dc165e64e28e17",
			"type": "deviceFamily",
			"triggerType": "platform",
			"appId": "MB-****-0000",
			"banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B8%A9-0815_20180815131504944.png",
			"icon": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B8%A92_20180730134901229.png",
			"createType": "Basis",
			"status": "publish",
			"auto": false
		}],
		"fromIndex": 0,
		"toIndex": 10
	}
}
```

##### 3、错误码  
> 见 快速开始——常用信息——平台公共错误码 

#### 获取家庭下应用场景列表
>获取家庭下应用场景列表。


##### 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/listAppSceneByFamily`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|
| familyId| String |32| Body| 必填|家庭Id|  
| limit| Int|N/A| Body| 选填|每页显示的记录数|   
| cursor| Int|N/A| Body| 必填|从0开始| 

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  |  Pagination<UserSceneDto>| Body  |  必填 |显示场景中的描述信息, 其中的规则rules中带有规则Id和规则名称以及规则描述, 同时记录按照创建时间倒叙   |

##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 3.3.0
clientId: 123456
sequenceId: 20161020153428000015
accessToken: TGT1IIOXZJHLWOXF2FED4YZTGIQ3B0
sign: f8f6c11f90f5a6322a4e9a8386f46f1d2dc41b74687b2993b6aaef52686b084b
timestamp: 1542184780515 
language: zh-cn
timezone: +8
appKey: f50c76fbc8271d361e1f6b5973f54585
Content-Encoding: utf-8
Content-type: application/json
Body
{
	"familyId": "461066634104000000",
	"limit": "100",
	"cursor": "0"
}

```  

**请求应答**

```java
{
	"retCode": "00000",
	"retInfo": "成功",
	"data": {
		"pageSize": 100,
		"recordCount": 57,
		"currentPage": 1,
		"totalPage": 1,
		"cursor": 100,
		"list": [{
			"id": "41b106d0a1324569837810a94d6ce5a9",
			"familyId": "461066634104000000",
			"sceneName": "延时状态",
			"sceneDesc": "延时状态",
			"isOpen": 0,
			"rules": [{
				"id": "5ed610957a7a4d01a202b6ca4ff6b792",
				"rule": "延时状态1"
			}],
			"createTime": "2018-10-26 14:13:13",
			"updateTime": "2018-10-26 15:42:21",
			"status": "publish",
			"auto": false,
			"canAppTrigger": true,
			"appId": "MB-****-0000"
		},
		{
			"id": "a1db6d649bb9485aa94da38a1d00485a",
			"familyId": "461066634104000000",
			"sceneName": "灯开有档位",
			"sceneDesc": "灯开有档位",
			"isOpen": 1,
			"rules": [{
				"id": "e3e30473070f49bea06e434ed463193d",
				"rule": "灯开有档位1"
			}],
			"createTime": "2018-10-26 10:36:27",
			"updateTime": "2018-10-26 10:36:27",
			"status": "publish",
			"auto": false,
			"canAppTrigger": false,
			"appId": "MB-****-0000"
		},
		{
			"id": "17b205eb69db4d8db50abdd50f972101",
			"familyId": "461066634104000000",
			"sceneName": "照明开烟机开",
			"sceneDesc": "照明开烟机开",
			"isOpen": 1,
			"rules": [{
				"id": "60a9b132b5a4433483d90e22d96665de",
				"rule": "照明开烟机开1"
			}],
			"createTime": "2018-10-26 10:19:08",
			"updateTime": "2018-10-26 10:19:08",
			"status": "publish",
			"auto": false,
			"canAppTrigger": false,
			"appId": "MB-****-0000"
		},
		{
			"id": "3168bb73f39a4c748916afc95b55c817",
			"familyId": "461066634104000000",
			"sceneName": "烟机开机照明状态开",
			"sceneDesc": "烟机开机照明状态开",
			"isOpen": 1,
			"rules": [{
				"id": "fbe30f3d6d8b486fbc48feb53ed51d2a",
				"rule": "烟机开机照明状态开1"
			}],
			"createTime": "2018-10-26 09:24:18",
			"updateTime": "2018-10-26 09:24:18",
			"status": "publish",
			"auto": false,
			"canAppTrigger": false,
			"appId": "MB-****-0000"
		},
		{
			"id": "d1113194dd4b416bb6af650631594cd2",
			"familyId": "461066634104000000",
			"sceneName": "烟机开机照明开",
			"sceneDesc": "烟机开机照明开",
			"isOpen": 1,
			"rules": [{
				"id": "67060fcfaf204670b80c3bfcb3ec0733",
				"rule": "烟机开机照明开1"
			}],
			"createTime": "2018-10-25 18:33:59",
			"updateTime": "2018-10-25 18:33:59",
			"status": "publish",
			"auto": false,
			"canAppTrigger": false,
			"appId": "MB-****-0000"
		},
		{
			"id": "f00def1e625049c5bdfc244a96d3e307",
			"familyId": "461066634104000000",
			"sceneName": "灯开柔风",
			"sceneDesc": "灯开柔风",
			"isOpen": 1,
			"rules": [{
				"id": "350debcf7e2a4f34a1a192ab9a9925f3",
				"rule": "灯开柔风1"
			}],
			"createTime": "2018-10-25 18:28:36",
			"updateTime": "2018-10-25 18:28:36",
			"status": "publish",
			"auto": false,
			"canAppTrigger": false,
			"appId": "MB-****-0000"
		},
		{
			"id": "a0740fdcb08f4720bdc901201feefd50",
			"familyId": "461066634104000000",
			"sceneName": "testq",
			"sceneDesc": "testq",
			"isOpen": 1,
			"rules": [{
				"id": "9c4e8a30870b42d09af906d4f9138cad",
				"rule": "testq1"
			}],
			"createTime": "2018-10-25 16:28:49",
			"updateTime": "2018-10-25 16:28:49",
			"status": "publish",
			"auto": false,
			"weight": 0,
			"canAppTrigger": false,
			"appId": "MB-UZHSH-0001"
		},
		{
			"id": "ce3e2a20dabd4316863d0ed0a042f201",
			"familyId": "461066634104000000",
			"sceneName": "test",
			"sceneDesc": "test",
			"isOpen": 1,
			"rules": [{
				"id": "d763cc4325854b99be8a7d603daec7d4",
				"rule": "test1"
			}],
			"createTime": "2018-10-25 16:13:57",
			"updateTime": "2018-10-25 16:13:57",
			"status": "publish",
			"auto": false,
			"weight": 0,
			"canAppTrigger": true,
			"appId": "MB-UZHSH-0001"
		},
		{
			"id": "3ed5e38bbf8041e6bc9668189e6b37dd",
			"familyId": "461066634104000000",
			"sceneName": "油烟机",
			"sceneDesc": "油烟机",
			"isOpen": 1,
			"rules": [{
				"id": "f2e52c0f33b74be1abb0a5ed1aee7c59",
				"rule": "油烟机1"
			}],
			"createTime": "2018-10-25 13:40:30",
			"updateTime": "2018-10-25 13:40:30",
			"status": "publish",
			"auto": false,
			"weight": 0,
			"canAppTrigger": true,
			"appId": "MB-UZHSH-0001"
		},
		{
			"id": "4787f577ddc44f04b026e4a31ce68106",
			"familyId": "461066634104000000",
			"sceneName": "47851",
			"sceneDesc": "47851",
			"isOpen": 1,
			"rules": [{
				"id": "a64c6e0a6b5b4c43bec531b6f3bc108f",
				"rule": "478511"
			}],
			"createTime": "2018-10-25 11:43:07",
			"updateTime": "2018-10-25 11:43:07",
			"status": "publish",
			"auto": false,
			"weight": 0,
			"canAppTrigger": true,
			"appId": "MB-UZHSH-0001"
		},
		{
			"id": "f3e20977e7294a8fbe3badc6a9a7389a",
			"familyId": "461066634104000000",
			"sceneName": "除湿",
			"sceneDesc": "除湿",
			"isOpen": 1,
			"rules": [{
				"id": "f1789d3edd224adc962079b550ad95f5",
				"rule": "除湿1"
			}],
			"createTime": "2018-10-24 16:28:27",
			"updateTime": "2018-10-24 16:34:31",
			"status": "publish",
			"auto": false,
			"weight": 0,
			"canAppTrigger": true,
			"appId": "MB-UZHSH-0001"
		},
		{
			"id": "ba2af47b17f8443ab05965f7caad1e78",
			"familyId": "461066634104000000",
			"sceneName": "柔风关机",
			"sceneDesc": "柔风关机",
			"isOpen": 1,
			"rules": [{
				"id": "c5473939f13e42b2938ce0e61a8cc551",
				"rule": "柔风关机1"
			}],
			"createTime": "2018-10-24 13:36:03",
			"updateTime": "2018-10-24 13:36:03",
			"status": "publish",
			"auto": false,
			"canAppTrigger": false,
			"appId": "MB-****-0000"
		},
		{
			"id": "f794cdb0dffd42fe83ad231dab061009",
			"familyId": "461066634104000000",
			"sceneName": "1111",
			"sceneDesc": "1111",
			"isOpen": 1,
			"rules": [{
				"id": "54e2da5b526a4c95a19c6eb2c241c2d0",
				"rule": "11111"
			}],
			"createTime": "2018-10-24 10:37:35",
			"updateTime": "2018-10-24 10:37:35",
			"status": "publish",
			"auto": false,
			"canAppTrigger": true,
			"appId": "MB-****-0000"
		},
		{
			"id": "9700d05885e34cb19542d5f0c7abf60d",
			"familyId": "461066634104000000",
			"sceneName": "柔风设置延时",
			"sceneDesc": "柔风设置延时",
			"isOpen": 1,
			"rules": [{
				"id": "8b0613e30429410d8e03c9c1486eb11d",
				"rule": "柔风设置延时1"
			}],
			"createTime": "2018-10-23 17:15:42",
			"updateTime": "2018-10-23 17:15:42",
			"status": "publish",
			"auto": false,
			"canAppTrigger": false,
			"appId": "MB-****-0000"
		},
		{
			"id": "79c0c611afd84cb991c345d197b984e0",
			"familyId": "461066634104000000",
			"sceneName": "灯开烟机开",
			"sceneDesc": "灯开烟机开",
			"isOpen": 1,
			"rules": [{
				"id": "20cf1471600240c5856d663b1e40db1f",
				"rule": "灯开烟机开1"
			}],
			"createTime": "2018-10-23 16:54:27",
			"updateTime": "2018-10-23 16:54:27",
			"status": "publish",
			"auto": false,
			"canAppTrigger": false,
			"appId": "MB-****-0000"
		},
		{
			"id": "c9fbaa76d4d04a72944e8517782f2087",
			"familyId": "461066634104000000",
			"sceneName": "开机照明开",
			"sceneDesc": "开机照明开",
			"isOpen": 1,
			"rules": [{
				"id": "dcc0d801083b4f798784bac5c3f1bb9c",
				"rule": "开机照明开1"
			}],
			"createTime": "2018-10-23 16:46:54",
			"updateTime": "2018-10-23 16:46:54",
			"status": "publish",
			"auto": false,
			"canAppTrigger": false,
			"appId": "MB-****-0000"
		},
		{
			"id": "e5dd31d021274b1b8fcea3835a322057",
			"familyId": "461066634104000000",
			"sceneName": "低速",
			"sceneDesc": "低速",
			"isOpen": 1,
			"rules": [{
				"id": "8649a48d26184933b9c92c81f16a7185",
				"rule": "低速1"
			}],
			"createTime": "2018-10-23 16:24:27",
			"updateTime": "2018-10-23 16:24:27",
			"status": "publish",
			"auto": false,
			"canAppTrigger": true,
			"appId": "MB-****-0000"
		},
		{
			"id": "acb3711fef65490782a5de36225c9bbd",
			"familyId": "461066634104000000",
			"sceneName": "关",
			"sceneDesc": "关",
			"isOpen": 1,
			"rules": [{
				"id": "59f4358cb8c94bfd819c1271e25ae74e",
				"rule": "关1"
			}],
			"createTime": "2018-10-23 16:23:45",
			"updateTime": "2018-10-23 16:23:45",
			"status": "publish",
			"auto": false,
			"canAppTrigger": true,
			"appId": "MB-****-0000"
		},
		{
			"id": "e5c1654900f84b73b22eba577ed2de6d",
			"familyId": "461066634104000000",
			"sceneName": "开机",
			"sceneDesc": "开机",
			"isOpen": 1,
			"rules": [{
				"id": "56d7ed29d53a4fbab8952db4b4aec63b",
				"rule": "开机1"
			}],
			"createTime": "2018-10-23 16:21:42",
			"updateTime": "2018-10-23 16:21:42",
			"status": "publish",
			"auto": false,
			"canAppTrigger": true,
			"appId": "MB-****-0000"
		},
		{
			"id": "47e20f6f5a8149e09e9cbe7dd8a5bc82",
			"familyId": "461066634104000000",
			"sceneName": "除湿剂测试测试",
			"sceneDesc": "除湿剂测试测试",
			"isOpen": 1,
			"rules": [{
				"id": "4c6d55919cc3430fa23fae98f51aa7a8",
				"rule": "除湿剂测试测试1"
			}],
			"createTime": "2018-10-18 17:17:39",
			"updateTime": "2018-10-18 17:17:39",
			"status": "publish",
			"auto": false,
			"weight": 0,
			"canAppTrigger": false,
			"appId": "MB-UZHSH-0001"
		},
		{
			"id": "b99807724ed7427f95500545628fbcd2",
			"familyId": "461066634104000000",
			"sceneName": "测试",
			"sceneDesc": "测试",
			"isOpen": 1,
			"rules": [{
				"id": "cc215e5abca946f1aadb38a10044a1bc",
				"rule": "测试1"
			}],
			"createTime": "2018-09-18 11:49:47",
			"updateTime": "2018-09-20 18:33:04",
			"status": "publish",
			"auto": false,
			"weight": 0,
			"canAppTrigger": false,
			"activeBeginTime": "16:03:00",
			"activeEndTime": "08:59:00",
			"appId": "MB-UZHSH-0001"
		},
		{
			"id": "a8f50f005de24ca5a385208fcd47cef1",
			"familyId": "461066634104000000",
			"sceneName": "开关类场景",
			"sceneDesc": "开关类场景",
			"isOpen": 1,
			"rules": [{
				"id": "633c21e4c6144c2484ffb2e989ab669a",
				"rule": "开关类场景1"
			}],
			"createTime": "2018-09-18 11:09:25",
			"updateTime": "2018-09-21 11:44:41",
			"appSceneAlias": "开关类场景h h h",
			"status": "publish",
			"auto": false,
			"weight": 0,
			"canAppTrigger": false,
			"activeBeginTime": "10:00:00",
			"activeEndTime": "17:00:00",
			"appId": "MB-UZHSH-0001"
		},
		{
			"id": "9074282c29aa4f528a5df4559aead352",
			"familyId": "461066634104000000",
			"sceneName": "一键就寝",
			"sceneDesc": "一键就寝，执行就寝模式下设备",
			"isOpen": 1,
			"rules": [{
				"id": "35faffb0d04148fb84043dad224741f8",
				"rule": "关闭卧室灯"
			}],
			"createTime": "2018-09-21 13:49:48",
			"updateTime": "2018-09-21 13:49:48",
			"sourceId": "0687dd83da4d405b8be80b7b439461d6",
			"type": "deviceFamily",
			"status": "publish",
			"auto": false,
			"keyword": "一键睡眠",
			"canAppTrigger": true,
			"banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%9C%AA%E6%A0%87%E9%A2%98-3_20180720183246519.jpg",
			"icon": "http://resource.haier.net:80/download/resource/uzhsh/00001/one_key_20180314.png",
			"appId": "MB-UZHSH-0001",
			"sortList": [{
				"id": 5,
				"name": "家庭类场景"
			}],
			"tagList": [{
				"id": 4,
				"name": "一键就寝"
			}]
		},
		{
			"id": "0844aa525aab4156960c6e6b381499ea",
			"familyId": "461066634104000000",
			"sceneName": "一键就寝",
			"sceneDesc": "一键就寝，执行就寝模式下设备",
			"isOpen": 1,
			"rules": [{
				"id": "20baf8ce8d3945fea49b84acf1bdbe9f",
				"rule": "关闭卧室灯"
			}],
			"createTime": "2018-09-20 18:30:37",
			"updateTime": "2018-09-20 18:30:37",
			"sourceId": "0687dd83da4d405b8be80b7b439461d6",
			"type": "deviceFamily",
			"status": "publish",
			"auto": false,
			"keyword": "一键睡眠",
			"canAppTrigger": true,
			"banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%9C%AA%E6%A0%87%E9%A2%98-3_20180720183246519.jpg",
			"icon": "http://resource.haier.net:80/download/resource/uzhsh/00001/one_key_20180314.png",
			"appId": "MB-UZHSH-0001",
			"sortList": [{
				"id": 5,
				"name": "家庭类场景"
			}],
			"tagList": [{
				"id": 4,
				"name": "一键就寝"
			}]
		},
		{
			"id": "b06bdda15deb4ee3825a9490124ddc87",
			"familyId": "461066634104000000",
			"sceneName": "一键离家（旧版）",
			"sceneDesc": "IFTTT1.0_场景38_一键离家 ",
			"isOpen": 1,
			"rules": [{
				"id": "ed6fd4aca4e942d4b653b8be1ef4a5e7",
				"rule": "IFTTT1.0_场景38_一键离家"
			}],
			"createTime": "2018-10-18 17:07:30",
			"updateTime": "2018-10-18 17:07:30",
			"sourceId": "22f86c65748d4a4aa362627cffae82a0",
			"type": "deviceFamily",
			"status": "publish",
			"auto": false,
			"canAppTrigger": true,
			"banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E7%A6%BB%E5%AE%B6-0815_20180815131404444.png",
			"icon": "http://resource.haier.net:80/download/resource/uzhsh/00001/one_key_20180314.png",
			"appId": "MB-UZHSH-0001"
		},
		{
			"id": "def3ff49cb3c4f7290c436c2afad209a",
			"familyId": "461066634104000000",
			"sceneName": "就寝模式",
			"sceneDesc": "就寝时可一键实现关灯、关窗帘、安防设备布防等。也可通过语音来实现上述功能。",
			"isOpen": 1,
			"rules": [{
				"id": "cbf44af909824d74b149bbaa199efb9f",
				"rule": "点击即可执行"
			}],
			"createTime": "2018-10-16 11:27:24",
			"updateTime": "2018-10-16 11:27:24",
			"sourceId": "43afa5489493447cb00d69a41fa77b65",
			"type": "deviceFamily",
			"status": "publish",
			"auto": false,
			"keyword": "一键睡眠",
			"canAppTrigger": true,
			"banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E5%B0%B1%E5%AF%9D-0815_20180815131543427.png",
			"icon": "http://resource.haier.net:80/download/resource/uzhsh/00001/one_key_20180314.png",
			"appId": "MB-****-0000"
		},
		{
			"id": "343938fba00445308cd2e7125df8b530",
			"familyId": "461066634104000000",
			"sceneName": "就寝模式",
			"sceneDesc": "就寝时可一键实现关灯、关窗帘、安防设备布防等。也可通过语音来实现上述功能。",
			"isOpen": 1,
			"rules": [{
				"id": "fb9e3fcb135e40a0a09bc84b64a0c25d",
				"rule": "点击即可执行"
			}],
			"createTime": "2018-09-19 17:01:50",
			"updateTime": "2018-09-19 17:02:02",
			"sourceId": "43afa5489493447cb00d69a41fa77b65",
			"appSceneAlias": "就寝模式11",
			"type": "deviceFamily",
			"status": "publish",
			"auto": false,
			"keyword": "一键睡眠",
			"canAppTrigger": true,
			"banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E5%B0%B1%E5%AF%9D-0815_20180815131543427.png",
			"icon": "http://resource.haier.net:80/download/resource/uzhsh/00001/one_key_20180314.png",
			"appId": "MB-UZHSH-0001"
		},
		{
			"id": "d7c4084c4be64543bef66fb981d66b5e",
			"familyId": "461066634104000000",
			"sceneName": "智慧除湿",
			"sceneDesc": "当湿度过高时，开启除湿机除湿",
			"isOpen": 1,
			"rules": [{
				"id": "5c68b4bc9d2a413c895d46853c799505",
				"rule": "除湿机自动除湿"
			}],
			"createTime": "2018-10-25 14:10:19",
			"updateTime": "2018-10-25 14:10:19",
			"sourceId": "51f01c4fff194b838fac465d43c0acc2",
			"type": "deviceFamily",
			"status": "publish",
			"auto": false,
			"canAppTrigger": false,
			"banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B9%BF-0815_20180815131452956.png",
			"icon": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B9%BF2_20180730134826251.png",
			"appId": "MB-****-0000"
		},
		{
			"id": "44371623ef5f4b67b79875d000e88aed",
			"familyId": "461066634104000000",
			"sceneName": "智慧除湿",
			"sceneDesc": "当湿度过高时，开启除湿机除湿",
			"isOpen": 1,
			"rules": [{
				"id": "649a0b4b4ed8412898251fcf1d9f16d6",
				"rule": "除湿机自动除湿"
			}],
			"createTime": "2018-10-25 13:35:15",
			"updateTime": "2018-10-25 13:35:31",
			"sourceId": "51f01c4fff194b838fac465d43c0acc2",
			"appSceneAlias": "智慧除湿111",
			"type": "deviceFamily",
			"status": "publish",
			"auto": false,
			"canAppTrigger": false,
			"banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B9%BF-0815_20180815131452956.png",
			"icon": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B9%BF2_20180730134826251.png",
			"appId": "MB-UZHSH-0001"
		},
		{
			"id": "f0aae2ac497545eaaad1a3b5d293c6bf",
			"familyId": "461066634104000000",
			"sceneName": "智慧除湿",
			"sceneDesc": "当湿度过高时，开启除湿机除湿",
			"isOpen": 1,
			"rules": [{
				"id": "c2293f3fce1a4c46946efec15a236952",
				"rule": "除湿机自动除湿"
			}],
			"createTime": "2018-10-25 13:35:10",
			"updateTime": "2018-10-25 13:35:39",
			"sourceId": "51f01c4fff194b838fac465d43c0acc2",
			"appSceneAlias": "智慧除湿222",
			"type": "deviceFamily",
			"status": "publish",
			"auto": false,
			"canAppTrigger": false,
			"banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B9%BF-0815_20180815131452956.png",
			"icon": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B9%BF2_20180730134826251.png",
			"appId": "MB-UZHSH-0001"
		},
		{
			"id": "ff2320da01bc4f338b9afaa582167b1b",
			"familyId": "461066634104000000",
			"sceneName": "智慧除湿",
			"sceneDesc": "当湿度过高时，开启除湿机除湿",
			"isOpen": 1,
			"rules": [{
				"id": "01186437b0a843658910da0fb4bae306",
				"rule": "除湿机自动除湿"
			}],
			"createTime": "2018-10-25 13:35:04",
			"updateTime": "2018-10-25 13:35:47",
			"sourceId": "51f01c4fff194b838fac465d43c0acc2",
			"appSceneAlias": "智慧除湿333",
			"type": "deviceFamily",
			"status": "publish",
			"auto": false,
			"canAppTrigger": false,
			"banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B9%BF-0815_20180815131452956.png",
			"icon": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B9%BF2_20180730134826251.png",
			"appId": "MB-UZHSH-0001"
		},
		{
			"id": "986d4cc5d89248538beba67a798c4bee",
			"familyId": "461066634104000000",
			"sceneName": "智慧除湿",
			"sceneDesc": "当湿度过高时，开启除湿机除湿",
			"isOpen": 1,
			"rules": [{
				"id": "83234209e5114271a427b979351fafc8",
				"rule": "除湿机自动除湿"
			}],
			"createTime": "2018-10-24 16:23:12",
			"updateTime": "2018-10-24 16:23:25",
			"sourceId": "51f01c4fff194b838fac465d43c0acc2",
			"appSceneAlias": "智慧除湿2",
			"type": "deviceFamily",
			"status": "publish",
			"auto": false,
			"canAppTrigger": false,
			"banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B9%BF-0815_20180815131452956.png",
			"icon": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B9%BF2_20180730134826251.png",
			"appId": "MB-UZHSH-0001"
		},
		{
			"id": "bbc0513449914aaba2d644127624b814",
			"familyId": "461066634104000000",
			"sceneName": "智慧除湿",
			"sceneDesc": "当湿度过高时，开启除湿机除湿",
			"isOpen": 1,
			"rules": [{
				"id": "a84c847fcfff413dbc21d7051d3776ff",
				"rule": "除湿机自动除湿"
			}],
			"createTime": "2018-10-24 16:22:43",
			"updateTime": "2018-10-24 16:23:07",
			"sourceId": "51f01c4fff194b838fac465d43c0acc2",
			"appSceneAlias": "智慧除湿1",
			"type": "deviceFamily",
			"status": "publish",
			"auto": false,
			"canAppTrigger": false,
			"banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B9%BF-0815_20180815131452956.png",
			"icon": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B9%BF2_20180730134826251.png",
			"appId": "MB-UZHSH-0001"
		},
		{
			"id": "2a4908ab903b402ab6dfaf1814a1e7b9",
			"familyId": "461066634104000000",
			"sceneName": "智慧除湿",
			"sceneDesc": "当湿度过高时，开启除湿机除湿",
			"isOpen": 1,
			"rules": [{
				"id": "3c37ae8d64b644d9b2208e162202e3e3",
				"rule": "除湿机自动除湿"
			}],
			"createTime": "2018-10-24 15:05:48",
			"updateTime": "2018-10-24 15:05:48",
			"sourceId": "51f01c4fff194b838fac465d43c0acc2",
			"type": "deviceFamily",
			"status": "publish",
			"auto": false,
			"canAppTrigger": false,
			"banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B9%BF-0815_20180815131452956.png",
			"icon": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B9%BF2_20180730134826251.png",
			"appId": "MB-****-0000"
		},
		{
			"id": "feb4480bc3a545a7bc250c63819e60a3",
			"familyId": "461066634104000000",
			"sceneName": "智慧除湿",
			"sceneDesc": "当湿度过高时，开启除湿机除湿",
			"isOpen": 1,
			"rules": [{
				"id": "6cd5fae3b4124e04bd45aab95a7bb512",
				"rule": "除湿机自动除湿"
			}],
			"createTime": "2018-10-19 15:56:56",
			"updateTime": "2018-10-19 15:57:26",
			"sourceId": "51f01c4fff194b838fac465d43c0acc2",
			"appSceneAlias": "智慧除湿2",
			"type": "deviceFamily",
			"status": "publish",
			"auto": false,
			"canAppTrigger": false,
			"banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B9%BF-0815_20180815131452956.png",
			"icon": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B9%BF2_20180730134826251.png",
			"appId": "MB-UZHSH-0001"
		},
		{
			"id": "8bd88d682f2241d59aa6ab9a15a71511",
			"familyId": "461066634104000000",
			"sceneName": "空调净化",
			"sceneDesc": "当空气质量差时，开启空调净化模式。",
			"isOpen": 1,
			"rules": [{
				"id": "2879bca6b3864746ad233c1757964c2f",
				"rule": "空调净化模式"
			}],
			"createTime": "2018-09-19 15:04:47",
			"updateTime": "2018-09-19 15:04:47",
			"sourceId": "62597cd68cb34413b00b9408d55b17a4",
			"type": "deviceFamily",
			"status": "publish",
			"auto": false,
			"canAppTrigger": false,
			"appId": "MB-UZHSH-0001"
		},
		{
			"id": "156e313d91e44b6093687e96976e11e1",
			"familyId": "461066634104000000",
			"sceneName": "智慧调温A",
			"sceneDesc": "当温度过高或过低时，开启挂式空调调温。",
			"isOpen": 1,
			"rules": [{
				"id": "76a504771da449afbaff7aed3540c0fa",
				"rule": "挂式空调调温"
			}],
			"createTime": "2018-09-19 18:03:41",
			"updateTime": "2018-09-19 18:03:41",
			"sourceId": "6e5faad84ef143e9a497c310e903baa4",
			"type": "deviceFamily",
			"status": "publish",
			"auto": false,
			"canAppTrigger": false,
			"appId": "MB-UZHSH-0001"
		},
		{
			"id": "bb17f88471b34effb9bfce011af9df82",
			"familyId": "461066634104000000",
			"sceneName": "智慧调温B",
			"sceneDesc": "当温度过高或过低时，开启柜式空调调温",
			"isOpen": 1,
			"rules": [{
				"id": "d67187f3058840498ecc4984df4c4dfc",
				"rule": "柜机空调调温"
			}],
			"createTime": "2018-10-25 14:06:49",
			"updateTime": "2018-10-25 14:06:49",
			"sourceId": "ab7bc975991d4c0e848dcb557f88e7a2",
			"type": "deviceFamily",
			"status": "publish",
			"auto": false,
			"canAppTrigger": false,
			"banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B8%A9-0815_20180815131504944.png",
			"icon": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B8%A92_20180730134901229.png",
			"appId": "MB-****-0000"
		},
		{
			"id": "71da2b99fb3249c08e03c682b3910b43",
			"familyId": "461066634104000000",
			"sceneName": "智慧调温B",
			"sceneDesc": "当温度过高或过低时，开启柜式空调调温",
			"isOpen": 0,
			"rules": [{
				"id": "9dce052a2ad34be89c5d083744b3b479",
				"rule": "柜机空调调温"
			}],
			"createTime": "2018-10-25 14:06:33",
			"updateTime": "2018-10-25 14:06:33",
			"sourceId": "ab7bc975991d4c0e848dcb557f88e7a2",
			"type": "deviceFamily",
			"status": "publish",
			"auto": false,
			"canAppTrigger": false,
			"banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B8%A9-0815_20180815131504944.png",
			"icon": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B8%A92_20180730134901229.png",
			"appId": "MB-****-0000"
		},
		{
			"id": "0c61e696355940fd987f7cc2f3d6bfbe",
			"familyId": "461066634104000000",
			"sceneName": "智慧调温B",
			"sceneDesc": "当温度过高或过低时，开启柜式空调调温",
			"isOpen": 1,
			"rules": [{
				"id": "70bfaa82cc994142b1db9a2afc4d899f",
				"rule": "柜机空调调温"
			}],
			"createTime": "2018-10-25 14:03:38",
			"updateTime": "2018-10-25 14:04:13",
			"sourceId": "ab7bc975991d4c0e848dcb557f88e7a2",
			"appSceneAlias": "测试bug",
			"type": "deviceFamily",
			"status": "publish",
			"auto": false,
			"canAppTrigger": false,
			"banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B8%A9-0815_20180815131504944.png",
			"icon": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B8%A92_20180730134901229.png",
			"appId": "MB-****-0000"
		},
		{
			"id": "e493c8fa99aa41baa6a5a8ac92f15e74",
			"familyId": "461066634104000000",
			"sceneName": "智慧调温B",
			"sceneDesc": "当温度过高或过低时，开启柜式空调调温",
			"isOpen": 0,
			"rules": [{
				"id": "3b396bb362d545f8a8f5dc9a8bcd5a54",
				"rule": "柜机空调调温"
			}],
			"createTime": "2018-10-25 14:02:06",
			"updateTime": "2018-10-25 14:02:31",
			"sourceId": "ab7bc975991d4c0e848dcb557f88e7a2",
			"appSceneAlias": "智慧调温BR234",
			"type": "deviceFamily",
			"status": "publish",
			"auto": false,
			"canAppTrigger": false,
			"banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B8%A9-0815_20180815131504944.png",
			"icon": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B8%A92_20180730134901229.png",
			"appId": "MB-****-0000"
		},
		{
			"id": "26ff085323854153b51635ec3e98f885",
			"familyId": "461066634104000000",
			"sceneName": "智慧调温B",
			"sceneDesc": "当温度过高或过低时，开启柜式空调调温",
			"isOpen": 0,
			"rules": [{
				"id": "97a94c470ccc49c5ba0f79b3d14ed812",
				"rule": "柜机空调调温"
			}],
			"createTime": "2018-10-25 14:01:32",
			"updateTime": "2018-10-25 14:01:55",
			"sourceId": "ab7bc975991d4c0e848dcb557f88e7a2",
			"appSceneAlias": "智慧调温BR123",
			"type": "deviceFamily",
			"status": "publish",
			"auto": false,
			"canAppTrigger": false,
			"banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B8%A9-0815_20180815131504944.png",
			"icon": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B8%A92_20180730134901229.png",
			"appId": "MB-****-0000"
		},
		{
			"id": "fefe5c439a394a4291fd15d635c214db",
			"familyId": "461066634104000000",
			"sceneName": "智慧调温B",
			"sceneDesc": "当温度过高或过低时，开启柜式空调调温",
			"isOpen": 0,
			"rules": [{
				"id": "954e0f8b683a4f818e2b97ca33953ff3",
				"rule": "柜机空调调温"
			}],
			"createTime": "2018-10-25 14:00:30",
			"updateTime": "2018-10-25 14:01:16",
			"sourceId": "ab7bc975991d4c0e848dcb557f88e7a2",
			"appSceneAlias": "智慧调温RYJ",
			"type": "deviceFamily",
			"status": "publish",
			"auto": false,
			"canAppTrigger": false,
			"banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B8%A9-0815_20180815131504944.png",
			"icon": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B8%A92_20180730134901229.png",
			"appId": "MB-****-0000"
		},
		{
			"id": "a5c15fa7624e4f308fe3d8c497c7fc22",
			"familyId": "461066634104000000",
			"sceneName": "智慧调温B",
			"sceneDesc": "当温度过高或过低时，开启柜式空调调温",
			"isOpen": 1,
			"rules": [{
				"id": "c41a6b7021ce49b68a94e41be1bcdadf",
				"rule": "柜机空调调温"
			}],
			"createTime": "2018-10-25 13:53:30",
			"updateTime": "2018-10-25 13:53:56",
			"sourceId": "ab7bc975991d4c0e848dcb557f88e7a2",
			"appSceneAlias": "42924",
			"type": "deviceFamily",
			"status": "publish",
			"auto": false,
			"canAppTrigger": false,
			"banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B8%A9-0815_20180815131504944.png",
			"icon": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B8%A92_20180730134901229.png",
			"appId": "MB-UZHSH-0001"
		},
		{
			"id": "84b1bce21a374e99990c4a61547eebe0",
			"familyId": "461066634104000000",
			"sceneName": "智慧调温B",
			"sceneDesc": "当温度过高或过低时，开启柜式空调调温",
			"isOpen": 1,
			"rules": [{
				"id": "1bec22d28afa44939e22ff649525222d",
				"rule": "柜机空调调温"
			}],
			"createTime": "2018-10-25 12:25:52",
			"updateTime": "2018-10-25 12:27:06",
			"sourceId": "ab7bc975991d4c0e848dcb557f88e7a2",
			"appSceneAlias": "智慧调温B42924",
			"type": "deviceFamily",
			"status": "publish",
			"auto": false,
			"canAppTrigger": false,
			"banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B8%A9-0815_20180815131504944.png",
			"icon": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B8%A92_20180730134901229.png",
			"appId": "MB-UZHSH-0001"
		},
		{
			"id": "9e4ae1d541ee408e98cd3c58617a19c9",
			"familyId": "461066634104000000",
			"sceneName": "智慧调温B",
			"sceneDesc": "当温度过高或过低时，开启柜式空调调温",
			"isOpen": 1,
			"rules": [{
				"id": "e4bf874bb518442cbf6e4f8abcd919b3",
				"rule": "柜机空调调温"
			}],
			"createTime": "2018-10-25 12:16:39",
			"updateTime": "2018-10-25 12:17:06",
			"sourceId": "ab7bc975991d4c0e848dcb557f88e7a2",
			"appSceneAlias": "智慧调温B888",
			"type": "deviceFamily",
			"status": "publish",
			"auto": false,
			"canAppTrigger": false,
			"activeBeginTime": "10:00:00",
			"activeEndTime": "01:59:00",
			"banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B8%A9-0815_20180815131504944.png",
			"icon": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B8%A92_20180730134901229.png",
			"appId": "MB-UZHSH-0001"
		},
		{
			"id": "4e0865d09f28475794a89be3535150e0",
			"familyId": "461066634104000000",
			"sceneName": "智慧调温B",
			"sceneDesc": "当温度过高或过低时，开启柜式空调调温",
			"isOpen": 1,
			"rules": [{
				"id": "d5ad9a00e5984ccc8b7675795019220e",
				"rule": "柜机空调调温"
			}],
			"createTime": "2018-10-25 12:12:20",
			"updateTime": "2018-10-25 12:12:20",
			"sourceId": "ab7bc975991d4c0e848dcb557f88e7a2",
			"type": "deviceFamily",
			"status": "publish",
			"auto": false,
			"canAppTrigger": false,
			"banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B8%A9-0815_20180815131504944.png",
			"icon": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B8%A92_20180730134901229.png",
			"appId": "MB-UZHSH-0001"
		},
		{
			"id": "c984de2b5b48450aa912406866ec1fe7",
			"familyId": "461066634104000000",
			"sceneName": "智慧调温B",
			"sceneDesc": "当温度过高或过低时，开启柜式空调调温。",
			"isOpen": 1,
			"rules": [{
				"id": "ecb965c126b44833b82745effe11b8cc",
				"rule": "柜机空调调温"
			}],
			"createTime": "2018-10-24 16:10:13",
			"updateTime": "2018-10-24 16:12:48",
			"sourceId": "ab7bc975991d4c0e848dcb557f88e7a2",
			"appSceneAlias": "智慧调温B222",
			"type": "deviceFamily",
			"status": "publish",
			"auto": false,
			"canAppTrigger": false,
			"activeBeginTime": "16:00:00",
			"activeEndTime": "01:59:00",
			"banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B8%A9-0815_20180815131504944.png",
			"icon": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B8%A92_20180730134901229.png",
			"appId": "MB-****-0000"
		},
		{
			"id": "98ee4c4936924dd28517189dd9af0264",
			"familyId": "461066634104000000",
			"sceneName": "智慧调温B",
			"sceneDesc": "当温度过高或过低时，开启柜式空调调温。",
			"isOpen": 1,
			"rules": [{
				"id": "fa3540fd15ff49f5882aa50b314748a0",
				"rule": "柜机空调调温"
			}],
			"createTime": "2018-10-24 16:09:50",
			"updateTime": "2018-10-24 16:09:50",
			"sourceId": "ab7bc975991d4c0e848dcb557f88e7a2",
			"type": "deviceFamily",
			"status": "publish",
			"auto": false,
			"canAppTrigger": false,
			"banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B8%A9-0815_20180815131504944.png",
			"icon": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B8%A92_20180730134901229.png",
			"appId": "MB-****-0000"
		},
		{
			"id": "13979a4b7d924338b3db468de70ec084",
			"familyId": "461066634104000000",
			"sceneName": "智慧调温B",
			"sceneDesc": "当温度过高或过低时，开启柜式空调调温。",
			"isOpen": 1,
			"rules": [{
				"id": "e1bb6b89d325420c9d1941c32e1e516a",
				"rule": "柜机空调调温"
			}],
			"createTime": "2018-10-24 15:15:39",
			"updateTime": "2018-10-24 15:15:39",
			"sourceId": "ab7bc975991d4c0e848dcb557f88e7a2",
			"type": "deviceFamily",
			"status": "publish",
			"auto": false,
			"canAppTrigger": false,
			"banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B8%A9-0815_20180815131504944.png",
			"icon": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B8%A92_20180730134901229.png",
			"appId": "MB-****-0000"
		},
		{
			"id": "0e49aef8d37e4aea9242acda205cfbce",
			"familyId": "461066634104000000",
			"sceneName": "智慧调温B",
			"sceneDesc": "当温度过高或过低时，开启柜式空调调温。",
			"isOpen": 1,
			"rules": [{
				"id": "5c5f7c7d23544e74858ab0d9f0ecd460",
				"rule": "柜机空调调温"
			}],
			"createTime": "2018-10-18 16:42:05",
			"updateTime": "2018-10-18 16:42:05",
			"sourceId": "ab7bc975991d4c0e848dcb557f88e7a2",
			"type": "deviceFamily",
			"status": "publish",
			"auto": false,
			"canAppTrigger": false,
			"activeBeginTime": "13:00:00",
			"activeEndTime": "13:00:00",
			"banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B8%A9-0815_20180815131504944.png",
			"icon": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B8%A92_20180730134901229.png",
			"appId": "MB-UZHSH-0001"
		},
		{
			"id": "f1121ed0d8954e34896f974678c92989",
			"familyId": "461066634104000000",
			"sceneName": "智慧调温B",
			"sceneDesc": "当温度过高或过低时，开启柜式空调调温。",
			"isOpen": 1,
			"rules": [{
				"id": "9c7a5c7036704d9e96811d947607a492",
				"rule": "柜机空调调温"
			}],
			"createTime": "2018-10-17 13:20:14",
			"updateTime": "2018-10-17 13:20:14",
			"sourceId": "ab7bc975991d4c0e848dcb557f88e7a2",
			"type": "deviceFamily",
			"status": "publish",
			"auto": false,
			"canAppTrigger": false,
			"banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B8%A9-0815_20180815131504944.png",
			"icon": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B8%A92_20180730134901229.png",
			"appId": "MB-UZHSH-0001"
		},
		{
			"id": "e9415322d05f4f449670ba5886934823",
			"familyId": "461066634104000000",
			"sceneName": "智慧调温B",
			"sceneDesc": "当温度过高或过低时，开启柜式空调调温。",
			"isOpen": 1,
			"rules": [{
				"id": "4958c7c00e834de190f8cfb6389044df",
				"rule": "柜机空调调温"
			}],
			"createTime": "2018-09-17 14:30:12",
			"updateTime": "2018-09-18 10:33:28",
			"sourceId": "ab7bc975991d4c0e848dcb557f88e7a2",
			"type": "deviceFamily",
			"status": "publish",
			"auto": false,
			"canAppTrigger": false,
			"activeBeginTime": "01:00:00",
			"activeEndTime": "22:58:00",
			"appId": "MB-UZHSH-0001"
		},
		{
			"id": "570900776e9748fdb5d7ba7cd00dd906",
			"familyId": "461066634104000000",
			"sceneName": "智慧调温B",
			"sceneDesc": "当温度过高或过低时，开启柜式空调调温。",
			"isOpen": 1,
			"rules": [{
				"id": "c770b029a89748c48ba7cdb47b3778ee",
				"rule": "柜机空调调温"
			}],
			"createTime": "2018-09-17 14:29:53",
			"updateTime": "2018-09-18 10:35:31",
			"sourceId": "ab7bc975991d4c0e848dcb557f88e7a2",
			"type": "deviceFamily",
			"status": "publish",
			"auto": false,
			"canAppTrigger": false,
			"activeBeginTime": "01:01:00",
			"activeEndTime": "22:58:00",
			"appId": "MB-UZHSH-0001"
		},
		{
			"id": "31bdf807a78e4307a5ada98bfc3a01e9",
			"familyId": "461066634104000000",
			"sceneName": "内测-满足任一条件（不同设备）",
			"sceneDesc": "任一空调制热时执行恒湿恒氧",
			"isOpen": 1,
			"rules": [{
				"id": "946df9fe63d647d8bc506ffe892a06f8",
				"rule": "空调任一制热时执行恒湿恒氧"
			}],
			"createTime": "2018-09-18 12:12:34",
			"updateTime": "2018-09-18 12:12:34",
			"sourceId": "fdb8afc6e5ae409b8dfbaeba228fea8d",
			"type": "deviceFamily",
			"status": "publish",
			"auto": false,
			"canAppTrigger": false,
			"banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E5%9B%9E%E5%AE%B6-0815_20180815131530653.png",
			"icon": "http://resource.haier.net:80/download/resource/uzhsh/00001/one_key_20180314.png",
			"appId": "MB-UZHSH-0001"
		},
		{
			"id": "b0fba39a8a764f379ba575d402cd2b27",
			"familyId": "461066634104000000",
			"sceneName": "内测-满足任一条件（不同设备）",
			"sceneDesc": "任一空调制热时执行恒湿恒氧",
			"isOpen": 1,
			"rules": [{
				"id": "e57f5bc8ee3c4199b4590baadaaf5d6b",
				"rule": "空调任一制热时执行恒湿恒氧"
			}],
			"createTime": "2018-09-18 12:12:19",
			"updateTime": "2018-09-18 12:12:19",
			"sourceId": "fdb8afc6e5ae409b8dfbaeba228fea8d",
			"type": "deviceFamily",
			"status": "publish",
			"auto": false,
			"canAppTrigger": false,
			"banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E5%9B%9E%E5%AE%B6-0815_20180815131530653.png",
			"icon": "http://resource.haier.net:80/download/resource/uzhsh/00001/one_key_20180314.png",
			"appId": "MB-UZHSH-0001"
		},
		{
			"id": "62a42daa24f1443e99ea7c9cc9289325",
			"familyId": "461066634104000000",
			"sceneName": "内测-满足任一条件（不同设备）",
			"sceneDesc": "任一空调制热时执行恒湿恒氧",
			"isOpen": 1,
			"rules": [{
				"id": "7e778b60d4094500961fff16c4f3f5ff",
				"rule": "空调任一制热时执行恒湿恒氧"
			}],
			"createTime": "2018-09-18 12:11:11",
			"updateTime": "2018-09-18 12:11:11",
			"sourceId": "fdb8afc6e5ae409b8dfbaeba228fea8d",
			"type": "deviceFamily",
			"status": "publish",
			"auto": false,
			"canAppTrigger": false,
			"banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E5%9B%9E%E5%AE%B6-0815_20180815131530653.png",
			"icon": "http://resource.haier.net:80/download/resource/uzhsh/00001/one_key_20180314.png",
			"appId": "MB-UZHSH-0001"
		}],
		"fromIndex": 0,
		"toIndex": 57
	}
}
```

##### 3、错误码  
> 见 快速开始——常用信息——平台公共错误码 


#### 根据家庭下的场景Id批量查询场景列表
>根据家庭下的场景Id批量查询场景列表。

##### 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/sceneInfamily`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|
| familyId| String |32| Body| 必填|家庭Id|  
| ids| String[]|32| Body| 选填|场景Id|   

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  | SceneDto[]| Body  |  必填 |显示场景Store中的描述信息，规则rules中带有规则Id和规则名称 |   

```java  
Header：
appId: MB-****-0000
appVersion: 3.3.0
clientId: 123456
sequenceId: 20161020153428000015
accessToken: TGT1RTMIZLU4ERIC1Y6UY558EVOE70
sign: ea50f7047b241e947c068adf1b6996ee9ea3cc7079923c51f5dd5acf66dcb71d
timestamp: 1543314284096 
language: zh-cn
timezone: +8
appKey: f50c76fbc8271d361e1f6b5973f54585
Content-Encoding: utf-8
Content-type: application/json
Body
{
	"familyId": "461066634104000000",
	"ids": ["98ee4c4936924dd28517189dd9af0264",
	"b06bdda15deb4ee3825a9490124ddc87",
	"feb4480bc3a545a7bc250c63819e60a3",
	"8bd88d682f2241d59aa6ab9a15a71511"]
}

```  

**请求应答**

```java
{
  "retCode": "00000",
  "retInfo": "成功",
  "data": [
    {
      "id": "98ee4c4936924dd28517189dd9af0264",
      "familyId": "461066634104000000",
      "sceneName": "智慧调温B",
      "sceneDesc": "当温度过高或过低时，开启柜式空调调温。",
      "isOpen": 0,
      "rules": [
        {
          "id": "fa3540fd15ff49f5882aa50b314748a0",
          "rule": "柜机空调调温"
        }
      ],
      "createTime": "2018-10-24 16:09:50",
      "updateTime": "2018-10-24 16:09:50",
      "sourceId": "ab7bc975991d4c0e848dcb557f88e7a2",
      "type": "deviceFamily",
      "fromType": "download",
      "status": "publish",
      "auto": false,
      "canAppTrigger": false,
      "banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B8%A9-0815_20180815131504944.png",
      "icon": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B8%A92_20180730134901229.png",
      "appId": "MB-****-0000"
    },
    {
      "id": "b06bdda15deb4ee3825a9490124ddc87",
      "familyId": "461066634104000000",
      "sceneName": "一键离家（旧版）",
      "sceneDesc": "IFTTT1.0_场景38_一键离家 ",
      "isOpen": 0,
      "rules": [
        {
          "id": "ed6fd4aca4e942d4b653b8be1ef4a5e7",
          "rule": "IFTTT1.0_场景38_一键离家"
        }
      ],
      "createTime": "2018-10-18 17:07:30",
      "updateTime": "2018-10-18 17:07:30",
      "sourceId": "22f86c65748d4a4aa362627cffae82a0",
      "type": "deviceFamily",
      "fromType": "download",
      "status": "publish",
      "auto": false,
      "canAppTrigger": true,
      "banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E7%A6%BB%E5%AE%B6-0815_20180815131404444.png",
      "icon": "http://resource.haier.net:80/download/resource/uzhsh/00001/one_key_20180314.png",
      "appId": "MB-UZHSH-0001"
    },
    {
      "id": "feb4480bc3a545a7bc250c63819e60a3",
      "familyId": "461066634104000000",
      "sceneName": "智慧除湿",
      "sceneDesc": "当湿度过高时，开启除湿机除湿",
      "isOpen": 0,
      "rules": [
        {
          "id": "6cd5fae3b4124e04bd45aab95a7bb512",
          "rule": "除湿机自动除湿"
        }
      ],
      "createTime": "2018-10-19 15:56:56",
      "updateTime": "2018-10-19 15:57:26",
      "sourceId": "51f01c4fff194b838fac465d43c0acc2",
      "appSceneAlias": "智慧除湿2",
      "type": "deviceFamily",
      "fromType": "download",
      "status": "publish",
      "auto": false,
      "canAppTrigger": false,
      "banner": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B9%BF-0815_20180815131452956.png",
      "icon": "http://resource.haigeek.com/download/resource/selfService/admin/%E6%81%92%E6%B9%BF2_20180730134826251.png",
      "appId": "MB-UZHSH-0001"
    },
    {
      "id": "8bd88d682f2241d59aa6ab9a15a71511",
      "familyId": "461066634104000000",
      "sceneName": "空调净化",
      "sceneDesc": "当空气质量差时，开启空调净化模式。",
      "isOpen": 0,
      "rules": [
        {
          "id": "2879bca6b3864746ad233c1757964c2f",
          "rule": "空调净化模式"
        }
      ],
      "createTime": "2018-09-19 15:04:47",
      "updateTime": "2018-09-19 15:04:47",
      "sourceId": "62597cd68cb34413b00b9408d55b17a4",
      "type": "deviceFamily",
      "fromType": "download",
      "status": "publish",
      "auto": false,
      "canAppTrigger": false,
      "appId": "MB-UZHSH-0001"
    }
  ]
}
```

##### 3、错误码  
> 见 快速开始——常用信息——平台公共错误码 



#### 修改场景昵称
>修改场景昵称（别名）。

##### 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/updateAppSceneAlias`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|
| sceneId| String |32| Body| 必填|场景id|  
| basicSceneAlias| String|64| Body| 必填|基础场景昵称|   

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  | Object| Body  |  必填 |显示为：null |

##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 3.3.0
clientId: 123456
sequenceId: 20161020153428000015
accessToken: TGT1IIOXZJHLWOXF2FED4YZTGIQ3B0
sign: 14a5736f619376ffad7d539e4540e644c711a39996ff2f4acb396d99bd3586cd
timestamp: 1542356687190 
language: zh-cn
timezone: +8
appKey: f50c76fbc8271d361e1f6b5973f54585
Content-Encoding: utf-8
Content-type: application/json
Body
{
	"sceneId": "56240541ee1848e69b672d42303b037e", "appSceneAlias":"jiayk001" 
}

```  

**请求应答**

```java
{
	"retCode": "00000",
	"retInfo": "成功",
	"data": null
}
```

##### 3、错误码  
> 见 快速开始——常用信息——平台公共错误码 






#### 修改场景生效时间段
>修改场景生效时间段。

##### 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/updateActiveTime`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|
| sceneId| String |32| Body| 必填|场景id|  
| activeBeginTime| String|64| Body| 必填|场景生效开始时间(`“HH:mm:ss”`对应的String形式)|  
| activeEndTime| String|64| Body| 必填|场景生效结束时间(`“HH:mm:ss”`对应的String形式)|  

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  | Object| Body  |  必填 |显示为：null |


##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 3.3.0
clientId: 123456
sequenceId: 20161020153428000015
accessToken: TGT1IIOXZJHLWOXF2FED4YZTGIQ3B0
sign: 2ce8188bdad4cd5df8bfe21952469d4a34d68fc90a5fabece365e9dba5c71244
timestamp: 1542357698903 
language: zh-cn
timezone: +8
appKey: f50c76fbc8271d361e1f6b5973f54585
Content-Encoding: utf-8
Content-type: application/json
Body
{
	"sceneId": "56240541ee1848e69b672d42303b037e",
	"activeBeginTime": "10:24:12",
	"activeEndTime": "11:24:12"
}
```  

**请求应答**

```java
{
	"retCode": "00000",
	"retInfo": "成功",
	"data": null
}
```

##### 3、错误码  
> 见 快速开始——常用信息——平台公共错误码 



#### 删除用户下载的场景
>删除用户下载的场景。

##### 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/deleteAppScene`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|
| familyId| String |32| Body| 必填|家庭id|  
| sceneIds| String[]|32| Body| 必填|要删除的场景id数组|  
 

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  | Object| Body  |  必填 |显示为：null |


##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 3.3.0
clientId: 123456
sequenceId: 20161020153428000015
accessToken: TGT1IIOXZJHLWOXF2FED4YZTGIQ3B0
sign: e7ff69d158c8faf7db9bd90b61571395e96dc4f0b3689a295c08593b830a8f65
timestamp: 1542357835611 
language: zh-cn
timezone: +8
appKey: f50c76fbc8271d361e1f6b5973f54585
Content-Encoding: utf-8
Content-type: application/json
Body
{
	"familyId": "191092663982000000",
	"appId": "MB-****-0001",
	"sceneIds": ["61b830e6fc3940daaca7c487ce7f288c"]
}
```  

**请求应答**

```java
{
	"retCode": "00000",
	"retInfo": "成功",
	"data": null
}
```

##### 3、错误码  
> 见 快速开始——常用信息——平台公共错误码 




#### 用户创建平台触发类场景
>根据用户填写的参数保存场景。

##### 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/addUserScene`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|
| userSceneDto| UserSceneDto |&emsp;| Body| 必填|场景参数 [Json结构](zh-cn/IFTTT_jsonDataStructure)|           
 

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  | Object| Body  |  必填 |返回创建成功后的场景ID |


##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 3.3.0
clientId: 123456
sequenceId: 20161020153428000015
accessToken: TGT1RTMIZLU4ERIC1Y6UY558EVOE70
sign: 8a4447bead76f47f85580b1291f36c6c59ca84415307f4ca89d1ffa3cae0b11d
timestamp: 1542614635624 
language: zh-cn
timezone: +8
appKey: f50c76fbc8271d361e1f6b5973f54585
Content-Encoding: utf-8
Content-type: application/json
Body
{
  "userSceneDto": {
    "auto": false,
    "familyId": "717042585118000000",
    "isOpen": 0,
    "rules": [
      {
        "then": {
          "actions": [
            {
              "control": {
                "args": [
                  {
                    "name": {
                      "id": "5cb7157b972b47648fd48cadd2b03380",
                      "required": true
                    },
                    "value": {
                      "desc": "低风",
                      "required": true,
                      "value": "3"
                    }
                  }
                ],
                "componentId": "fd1519209d7c11e88943fa163eb273a5",
                "object": {
                  "required": false,
                  "value": "DC330D630E49"
                },
                "operation": {
                  "desc": "设置风速",
                  "id": "ff58285a9e184eacb7a84d5a9e643aef",
                  "required": false
                }
              },
              "dealyTime": 0,
              "type": "DeviceControl"
            }
          ]
        },
        "when": {
          "conditions": [
            {
              "componentId": "fd1519209d7c11e88943fa163eb273a5",
              "desc": "开关机状态设置为等于开机",
              "key": {
                "id": "bd0ebf1efb5f45f18beced92ebcec529",
                "required": true
              },
              "object": {
                "desc": "除湿机1",
                "required": false,
                "value": "DC330D630E49"
              },
              "operationSign": "equal",
              "value": {
                "desc": "开机",
                "required": true,
                "value": "true"
              }
            }
          ]
        }
      }
    ],
    "sceneDesc": "中国版123",
    "sceneName": "中国版123",
    "userId": "5458199"
  }
}
```  

**请求应答**

```java
{
	"retCode": "00000",
	"retInfo": "成功",
	"data": "6aa9a62f720d460da7293d7fc40453aa"
}
```

##### 3、错误码  
> 见 快速开始——常用信息——平台公共错误码 

#### 用户创建手动触发类场景
>根据用户填写的参数保存场景。

##### 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/addUserManualScene`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|
| userSceneDto| UserSceneDto |&emsp;| Body| 必填|场景参数 [Json结构](zh-cn/IFTTT_jsonDataStructure)|           
 

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  | Object| Body  |  必填 |返回创建成功后的场景ID |



#### 用户创建定时执行类场景
>根据用户填写的参数保存场景。

##### 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/addUserTimingScene`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|
| userSceneDto| UserSceneDto |&emsp;| Body| 必填|场景参数 [Json结构](zh-cn/IFTTT_jsonDataStructure)|           
 

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  | Object| Body  |  必填 |返回创建成功后的场景ID |


##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 3.3.0
clientId: 123456
sequenceId: 20161020153428000015
accessToken: TGT1RTMIZLU4ERIC1Y6UY558EVOE70
sign: 088084235118c25106051fc3f13aa6de67fa3a4cd3f17246541f157476d4eec8
timestamp: 1543460486091 
language: zh-cn
timezone: +8
appKey: f50c76fbc8271d361e1f6b5973f54585
Content-Encoding: utf-8
Content-type: application/json
Body
{
	"userSceneDto": {
		"sceneName": "质量部定时执行场景guy_001_081210:31",
		"sceneAlias": "zhiliangbuscene alias",
		"sceneDesc": "scene descrition",
		"userId": "100013957366157121",
		"familyId": "aabbccdd1234567890",
		"type": "deviceUser",
		"type": "User",
		"triggerType": "platform",
		"isOpen": 0,
		"weight": 2,
		"aiKeyword": "ai keyword",
		"taskInfo": {
			"type": 1,
			"cron": {
				"minutes": "15",
				"hours": "10",
				"day": "?",
				"month": "*",
				"week": "MON,FRI",
				"year": "*"
			},
			"status": true,
			"activeBeginTime": "09:00:00",
			"activeEndTime": "19:00:00"
		},
		"rules": [{
			"rule": "质量部定时执行场景片段一_081210:31",
			"salience": 1,
			"when": {
				"desc": "lyx0525条件描述",
				"conditions": [{
					"desc": "室内温度过高",
					"key": {
						"id": "d0c2d17a894711e7ba2bfa163eb273a5",
						"value": "onOffStatus",
						"desc": "运行状态",
						"scope": {
							"value": "{\"onOffStatus\":\"false\",\"onOffStatus\":\"true\"}",
							"type": "bool"
						}
					},
					"operationSign": "equal",
					"value": {
						"value": "false",
						"desc": "1",
						"required": "true"
					},
					"object": {
						"value": "DC330D000038",
						"desc": "deviceMacDescription",
						"required": "false"
					},
					"componentId": "4e8a0dfb038d40fab6c5b49369240000"
				}]
			},
			"then": {
				"actions": [{
					"control": {
						"operation": {
							"id": "029c006ccb754681a59ee43cc7c61b9e",
							"desc": "2.3引擎关爱场景-空调-1.0-分体",
							"scope": {
								"value": "{\"onOffStatus\":\"false\",\"onOffStatus\":\"true\"}",
								"type": "int"
							},
							"value": 20,
							"required": true
						},
						"args": null,
						"object": {
							"value": "DC330D000038",
							"desc": "deviceMacDescription",
							"required": "false"
						},
						"controlBtnText": null,
						"componentId": "4e8a0dfb038d40fab6c5b49369240000"
					},
					"dealyTime": null,
					"desc": "",
					"pushMessage": null,
					"type": "DeviceControl",
					"isOpen": 0
				}]
			},
			"isOpen": 0
		}],
		"sortList": [{
			"id": "1",
			"name": "分类1"
		}],
		"tagList": [{
			"id": "1",
			"name": "标签1"
		}]
	}
}

```  

**请求应答**

```java
{
	"retCode": "00000",
	"retInfo": "成功",
	"data": "3f0c17a6073245cda7bee7d2b2dea674"
}
```

##### 3、错误码  
> 见 快速开始——常用信息——平台公共错误码 



#### 修改用户平台触发类场景
>修改用户平台触发类场景（传入参数userSceneDto中必须传入需要修改的id,userId不能修改）。</br>说明：1、更新场景只能更新 场景名称、场景描述、场景别名、定时策略、规则、权重、banner、icon，标签、分类本期暂时没有涉及，先不做要求。</br>2、如果要修改场景的规则信息，必须传入原有场景的ruleId;


##### 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/updateUserScene`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|
| userSceneDto| UserSceneDto |&emsp;| Body| 必填|场景参数 [Json结构](zh-cn/IFTTT_jsonDataStructure)|           
 

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  | Object| Body  |  必填 |返回创建成功后的场景ID |


#### 修改用户手动触发类场景
>修改用户手动触发类场景（传入参数userSceneDto中必须传入需要修改的id,userId不能修改）。</br>说明：1、更新场景只能更新 场景名称、场景描述、场景别名、定时策略、规则、权重、banner、icon，标签、分类本期暂时没有涉及，先不做要求。</br>2、如果要修改场景的规则信息，必须传入原有场景的ruleId;


##### 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/updateUserManualScene`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|
| userSceneDto| UserSceneDto |&emsp;| Body| 必填|场景参数 [Json结构](zh-cn/IFTTT_jsonDataStructure)|           
 

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  | Object| Body  |  必填 |返回创建成功后的场景ID |   



#### 修改用户定时执行类场景
>修改用户定时执行类场景（传入参数userSceneDto中必须传入需要修改的id,userId不能修改）。</br>说明：1、更新场景只能更新 场景名称、场景描述、场景别名、定时策略、规则、权重、banner、icon，标签、分类本期暂时没有涉及，先不做要求。</br>2、如果要修改场景的规则信息，必须传入原有场景的ruleId;



##### 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/updateTimingUserScene`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|
| userSceneDto| UserSceneDto |&emsp;| Body| 必填|场景参数 [Json结构](zh-cn/IFTTT_jsonDataStructure)|           
 

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  | Object| Body  |  必填 |返回创建成功后的场景ID |





#### 规则类获取规则详情
>根据规则Id查询规则具体信息。

##### 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/rule/getById`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|
| familyId| String |32| Body| 必填|家庭Id|           
| ruleId| String |32| Body| 必填|规则Id|     

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  | RuleTemplateDto| Body  |  必填 |&emsp;  |



#### 修改规则名称
>修改规则名称。

##### 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/updateRuleName`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| ruleId| String |32| Body| 必填|场景Id|  
| name| String |255| Body| 必填|规则名称|      

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  | Object| Body  |  必填 |显示为：null  |

##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 3.3.0
clientId: 123456
sequenceId: 20161020153428000015
accessToken: TGT1RTMIZLU4ERIC1Y6UY558EVOE70
sign: 7ffa0beaba5480237d441ddeeb0087b85c2ce675ba551c377330623a2de14690
timestamp: 1542618834303 
language: zh-cn
timezone: +8
appKey: f50c76fbc8271d361e1f6b5973f54585
Content-Encoding: utf-8
Content-type: application/json
Body
{
	"ruleId": "eb83f2d339d644839e3f95901a0af094",
	"name": "2.3引擎关爱场景-空调-1.0-分体jiayk"
}
```  

**请求应答**

```java
{
	"retCode": "00000",
	"retInfo": "成功",
	"data": null
}
```

##### 3、错误码  
> 见 快速开始——常用信息——平台公共错误码 





#### 修改规则描述
>修改规则描述。

##### 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/updateRuleDescribe`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| ruleId| String |32| Body| 必填|场景Id|  
| describe| String |512| Body| 必填|规则描述|      

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  | Object| Body  |  必填 |显示为：null  |

##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 3.3.0
clientId: 123456
sequenceId: 20161020153428000015
accessToken: TGT1RTMIZLU4ERIC1Y6UY558EVOE70
sign: 22f09c8c7349bda4da2d6fb3e1bad9780838b0ea780344b6ec2e4711c8e5097c
timestamp: 1542618995204 
language: zh-cn
timezone: +8
appKey: f50c76fbc8271d361e1f6b5973f54585
Content-Encoding: utf-8
Content-type: application/json
Body
{
	"ruleId": "eb83f2d339d644839e3f95901a0af094",
	"describe": "rule_description1_jiayk"
}
```  

**请求应答**

```java
{
	"retCode": "00000",
	"retInfo": "成功",
	"data": null
}
```

##### 3、错误码  
> 见 快速开始——常用信息——平台公共错误码 






#### 修改规则开关状态
>修改规则开关状态接。

##### 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/updateRuleStatus`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| ruleId| String |32| Body| 必填|规则Id|  
| isOpen| Integer |N/A| Body| 必填|场景片段是否开启: 1 开启 0 未开启 App根据isOpen状态判断能否编辑规则和设置规则参数，isOpen只有是在开启状态才可编辑规则和设置规则参数|      

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  | Object| Body  |  必填 |显示为：null  |


##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 3.3.0
clientId: 123456
sequenceId: 20161020153428000015
accessToken: TGT1RTMIZLU4ERIC1Y6UY558EVOE70
sign: 5d88391974492f6ccf8dfdfb8c2ca8721f5ced9ece6e45e2523459d4edeb9093
timestamp: 1542619171879 
language: zh-cn
timezone: +8
appKey: f50c76fbc8271d361e1f6b5973f54585
Content-Encoding: utf-8
Content-type: application/json
Body
{ 
	"ruleId": "eb83f2d339d644839e3f95901a0af094", "isOpen": "1"
}
```  

**请求应答**

```java
{
	"retCode": "00000",
	"retInfo": "成功",
	"data": null
}
```

##### 3、错误码  
> 见 快速开始——常用信息——平台公共错误码 





#### 修改行为开关状态
>修改行为开关状态。

##### 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/updateActionStatus`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| ruleActionId| String |32| Body| 必填|规则行为Id|  
| isOpen| Integer |N/A| Body| 必填|行为是否开启: 1 已开启 0 未开启 App根据isOpen状态判断能否编辑规则和设置规则参数，isOpen只有是在开启状态才可编辑规则和设置规则参数 |      

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  | Object| Body  |  必填 |显示为：null  |

##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 3.3.0
clientId: 123456
sequenceId: 20161020153428000015
accessToken: TGT1RTMIZLU4ERIC1Y6UY558EVOE70
sign: 1655668e4ecb63ba1b6e89d12d42ba7719c3c62f3a339d6419de8f5822c1db75
timestamp: 1542619291670 
language: zh-cn
timezone: +8
appKey: f50c76fbc8271d361e1f6b5973f54585
Content-Encoding: utf-8
Content-type: application/json
Body
{ 
	"ruleActionId": "a0b2568f2c794ec4a7ec7b162029d854", "isOpen": "1"
}
```  

**请求应答**

```java
{
	"retCode": "00000",
	"retInfo": "成功",
	"data": null
}
```

##### 3、错误码  
> 见 快速开始——常用信息——平台公共错误码 





#### 修改消息推送行为中消息内容
>修改消息推送行为中消息内容。

##### 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/updatePushMessage`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| ruleActionId| String |32| Body| 必填|规则行为Id|  
| pushMessage| String |N/A| Body| 必填|推送的消息内容 |      

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  | Object| Body  |  必填 |显示为：null  |

##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 3.3.0
clientId: 123456
sequenceId: 20161020153428000015
accessToken: TGT1RTMIZLU4ERIC1Y6UY558EVOE70
sign: 57ed50324ef9945fa6d717e81db7386128b029b9215383fa13d44902688efd14
timestamp: 1542619379759 
language: zh-cn
timezone: +8
appKey: f50c76fbc8271d361e1f6b5973f54585
Content-Encoding: utf-8
Content-type: application/json
Body
{
	"ruleActionId":"071099dacbee41eeab4a0375ca2b0688",
     "pushMessage":{
        "messagePush":{
            "pushType":"family_device",
            "pushContent":{
                "msgName":"",
                "expires":"86400",
                "msgTitle":"危险",
                "msgContent":"海尔空小调：咳咳咳～。"
            },
            "showTypes":{
                "01":"3",
                "02":"3"
            },
            "msgStrategy":"null"
        },
        "deviceControl":{
            "args":"null",
            "controlBtnText":"启动空调",
            "operation":{
                "id":"f313ddbcb49d11e798b8fa163eb273a5_0",
                "value":"null",
                "desc":"空调参数设置(组命令)_测试组命令24",
                "required":"false"
            },
            "componentId":"4e8a0dfb038d40fab6c5b49369240000"
        }
    }
}
```  

**请求应答**

```java
{
	"retCode": "00000",
	"retInfo": "成功",
	"data": null
}
```

##### 3、错误码  
> 见 快速开始——常用信息——平台公共错误码





#### 修改条件描述
>修改条件描述。

##### 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/updateRuleConditionsDescribe`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| ruleConditionId| String |32| Body| 必填|规则条件id|  
| desc| String |512| Body| 必填|描述 |      

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  | Object| Body  |  必填 |显示为：null  |



##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 3.3.0
clientId: 123456
sequenceId: 20161020153428000015
accessToken: TGT1RTMIZLU4ERIC1Y6UY558EVOE70
sign: 8bf804aa13f267a87ea4f927270290c5005a09a7edbabd0f84bfed79c4441325
timestamp: 1542619548528 
language: zh-cn
timezone: +8
appKey: f50c76fbc8271d361e1f6b5973f54585
Content-Encoding: utf-8
Content-type: application/json
Body
{
  "ruleConditionId": "8ad397d15ba74186932b2102c94006d9",
  "desc": "当前室内温度 >= 28℃jia123"
}
```  

**请求应答**

```java
{
	"retCode": "00000",
	"retInfo": "成功",
	"data": null
}
```

##### 3、错误码  
> 见 快速开始——常用信息——平台公共错误码 






#### 修改动作描述
>修改动作描述。

##### 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/updateRuleActionDescribe`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| ruleActionId| String |32| Body| 必填|规则行为Id|  
| desc| String |512| Body| 必填|描述 |      

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  | Object| Body  |  必填 |显示为：null  |

##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 3.3.0
clientId: 123456
sequenceId: 20161020153428000015
accessToken: TGT1RTMIZLU4ERIC1Y6UY558EVOE70
sign: c2a733ed66f5c4435d32d7677dd28ab93df9cabb2bcd7cd26f8e76ed2ac5ba7d
timestamp: 1542619708199 
language: zh-cn
timezone: +8
appKey: f50c76fbc8271d361e1f6b5973f54585
Content-Encoding: utf-8
Content-type: application/json
Body
{
  "ruleActionId": "a0b2568f2c794ec4a7ec7b162029d854",
  "desc": "DESCRIPTION_jiayk11"
}
```  

**请求应答**

```java
{
	"retCode": "00000",
	"retInfo": "成功",
	"data": null
}
```

##### 3、错误码  
> 见 快速开始——常用信息——平台公共错误码 





#### 查询支持的关系表达式
>查询支持的关系表达式。

##### 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/getRelationOperator`  
 **HTTP Method：** POST

**输入参数**  

标准输入，无输入参数。    

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  | JSON| Body  |  必填 |详见下方 |

data字段说明：</br>
```  
[{
	"value": "greaterThan",
	"desc": "高于",
	"required": false
},
{
	"value": "greaterThanEqual",
	"desc": "高于或等于",
	"required": false
},
{
	"value": "equal",
	"desc": "等于",
	"required": false
},
{
	"value": "lessThan",
	"desc": "低于",
	"required": false
},
{
	"value": "lessThanEqual",
	"desc": "低于或等于",
	"required": false
}]
```

##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 3.3.0
clientId: 123456
sequenceId: 20161020153428000015
accessToken: TGT1RTMIZLU4ERIC1Y6UY558EVOE70
sign: 1ef2c702773e4da4e8e7994478f97d4d6313012c66f6437bae4f25bc8fb13980
timestamp: 1542619838663 
language: zh-cn
timezone: +8
appKey: f50c76fbc8271d361e1f6b5973f54585
Content-Encoding: utf-8
Content-type: application/json
Body
{}
```  

**请求应答**

```java
{
	"retCode": "00000",
	"retInfo": "成功",
	"data": [{
		"value": "greaterThan",
		"desc": "大于",
		"required": false
	},
	{
		"value": "greaterThanEqual",
		"desc": "大于等于",
		"required": false
	},
	{
		"value": "equal",
		"desc": "等于",
		"required": false
	},
	{
		"value": "lessThan",
		"desc": "小于",
		"required": false
	},
	{
		"value": "lessThanEqual",
		"desc": "小于等于",
		"required": false
	}]
}
```

##### 3、错误码  
> 见 快速开始——常用信息——平台公共错误码 




#### 获取场景最新操作日志
>APP下拉刷新，获取家庭下场景的操作日志。

##### 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/getNewOperationLog`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| familyId| String |32| Body| 必填|家庭Id|  
| limit| Int |N/A| Body| 必填|查询记录数 取值范围: 10-100 | 
     

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  | List< SceneOperationLogDto >| Body  |  必填 |返回家庭下场景的操作记录列表，根据记录产生的时间倒序排列  |

##### 2、请求样例  

**用户请求**
```java  
Header：
appId: MB-****-0000
appVersion: 3.3.0
clientId: 123456
sequenceId: 20161020153428000015
accessToken: TGT1RTMIZLU4ERIC1Y6UY558EVOE70
sign: a9a246405762255773e6d569ad65c745e265e783fafce1c880af475f809cbe12
timestamp: 1542620093937 
language: zh-cn
timezone: +8
appKey: f50c76fbc8271d361e1f6b5973f54585
Content-Encoding: utf-8
Content-type: application/json
Body
{"familyId":"385062139898000000","limit":50}
```  

**请求应答**

```java
{
	"retCode": "00000",
	"retInfo": "成功",
	"data": [{
		"id": 17074,
		"sceneId": "98c2af1901b84eb1b4be99cedb53e96b",
		"sceneName": "回家模式",
		"sceneDesc": "回家后可一键实现开灯、关窗帘、安防设备撤防、打开空调等。也可通过语音来实现上述功能。",
		"familyId": "385062139898000000",
		"operationType": 3,
		"operationStatus": false,
		"operationResult": "执行成功",
		"time": "1542352332000",
		"sn": "a2dfa747-9bdc-4816-b961-046e876e15a2",
		"status": 1
	},
	{
		"id": 17045,
		"sceneId": "98c2af1901b84eb1b4be99cedb53e96b",
		"sceneName": "回家模式",
		"sceneDesc": "回家后可一键实现开灯、关窗帘、安防设备撤防、打开空调等。也可通过语音来实现上述功能。",
		"familyId": "385062139898000000",
		"operationType": 3,
		"operationStatus": false,
		"operationResult": "执行成功",
		"time": "1541833930000",
		"sn": "71851546-6827-437d-adf2-6fb492e4cff5",
		"status": 1
	},
	{
		"id": 17042,
		"sceneId": "98c2af1901b84eb1b4be99cedb53e96b",
		"sceneName": "回家模式",
		"sceneDesc": "回家后可一键实现开灯、关窗帘、安防设备撤防、打开空调等。也可通过语音来实现上述功能。",
		"familyId": "385062139898000000",
		"operationType": 3,
		"operationStatus": false,
		"operationResult": "执行成功",
		"time": "1541747530000",
		"sn": "5a0ed6e4-795c-468d-8c72-c58b3909c567",
		"status": 1
	},
	{
		"id": 17003,
		"sceneId": "98c2af1901b84eb1b4be99cedb53e96b",
		"sceneName": "回家模式",
		"sceneDesc": "回家后可一键实现开灯、关窗帘、安防设备撤防、打开空调等。也可通过语音来实现上述功能。",
		"familyId": "385062139898000000",
		"operationType": 3,
		"operationStatus": false,
		"operationResult": "执行成功",
		"time": "1541229129000",
		"sn": "0baf1355-9a8c-4934-9ccd-286440fd7367",
		"status": 1
	},
	{
		"id": 17000,
		"sceneId": "98c2af1901b84eb1b4be99cedb53e96b",
		"sceneName": "回家模式",
		"sceneDesc": "回家后可一键实现开灯、关窗帘、安防设备撤防、打开空调等。也可通过语音来实现上述功能。",
		"familyId": "385062139898000000",
		"operationType": 3,
		"operationStatus": false,
		"operationResult": "执行成功",
		"time": "1541142729000",
		"sn": "d79e20f6-5321-4bca-9093-c24ef49094da",
		"status": 1
	}]
}
```

##### 3、错误码  
> 见 快速开始——常用信息——平台公共错误码 





#### 获取场景历史操作日志
>上拉加载，获取家庭下场景的操作日志</br>APP下拉刷新，获取家庭下场景的执行日志，精确到动作级别。

##### 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/getHistoryOperationLog`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| familyId| String |32| Body| 必填|家庭Id|  
| number| Long |N/A| Body| 查询此number(不包括这一条) 之前的limit条数据，如果满足条件的数据条数不足limit条也正常返回； | 
| limit| Int |N/A| Body| 必填|查询记录数 取值范围: 10-100 | 
     

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  | List< SceneOperationLogDto>| Body  |  显示家庭下场景的操作记录列表，根据记录产生的时间倒序排列 |


#### 通过来源（模板id）查询家庭下已下载的场景列表
>通过来源(模板id)获取家庭下场景列表（通过模板下载的场景）。

##### 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/listInFamilyBySource`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| familyId| String |32| Body| 必填|家庭Id|  
| sourceId| String |32| Body| 必填|场景来源id(模板id) | 
     

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  | List<UserSceneDto>| Body  |必填| 显示场景的描述性信息, 其中的规则rules中带有规则Id和规则名称,其他字段不返回，同时记录按照创建时间倒序|




### 家庭配置类
> API接口总览

| API名称        | 作用          | 是否开放  | 特别说明|
| ------------- |:-------------:|:-----:|:-------------:|
| 判断该场景下规则的条件以及动作等是否正确 | 根据sceneId判断该场景下规则的条件以及动作等是否正确 | 是| 无|  
| 查询规则设置的参数| 查询条件或者行为中用户保存的值 | 是| 无|  
| 根据场景模板Id保存规则参数信息 | 根据场景模板Id保存规则参数信息 | 是| 无|  
| 批量更新规则的参数| 批量更新规则的参数| 是| 无|  


#### 判断该场景下规则的条件以及动作等是否正确
>根据场景id判断该场景下规则的条件以及动作等是否正确，返回Map<String,List<String>> ,key 为规则id,value为校验符合要求,可以开启的动作集合。

##### 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/ruleJudgement`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| sourceId| String |&emsp;| Body|必填 |&emsp; | 
     

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  ruleMap  | LMap<String,List<String>>| Body  |必填| &emsp;|



#### 查询规则设置的参数
>根据条件Id或者行为Id查询用户之前保存的设备或者需要用户填写的数据。
##### 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/rule/settings`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| familyId| String |32| Body| 必填|家庭Id | 
| ids| String[] |32| Body| 必填|条件或者行为的Id数组，只能是一种，不能混合查询；具体类型由type区分 | 
| type| String |255| Body| 必填|条件还是行为，枚举：condition或者action| 
     

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  Data  | Map<String,RuleSettings>| Body  |必填|返回设置的参数 Key为条件或者行为的Id Value为具体的RuleSettings|



#### 根据场景模板Id保存规则参数信息
>根据场景模板Id下载场景,并且更新下载后的场景的规则参数信息。
##### 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/saveTemplate`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| familyId| String |32| Body| 必填|家庭Id | 
| templateId| String[] |32| Body| 必填|场景模板Id | 
| ruleSettings| RuleSettings[] |N/A| Body| 必填|type:必填，枚举类型可选值condition或者action；</br>id：模板的条件或者行为Id，取决于type；数据结构定义：</br>`{`</br>`“mac”:”A123456”,`</br>`“clazz”:”00123”  //设备大类加中类；这个必须跟海极网一致`</br>`}`</br>如果同一型号需要传入多个设备，数据格式如下：</br>`{"mac":"mac1,mac2,mac3","clazz":"02012"}`</br>和V2.3不一样的是去除了wifitype</br>value:选填 数据结构定义：</br>`{`</br>` “value”:”open”, //具体的值；`</br>`“desc”:”开机”   //具体值的描述`</br>`}`|   
   

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  sceneId  |String| Body  |必填|返回保存下载后场景更新规则参数成功的场景Id|



#### 批量更新规则的参数
>批量更新规则详情 说明：如果需要更新的场景是手动触发类（例如，一键离家）那么每次更新规则系统都会判断当前更新后的场景是否满足实例化条件，如果满足则立即实例化并且场景的isOpen为true表示场景已经开启满足触发要求；如果是开关类场景，则只会更新最新场景的数据不会进行实例化，如果需要实例化需要APP调用手动开启场景接口。

##### 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/rule/updateSettings`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| settings| RuleSettings[] |N/A| Body| 必填|如果同一型号需要传入多个设备，数据格式如下：`{"mac":"mac1,mac2,mac3","clazz":"02012"}`和V2.3不一样的是去除了wifitype| 
| familyId| String[] |32| Body| 必填|&emsp;| 

     

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  | Object| Body  |必填|如果成功返回success；如果失败返回错误信息|


### 触发类
> API接口总览

| API名称        | 作用          | 是否开放  | 特别说明|
| ------------- |:-------------:|:-----:|:-------------:|
| 开启或关闭场景 | 激活场景或者去消息激活 | 是| 无|  
| 开启或关闭用户场景| 开启或关闭用户场景 | 是| 无|  
| 手动执行场景 | 手动执行场景 | 是| 无|  
| 手动执行用户场景| 手动执行用户场景| 是| 无|  



#### 开启或关闭场景
>激活场景或者去消息激活。
##### 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/operation`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| familyId| String |32| Body| 必填|家庭Id | 
| sceneId| String |32| Body| 必填|&emsp; | 
| status| String |32| Body| 必填|目前可取值 0：取消激活；1：激活场景| 
     

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  | Object| Body  |必填|显示为：null|




#### 开启或关闭用户场景
>开启或关闭用户场景。
##### 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/operationUserScene`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| familyId| String |32| Body| 必填|家庭Id | 
| sceneId| String |32| Body| 必填|&emsp; | 
| status| String |32| Body| 必填|目前可取值 0：取消激活；1：激活场景| 
     

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  | Object| Body  |必填|显示为：null|



#### 手动执行场景
>手动执行场景。
##### 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/triggerScene`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| familyId| String |32| Body| 必填|家庭Id | 
| sceneId| String |32| Body| 必填|&emsp; | 

     

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  | Object| Body  |必填|显示为：null|


#### 手动执行用户场景
>手动执行用户场景。
##### 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/triggerUserScene `  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| familyId| String |32| Body| 必填|家庭Id | 
| sceneId| String |32| Body| 必填|&emsp; | 

     

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  | Object| Body  |必填|显示为：null|



### 定时类
> API接口总览

| API名称        | 作用          | 是否开放  | 特别说明|
| ------------- |:-------------:|:-----:|:-------------:|
| 用户配置场景定时任务 | 用户配置场景定时任务 | 是| 无|  
| 用户配置场景定时功能| 用户配置场景定时功能 | 是| 无|  
| 开启或关闭定时器 | 开启或关闭场景定时器 | 是| 无|  
| 用户删除场景的定时功能| 删除场景对应的定时功能| 是| 无|  


#### 用户配置场景定时任务
>用户场景绑定定时器，主要支持quartz表达式，最低支持到分钟级别。说明：</br>1、支持手动触发类场景；</br>2、支持平台触发类场景；</br>3、支持定时触发类场景；</br>4、场景开启或关闭状态都可以设置定时功能。

##### 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/bindingTaskInfo`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| sceneId| String |32| Body| 必填|&emsp; | 
| taskInfo| TaskInfoDto |N/A| Body| 必填|定时任务详情，包括：</br>type:定时策略类型；（必填）</br>cornDto，定时表达式；（必填）</br>activeBeginTime，场景生效开始时间；</br>activeEndTime,场景生效结束时间（定时类场景或者手动触发类场景无生失效时间段设置，但是必须有cron表达式，平台类场景的cron表达式和生失效时间同时必须传）</br>status：定时状态开关（必填，如果没有，默认false，不报错）|
    

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  | Object| Body  |必填|显示为：null|





#### 用户配置场景定时功能
>用户场景绑定定时器，主要支持quartz表达式，最低支持到分钟级别。说明：</br>1、支持手动触发类场景；</br>2、支持平台触发类场景；</br>3、支持定时触发类场景；</br>4、场景开启或关闭状态都可以设置定时功能。



##### 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/bindingTimer`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| id| String |32| Body| 必填|场景Id| 
| cron| Cron |10| Body| 必填|Quartz表达式最低支持到分钟|
| status| Boolean |N/A| Body| 非必填|定时任务开关状态，默认为启用|    

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  | Object| Body  |必填|显示为：null|



#### 开启或关闭定时器
>支持用户场景(平台、手动和定时触发类)调用该接口, 场景开启或关闭状态都可以设置(开启或关闭)定时器。



##### 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/operationTimer`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| sceneId| String |32| Body| 必填|场景Id| 
| familyId| String |32| Body| 必填|家庭Id|
| status| Boolean |N/A| Body| 必填|true:开启定时器</br>false:关闭定时器|    


**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  | Object| Body  |必填|显示为：null|




#### 用户删除场景的定时功能
>删除场景对应的定时功能（支持平台、手动和定时触发类场景），场景开启或关闭状态都可以删除定时功能。



##### 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/task/delete`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| id| String |N/A| Body| 必填|用户场景Id| 
 


**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  | Object| Body  |必填|显示为：null|   

#### 更新多生失效时间段接口
>更新多个生失效时间段功能（支持平台），场景开启或关闭状态都可以更新。  


##### 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/updateTaskInfoList`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| sceneId| String |N/A| Body| 必填|用户场景Id|  
| taskInfoList| List<TaskInfoDto> |5| Body| 必填|需要更新的生失效时间段| 
 


**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  | String| Body  |必填|&emsp;|



## 场景Portal

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
Data|Object|Body|必填|应用场景相关标签列表

#### 根据应用标识查询标签列表（v2.4）

##### 1、接口定义
?> **接入地址：** `/iftttscene/scene/store/getSceneTagListByAppId`</br>
**HTTP Method：** POST

**输入参数** ： 无

**输出参数:** 

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
Data|Object|Body|必填|标签列表


#### 根据应用标识查询分类列表（v2.4）

##### 1、接口定义
?> **接入地址：** `/iftttscene/scene/store/getSceneSortListByAppId`</br>
**HTTP Method：** POST

**输入参数** ： 无

**输出参数:** 

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
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
data|funsionsDto|Body|必填|

data字段说明：
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


data字段说明：
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
data|PropOfComponentDto|Body|必填|

data字段说明：
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

#### 据根据中类组件属性ID列表批量查询属性信息


##### 1、接口定义
?> **接入地址：** `/iftttscene/component/getPropByIds`</br>
**HTTP Method：** POST

**输入参数** 

参数名|类型|最大长度|位置|必填|说明
:-|:-:|:-:|:-:|:-:|:-
propIds|List||Body|必填|组件属性id


**输出参数:** 

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
data|PropOfComponentDto|Body|必填|

data字段说明：
```
[
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
}，
{
"id":"0021ba83f2314d0b9b9702791193b22d",
"propName":"grSetTank",
"propClass":"group",
"functionName":"grSetTank",
"functionDesc":"浴缸设置",
"propType":"GROUP",
"readable":false,
"writable":true,
"description":"浴缸设置",
"splitFunc":0
},
]

```




[^-^]:常用图片注释
[IFTTT_type]:_media/_IFTTT/IFTTT_type.png
[IFTTT_liucheng]:_media/_IFTTT/IFTTT_liucheng.png


