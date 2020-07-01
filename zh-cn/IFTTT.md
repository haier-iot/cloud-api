
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
|  createUserLogo |  String  |   场景开发者展示LOGO  |最大长度256 场景开发者在海极网配置的logo信息PNG或JPG格式，128x128或64x64规则 用户自建场景忽略|
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
|desc           | String              |条件描述                                                                                                               |  目前是程序组装，只有叶子节点条件才会组装，上层结构不会 用户自建场景忽略                                                                                       |
|key            | StatusDesc          |标准模型name；OS需要录入StatusDesc的ID；标识某个组件的属性ID；OS查询条件返回组件属性的描述（值和描述，还有组件属性的ID）| 选填；                                                                                                                                                        |
|operationSign  | OperationSign       |关系运算符（枚举类型）                                                                                                 |  选填；取值如下：</br>`greaterThan(">"),`</br>`greaterThanEqual(">="),`</br>`equal("=="),`</br>`lessThan("<"), `</br>`lessThanEqual("<="),`</br>`unequal(“!=)` |
|value          | StatusDesc          |标准模型值 |  选填；  |                                                                                                                                                                                                                                                                 
|logicalSign    | LogicalSign         |逻辑运算符（枚举类型）|该条件对应上一个平行条件的逻辑运算，取值如下</br>`AND(”&&”) OR("`&#124;&#124;`")` |                                                                                                                                                                                      
|conditions     | RuleWhenCondition[] |子条件对象数组|选填；说明：举例</br>`if(a`&#124;&#124;`b)` 这种格式那么`conditions`为`null`；</br>除了`logicalSign`其它必填；</br>`if((a`&#124;&#124;`b)&&(c`&#124;&#124;`d))`这种格式则`conditions`必填；</br>其它都为`null`；      |                                                                                                        
|componentId    | String              |所属组件Id  |  必填；由组件区分是设备组件还是其它组件   |          
|componentType    | String              |组件类型  | 选填； |  
|settings    | RuleSettingsDto              |条件配置的参数  |  该dto里面参数为：</br>Id:</br>Object: 设置的设备信息或者天气信息</br>Value: 设置的值|                                                                                                                                                                                                                                                                                                                 



### RuleThenDto  
规则触发动作  
  
| **名称** | 规则触发动作 |&emsp;| RuleThenDto |   
| ------------- |:----------:|:-----:|:--------:|
|**字段名**|**类型**|**说明**|**备注**|  
|actions|RuleThenAction[]|需要触发的动作     |选填 |
|desc      |String             |表示when字段描述 |选填；用户自建场景忽略         |

### RuleThenAction  
规则触发动作详细描述  
  
| **名称** | 规则触发动作详细描述 |&emsp;| RuleThenAction |   
| ------------- |:----------:|:-----:|:--------:|
|**字段名**|**类型**|**说明**|**备注**|  
|id          |String                |行为ID                   |主键ID      UUID|                                                                              
|desc        |String                |行为动作描述             |选填|
|type        |ActionEnum            |需要触发的动作类型枚举类 |取值如下：DeviceControl：设备控制 MessagePush：消息推送 MessagePushWithControl：带小循环控制的消息|
|control     |RuleThenDeviceControl |设备控制动作             |选填；如果type值为：DeviceControl control必填，pushMessage为空；如果type为MessagePushWithControl 则control和pushMessage都必填|
|pushMessage |RuleThenPushMessage   |消息推送动作             |选填；如果选择消息推送则control为空，pushMessage必填|
|delayControl      |RuleThenDelayControl |延时动作控制 |type 为Delayed：延时</br>延时动作结构体|
|newMessage      |RuleThenNewMessage               |新版消息动作         |选填：如果type为NewMessagePush，newMessage必填|
|isOpen      |Integer               |动作片段是否开启         |1开启，0关闭|
|settings      |RuleSettingsDto   |动作配置的参数   |该dto里面参数为：</br>Id: settings主键id</br>Object: 设置的设备信息</br>Value: 设置的值|
|sceneControl      |SceneControl   |控制场景  |选填：如果type为TriggerScene：执行场景OperationScene：开关场景 必填|


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

### RuleThenNewMessage   
新版消息推送动作  
  
| **名称** | 消息推送动作 |&emsp;| RuleThenNewMessage | 
| ------------- |:----------:|:-----:|:--------:|
|**字段名**|**类型**|**说明**|**备注**|  
|pushType    | PushType   |推送类型 ，枚举 |必填;取值：</br>按照家庭推送：family|
|msgTitle | String       |消息标题 |必填|
|msgContent   |String|消息内容    |必填|
|showTypes | int   |终端显示类型 |实时消息的显示样式：</br>-1：不显示，app一般用于无UI展示，直接处理消息内容</br>0： toast</br>2： 弹框，业务事件及button 按钮自定义，见 btns 封装。</br>4： 红色感叹号显示 ;</br>20：弹框，无按钮；</br>21：弹框,一个按钮，</br>无相应业务事件处理；</br>22：弹框，确定、取消</br>两个按钮，均无业务事件处理|
|msgName    | String           |消息名称      |&emsp;|
|expires    | int           |消息过期时间      |消息在客户端离线时在第三方推送平台缓存时间，过期将不再推送给客户端。单位为秒，最长86400秒，如未指定则默认为86400秒。|
|priority    | int           |消息优先级     |消息优先级别，如设置则默认为1。取值为：</br>0 ：紧急消息，一般需要唤醒屏幕，播放消息提示音</br>1：一般消息，在屏幕黑屏时候，播放消息提示音，无需唤醒屏幕</br>2：中低消息，无需声音提示</br>3： 低级此类消息，可能接收消息后，APP 无需立刻处理，等系统空闲或者 wifi 状态下处理即可|
|msgStrategy    | MessageStrategy           |消息发送策略      |选填；|


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
|object        |StatusDesc         |动作载体 为设备信息，value为设备mac；desc为设备昵称|删除，请用settingDto结构 |
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

### RuleThenDelayControl
延时动作控制结构体
  
| **名称** | 延时动作控制结构体 |&emsp;| RuleThenDelayControl |   
| ------------- |:----------:|:-----:|:--------:| 
|**字段名**|**类型**|**说明**|**备注**|   
|args  |DelayControlArgs[] |延时动作集合     |选填；如果为组命名则args有可能为空|
|componentId |String     |组件id |必填；|


### DelayControlArgs
设备控制命令参数
  
| **名称** | 延时控制参数 |&emsp;| DelayControlArgs |   
| ------------- |:----------:|:-----:|:--------:| 
|**字段名**|**类型**|**说明**|**备注**|   
|name  |StatusDesc |名称     |必填{"id":"111111","value":"30","required":false,"desc":"延时"}|
|value |StatusDesc     |值 |选填；原始值放入value属性|


### DeviceControlArgs  
设备控制命令参数  
  
| **名称** | 设备控制命令参数 |&emsp;| DeviceControlArgs |   
| ------------- |:----------:|:-----:|:--------:| 
|**字段名**|**类型**|**说明**|**备注**|   
|name  |StatusDesc |控制标准模型名称     |必填        |
|value |StatusDesc    |控制标准模型值 |选填；原始值放入value属性，递增递减值放入changeValue |
|changeValue|StatusDesc |标准模型的改变值（增加或减少相应单位量）|选填；原始值放入value属性，递增递减值放入changeValue |


### SceneControl  
场景控制参数结构体  
  
| **名称** | 设备控制动作 |&emsp;| SceneControl |   
| ------------- |:----------:|:-----:|:--------:| 
|**字段名**|**类型**|**说明**|**备注**|   
|familyId  |String |家庭id     |必填        |
|sceneId |String     |场景id |必填；|
|sceneName|String |场景名称|选填 |
|status|String |状态|选填：开关类场景必填</br>取值 </br>0：关闭场景</br>1：开启场景|


### ComponentDeviceDesc  
设备描述  
  
| **名称** | 设备描述 |&emsp;| RuleDeviceDesc |   
| ------------- |:----------:|:-----:|:--------:| 
|**字段名**|**类型**|**说明**|**备注**|   
|id  |String |设备主键     |必填        |
|bigClass |StatusDesc     |设备大类 |必填|
|middleClass|StatusDesc |设备中类|必填 |


### ComponentDto  
组件信息  
  
| **名称** | 组件信息 |&emsp;| ComponentDto |   
| ------------- |:----------:|:-----:|:--------:| 
|**字段名**|**类型**|**说明**|**备注**|   
|id  |String |组件主键     |必填        |
|componentName |String     |组件名称 |必填|
|componentDesc|String |组件描述|必填 |
|componentType|ComponentType |组件类型|枚举类型</br>设备组件：</br>中类组件：DEVICE("device")</br>typeId组件：TYPEID（“typeId”）</br>型号组件：MODEL(“model”)</br>|
|deviceDesc|ComponentDeviceDesc |组件所属设备标准模型类型|选填，只有当组件类型为设备组件：DEVICE("device")该属性才有值|
|wifitypeList|String[] |组件支持的wifitype列表|如果该字段为空，则支持中类下所有typeid设备，如果不为空，则组件只支持返回的typeid列表|
|propList|List<PropOfComponentDto> |组件相关的属性列表|选填|




### PropOfComponentDto  
组件下的属性信息  
  
| **名称** | 组件下的属性信息 |&emsp;| PropOfComponentDto |   
| ------------- |:----------:|:-----:|:--------:| 
|**字段名**|**类型**|**说明**|**备注**|   
|id  |String |组件主键     |选填；最大长度32位       |
|propName |String     |组件名称 |必填；硬件属性的标识名称，程序读的|
|propClass|String |属性类别|可取值： property(属性) alarm（告警） operation(操作类属性)，group（组命令） |
|functionName|String |功能标识名称|程序读的|
|description|String |显示名称|选填；人读的|
|functionDesc|String |功能显示名称|人读的|
|propValType|String|属性值类型|选填：可取值：prop_class为property或者为operation时，可取double,int,bool,enum；prop_class为alarm,该字段为null|
|readable|boolean |是否可读|选填|
|writable|boolean |是否可写|选填|
|variants|String |取值范围|存储取值范围的json字符串|



### FunctionsDto  
组件功能列表  
  
| **名称** | 字段取值和描述 |&emsp;| StatusDesc |   
| ------------- |:----------:|:-----:|:--------:| 
|**字段名**|**类型**|**说明**|**备注**|   
|sysProps  |SysPropDto |app用系统属性     |&emsp;       |
|conditions |List<ConditionOrActionDto>     |条件 |&emsp;|
|actions|List<ConditionOrActionDto> |动作|组命令仅用作动作|



### SysPropDto  
app用系统属性 
  
| **名称** | 字段取值和描述 |&emsp;| StatusDesc |   
| ------------- |:----------:|:-----:|:--------:| 
|**字段名**|**类型**|**说明**|**备注**|   
|id  |String |主键     |typeId或型号映射表的主键      |
|componentId |String    |组件ID |&emsp;|
|componentType|ComponentType |组件类型|typeId组件：TYPEID（“typeId”）</br>型号组件：MODEL(“model”)|

### ConditionOrActionDto  
条件或者动作 
  
| **名称** | 字段取值和描述 |&emsp;| StatusDesc |   
| ------------- |:----------:|:-----:|:--------:| 
|**字段名**|**类型**|**说明**|**备注**|   
|propId  |String |主键     |单命令：属性主键；组命令：组命令功能主键|
|desc |String    |描述 |单命令：属性标识显示名称；组命令：组命令功能显示名称|
|fixer|String |属性修饰词|&emsp;|
|props|List< ComponentFunctionPropDto > |功能属性集合|&emsp;|


### ComponentFunctionPropDto  
功能属性信息 
  
| **名称** | 功能属性信息 |&emsp;| ComponentFunctionPropDto |   
| ------------- |:----------:|:-----:|:--------:| 
|**字段名**|**类型**|**说明**|**备注**|   
|propId  |String |属性主键     |&emsp;|
|propName |String    |标识名称 |&emsp;|
|fixer|String |属性修饰词|&emsp;|
|functionName|String |功能标识名称|&emsp;|
|desc|String |属性标识显示名称|&emsp;|
|propValType|String |属性值类型|&emsp;|
|variants|String |取值范围|存储取值范围的json字符串|
|defaultValue|String |默认值|预留字段，不维护任何值|
|defaultValueDesc|String |默认值描述|预留字段，不维护任何值|




### SceneFunctionSupportDto  
场景功能支持对象 
  
| **名称** | 功能属性信息 |&emsp;| SceneFunctionSupportDto |   
| ------------- |:----------:|:-----:|:--------:| 
|**字段名**|**类型**|**说明**|**备注**|   
|model  |String |设备型号     |&emsp;|
|typeId |String    |设备类型 |&emsp;|
|isSupportScene|Boolean |是否支持场景|true : 支持场景</br>false：不支持场景|
|sceneType|Int |支持场景的类型|1. 只支持行为 </br> 2. 只支持动作</br>  3. 既支持行为也支持动作|


### StatusDesc  
场景功能支持对象 
  
| **名称** | 字段取值和描述 |&emsp;| StatusDesc |   
| ------------- |:----------:|:-----:|:--------:| 
|**字段名**|**类型**|**说明**|**备注**|   
|id  |String |属性Id     |选填；如果为条件的Key，则Id表示组件中的某个属性的ID|
|value |String    |字段值 |选填；|
|desc|String |字段值中文描述|选填；|
|scope     |Map<String,String> |取值范围，跟标准同步 |选填；数据结构：</br>`{`</br>`“type”:”enum”,`</br>`“value:”json”`</br>`}`</br>type类型遵循PropOfComponentDto中的PropValType </br>字段描述本结构中value的取值范围，</br>只有在具体的属性中才有值，</br>表示这个属性的取值范围；不在属性的值中出现|
|required|Boolean |属性是否必填|如果为true表示app实例化需要填写此参数|
|room|String |所属房间|只读：设备类所属房间信息|



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
|isOpen |Integer   |表示条件或者动作状态|1： 开启</br>0： 关闭</br>备注：这个表示条件或者动作的开启，本期默认传递1|
|value  |Map<String,String> |条件或者行为中的取值    |选填；数据结构定义：</br>`{`</br>`“value”:”open”, //具体的值；`</br>`“desc”:”开机”   //具体值的描述`</br> `}`</br>类型地理围栏：`{`"desc":"离开","value":"leave"`}`|   
|object |Map<String,String>   |设备参数或者天气参数保存 |必填；表示设备参数或者天气参数 | 
|userSettingGroupPropDtoList |List<UserSettingGroupPropDto>|用户设置的参数 |选填；备注：动作为组命令，用户设置的参数信息 | 
|dealyTime |Integer|延迟时间 |选填；取值为数字 单位为秒 说明该动作需要在触发执行后多少秒后触发如果不填或者0则认为不延迟|  
|sceneId |String   |场景id |选填| 
|componentId |String   |组件ID |只读,为空情况目前非组件消息类型| 
|functionName |String   |功能名称 |只读，MessagePush</br>Delayed：延时</br>NewMessagePush：新版消息推送</br>TriggerScene：执行场景</br>OperationScene：开关场景| 
|status |String   |条件或动作状态 |0：失效；1：正常| 

object字段说明：</br>   
```  
数据结构定义：
{
   “id”: “mac1”,		//存放设备mac或者城市code编码或者cron表达式，或者位置信息
   “name”:”名字” 
}
如果同一型号需要传入多个设备，数据格式如下：
{"mac":"mac1,mac2,mac3","clazz":"02012"}
备注：之前版本存入device字段的信息，现在都存储在该字段，具体结构如上。
{
   “id”: “mac1”,		//存放设备mac或者城市code编码或者cron表达式，或者位置信息
   “name”:”名字” 
}

地理围栏：
{
   “id”: “{"longitude": "经度",
		"latitude": "纬度",
		"radius ": "半径",
		"userId": "用户ID"
}”,	name”:”龙旗科技园” 
}

延时类型：
{“id”: “60”, “name”:”延时1分钟” 
}

消息类型
{
   “id”: “"{"title":"消息标题","content":"消息内容"}"”,
   “name”:”动作消息” 
}
开关场景类型：
{“id”: “场景id”, “name”:”场景名称” 
}
执行场景类型：
{“id”: “场景id”, “name”:”场景名称” 
}


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



### RuleSettingsDto
场景设置的参数信息

| **名称** | 条件对象 |&emsp;| RuleWhenCondition |   
| ------------- |:----------:|:-----:|:-----:|  
|**字段名**|**类型**|**说明**|**备注**|
|id |String   |ruleSttings  |UUID|
|object |StatusDesc   |条件或者动作载体 |使用如RuleWhenCondition或RuleThenAction中object使用方式 |
|status |String   |条件或动作状态 |0：失效;1：正常 |



### BatchResultDto
领域模型回调执行结果

| |  ||  |   
| ------------- |:----------:|:-----:|:-----:|  
|**字段名**|**类型**|**说明**|**备注**|
|oid |long   |数据代理主键  |&emsp;|
|cmdSn |String   |批量指令的总sn |&emsp;|
|cmdSubSn |String   |本条指令的sn |&emsp;|
|execStep |Int   |本条指令的执行步骤 |&emsp;|
|execResult |Boolean   |本条指令本步骤的执行结果 |&emsp;|
|execResultInfo |String   |本条指令本步骤的执行结果说明|&emsp;|
|resultTime |long   |本条指令本步骤的反馈时间 |&emsp;|




### ResultDto
洗烘联动回调执行结果

| |  ||  |   
| ------------- |:----------:|:-----:|:-----:|  
|**字段名**|**类型**|**说明**|**备注**|
|sn |String   |指令的总sn  |&emsp;|
|deviceId |String   |干衣机设备Id |&emsp;|
|context |Map<String, Object>   |上下文信息 |&emsp;|
|execStep |Int   |本条指令的执行步骤 |&emsp;|
|execResult |Boolean   |本条指令本步骤的执行结果 |&emsp;|
|execResultCode |String   |本条指令本步骤的执行结果说明|&emsp;|
|execResultInfo |String   |本条指令本步骤的执行结果说明|&emsp;|
|resultTime |long   |本条指令本步骤的反馈时间 |&emsp;|


## 接口清单

### 场景类

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
>根据用户填写的参数保存场景

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





#### 修改用户平台触发类场景
>修改用户平台触发类场景（传入参数userSceneDto中必须传入需要修改的id,userId不能修改）。
说明：
1、 更新场景只能更新 场景名称、场景描述、场景别名、定时策略、规则、权重、banner、icon，标签、分类本期暂时没有涉及，先不做要求。<br>
2、如果要修改场景的规则信息，必须传入原有场景的ruleId;<br>
3、模板转自定义调用该接口保存平台触发类场景。


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
>修改用户手动触发类场景（传入参数userSceneDto中必须传入需要修改的id,userId不能修改）。
说明：
1、更新场景只能更新 场景名称、场景描述、场景别名、定时策略、规则、权重、banner、icon，标签、分类本期暂时没有涉及，先不做要求。<br>
2、如果要修改场景的规则信息，必须传入原有场景的ruleId;<br>
3、模板转自定义调用该接口保存手动执行类场景。



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




#### 修改用户场景
>修改用户场景（传入参数userSceneDto中必须传入需要修改的id,familyId不能修改）。
说明：
1，	支持场景开启状态下的编辑功能；
2，	不区分场景类型，所有场景均可编辑；
3，	编辑后场景规则ID、条件、动作ID均会重新生成


##### 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/modifyUserScene`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|
| userSceneDto| UserSceneDto |&emsp;| Body| 必填|场景参数 |           
 

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  | Object| String  |  必填 |&nbsp; |  
|  retInfo  | Object| String  |  必填 |&nbsp; |  
|  data  | Object| Body  |  必填 |返回修改成功的场景ID |   








#### 规则类获取规则详情
>根据规则Id查询规则具体信息。返回结构和保存用户场景结构一致。

##### 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/rule/getRuleInfo`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|          
| ruleId| String |32| Body| 必填|规则Id|     

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  | RuleTemplateDto| Body  |  必填 |&emsp;  |






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


#### 查询场景详情
>根据场景id查询场景的基本信息、条件和动作信息。

##### 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/getSceneDetailBySceneId`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| sceneId| String |N/A| Body| 必填|场景id|  
     

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  | UserSceneDto| Body  |必填| &nbsp;|




### 家庭配置类

#### 判断该场景下规则的条件以及动作等是否正确
>根据场景id判断该场景下规则的条件以及动作等是否正确，返回Map<String,List<String>> ,key 为规则id,value为校验符合要求,可以开启的动作集合。

##### 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/ruleJudgement`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| sceneId| String |&emsp;| Body|必填 |&emsp; | 
     

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  |String| Body  |必填| &emsp;|
|  retInfo  |String| Body  |必填| &emsp;|
|  ruleMap  |Map<String,List<String>| Body  |必填| &emsp;|



#### 根据场景模板Id保存规则参数信息
>根据场景模板Id下载场景,并且更新下载后的场景的规则参数信息。
##### 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/saveTemplate`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| familyId| String |32| Body| 必填|家庭Id | 
| templateId| String |32| Body| 必填|场景模板Id | 
| sceneTagList| List<SceneTagDto> |N/A| Body|选填|场景位置标签 |
| ruleSettings| RuleSettings[] |N/A| Body| 必填|type:必填，枚举类型可选值condition或者action；</br>id：模板的条件或者行为Id，取决于type；数据结构定义：</br>`{`</br>`"mac":"A123456",`</br>`"clazz":"00123"  //设备大类加中类；这个必须跟海极网一致`</br>`}`</br>如果同一型号需要传入多个设备，数据格式如下：</br>`{"mac":"mac1,mac2,mac3","clazz":"02012"}`</br>和V2.3不一样的是去除了wifitype</br>value:选填 数据结构定义：</br>`{`</br>` "value":"open", //具体的值；`</br>`"desc":"开机"   //具体值的描述`</br>`}`|   
   

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  |String| Body  |必填| &emsp;|
|  retInfo  |String| Body  |必填| &emsp;|
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
| familyId| String |32| Body| 必填|&emsp;| 

     

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  |String| Body  |必填| &emsp;|
|  retInfo  |String| Body  |必填| &emsp;|
|  data  | Object| Body  |必填|如果成功返回success；如果失败返回错误信息|



#### 获取家庭规则参数设置
>根据familyId和场景id查询家庭规则设置，其中条件或者动作未设置参数(即没有配置设备信息，外部条件信息等)也会返回记录。

##### 1、接口定义
?> **接入地 址：**  `/iftttscene/scene/getRuleSettingsByFamilyId`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| familyId| String |N/A| Body| 必填|家庭Id| 
| sceneIds| String[] |N/A| Body| 选填|场景id数组| 
     

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  |String| Body  |必填| &emsp;|
|  retInfo  |String| Body  |必填| &emsp;|
|  data  | List<RuleSettings>| Body  |必填|显示为null|



#### 手动执行类场景列表查询
>查询家庭下手动执行类场景列表

##### 1、接口定义
?> **接入地 址：**  `/iftttscene/scene/ listManuallySceneByFamily`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| familyId| String |32| Body| 必填|家庭Id| 
| limit| Int |N/A| Body| 必填|每页显示的记录数最多100条，默认30条| 
| cursor| Int |N/A| Body| 必填|从0开始| 
     

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  |String| Body  |必填| &emsp;|
|  retInfo  |String| Body  |必填| &emsp;|
|  data  | Pagination<UserSceneDto >| Body  |必填|1.	显示场景中的描述信息, 其中的规则rules中只显示规则Id和规则名称以及规则描述, 同时记录按照createTime（创建时间）倒序。</br>2.	返回为null的字段被过滤掉，不显示。|


#### 开关类场景列表查询
>平台触发类场景列表查询

##### 1、接口定义
?> **接入地 址：**  `/iftttscene/scene/listPlatformSceneByFamily`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| familyId| String |32| Body| 必填|家庭Id| 
| limit| Int |N/A| Body| 必填|每页显示的记录数最多100条，默认30条| 
| cursor| Int |N/A| Body| 必填|从0开始| 
     

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  |String| Body  |必填| &emsp;|
|  retInfo  |String| Body  |必填| &emsp;|
|  data  | Pagination<UserSceneDto >| Body  |必填|1.	显示场景中的描述信息, 其中的规则rules中只显示规则Id和规则名称以及规则描述, 同时记录按照createTime（创建时间）倒序。</br>2.	返回为null的字段被过滤掉，不显示。|


#### 根据系统id查询场景列表
>根据系统id查询场景列表，进行应用隔离,系统id根据UAG获取区分。

##### 1、接口定义
?> **接入地 址：**  `/iftttscene/scene/listFamilySceneBySystemId`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| familyId| String |32| Body| 必填|家庭Id| 
| limit| Int |N/A| Body| 必填|默认10条| 
| cursor| Int |N/A| Body| 必填|从0开始| 
     

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  |String| Body  |必填| &emsp;|
|  retInfo  |String| Body  |必填| &emsp;|
|  data  | Pagination<UserSceneDto >| Body  |必填|1.	显示场景中的描述信息, 其中的规则rules中只显示规则Id和规则名称以及规则描述, 同时记录按照createTime（创建时间）倒序。</br>2.	返回为null的字段被过滤掉，不显示。|

#### 根据语音标签查询场景
>根据标签查询场景列表

##### 1、接口定义
?> **接入地 址：**  `/iftttscene/scene/ listUserSceneByKeyword`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| familyId| String |32| Body| 必填|家庭Id| 
| sceneVoiceTag| String |32| Body| 必填|&emsp;| 
| limit| Int |N/A| Body| 必填|每页显示的记录数最多100条，默认30条| 
| cursor| Int |N/A| Body| 必填|从0开始| 
     

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  |String| Body  |必填| &emsp;|
|  retInfo  |String| Body  |必填| &emsp;|
|  data  | Pagination<UserSceneDto >| Body  |必填|1.	显示场景中的描述信息, 其中的规则rules中只显示规则Id和规则名称以及规则描述, 同时记录按照createTime（创建时间）倒序。</br>2.	返回为null的字段被过滤掉，不显示。|

#### 根据属性id查询场景
>根据设备id 查询场景列表

##### 1、接口定义
?> **接入地 址：**  `/iftttscene/scene/listSceneBySettings`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| familyId| String |32| Body| 必填|家庭Id| 
| param| Map<String,String> |N/A| Body| 必填|{“id”: “mac”}id值：设备mac值
| 
| limit| Int |N/A| Body| 必填|每页显示的记录数最多100条，默认30条| 
| cursor| Int |N/A| Body| 必填|从0开始| 
     

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  |String| Body  |必填| &emsp;|
|  retInfo  |String| Body  |必填| &emsp;|
|  data  | Pagination<UserSceneDto >| Body  |必填|1.	显示场景中的描述信息, 其中的规则rules中只显示规则Id和规则名称以及规则描述, 同时记录按照createTime（创建时间）倒序。</br>2.	返回为null的字段被过滤掉，不显示。|

#### 查询场景执行状态
>根据场景id 查询场景场景执行状态

##### 1、接口定义
?> **接入地 址：**  `/iftttscene/scene/getOperationLogInfo`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| familyId| String |32| Body| 必填|家庭Id| 
| sceneId| String |32| Body| 必填|场景id
| 
| sequenceid| String |32| Body| 必填|场景执行id| 

     

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  |String| Body  |只读| &emsp;|
|  retInfo  |String| Body  |只读| &emsp;|
|  data  | SceneOperationLogDto| Body  |只读|返回家庭下场景的操作记录|

#### 根据场景名称查询场景
>根据场景名称模糊查询场景列表

##### 1、接口定义
?> **接入地 址：**  `/iftttscene/scene/ listSceneBySceneName`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| familyId| String |32| Body| 必填|家庭Id| 
| sceneName| String |场景名称| Body| 选填|场景名称| 

     

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  |String| Body  |只读| &emsp;|
|  retInfo  |String| Body  |只读| &emsp;|
|  data  | List<UserSceneDto >| Body  |只读|返回为null的字段被过滤掉，不显示|

#### 修改场景描述信息
>修改场景描述信息（场景名称、场景别名、场景描述）,多余参数不要传，防止修改出错。

##### 1、接口定义
?> **接入地 址：**  `/iftttscene/scene/updateSceneDesc`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| familyId| String |32| Body| 必填|家庭Id| 
| userSceneDto| UserSceneDto |&emsp;| Body| 必填|场景参数| 

     

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  |String| Body  |必填| &emsp;|
|  retInfo  |String| Body  |必填| &emsp;|


#### 修改场景标签
>修改场景标签，场景id必须，标签信息必须

##### 1、接口定义
?> **接入地 址：**  `/iftttscene/scene/updateSceneTag`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| sceneId| String |32| Body| 必填|场景id| 
| familyId| String |32| Body| 必填|家庭Id| 
| sceneTagList| List<SceneTagDto> |N/A| Body| 必填|场景标签列表| 

     

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  |String| Body  |必填| &emsp;|
|  retInfo  |String| Body  |必填| &emsp;|

#### 查询家庭下标签信息
>获取家庭下场景标签列表

##### 1、接口定义
?> **接入地 址：**  `/iftttscene/scene/listSceneTagByFamily`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|    
| familyId| String |32| Body| 必填|家庭Id| 
     

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  |String| Body  |必填| &emsp;|
|  retInfo  |String| Body  |必填| &emsp;|
|  tagList  |List<SceneTagDto>| Body  |必填| 用户场景标签|

#### 根据场景模板Id启用场景，优化接口
>根据场景模板Id下载场景,并设置参数，如果该场景是开关类场景则把场景打开。

##### 1、接口定义
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



### 触发类


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


### 组件类


#### 组件ID查询家庭场景列表
>通过组件ID和家庭ID查询家庭下的场景列表。 


##### 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/getSceneListByComponentId`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| componentId| String |N/A| Body| 必填|组件Id|  
| familyId| String |N/A| Body| 必填|家庭Id| 
 


**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  | String| Body  |必填|&emsp;|
|  retInfo  | String| Body  |必填|&emsp;|
|  data  | List<UserSceneDto >| Body  |必填|返回为null的字段被过滤掉，不显示。|


### 回调类


#### 领域模型执行动作后回调执行结果
>领域模型执行动作后回调返回执行结果。 


##### 1、接口定义

?> **接入地 址：**  `/iftttscene/internal/callback/action/{sn}`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|    
| cmdSn| String |N/A| Body| 必填|命令sn|      
| batchResult| BatchResultDto |N/A| Body| 必填|详细|  
| detailInfo| boolean |N/A| Body| 必填|执行结果| 
 


**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  | String| Body  |必填|&emsp;|
|  retInfo  | String| Body  |必填|&emsp;|



#### 洗烘联动执行后回调执行结果
>洗烘联动执行动作后回调返回执行结果。 


##### 1、接口定义

?> **接入地 址：**  `/iftttscene/internal/callback/action/{componentType}/{sn}/{actionSn}`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:| 
| detailInfo| ResultDto |N/A| Body| 必填|命令执行结果| 
 


**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  | String| Body  |必填|&emsp;|
|  retInfo  | String| Body  |必填|&emsp;|

## 场景Portal

### 场景商店类

#### 根据appSceneId查询场景基本信息

>根据appId和appSceneId查询场景。 

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
retCode|String|Body|必填|&nbsp;|
retInfo|String|Body|必填|&nbsp;|
data|SceneDto|Body|必填|应用场景详情, 其中的规则rules中带有规则Id和规则名称以及规则描述

#### 查询应用场景的规则信息

>查询应用场景的规则信息。

##### 1、接口定义
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

#### 判断设备列表是否支持该场景列表

>判断型号列表是否支持该场景。

##### 1、接口定义
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

#### 获取所有场景标签列表

>查询场景所有标签列表

##### 1、接口定义
?> **接入地址：** `/iftttscene/sceneportal/store/getAllSceneTagList`</br>
**HTTP Method：** POST

**输入参数** ： 无

**输出参数:** 

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
Data|SceneTagDto[]|Body|[{"id":1,"name":""}]




### 组件类

#### 查询组件信息

> 查询组件信息(增加业务组件为typeId组件时返回typeid,组件为型号组件时,返回设备型号)。

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
retCode|String|Body|必填|&nbsp;|
retInfo|String|Body|必填|&nbsp;|
data|ComponentDto|Body|必填|&nbsp;|


#### 根据组件id功能id功能值和设备获取支持的型号属性信息

> 根据组件Id、功能id、功能值 和设备(型号产品编码列表) 获取具体设备的属性信息。

##### 1、接口定义
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



####	根据组命令功能名和设备获取具体组功能属性信息

> 根据组功能名称 和设备(型号产品编码列表) 获取具体设备的属性信息。

##### 1、接口定义
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


####	根据设备产品编码查询型号属性列表

> 根据设备型号产品编码，查询型号对应的功能，如果该型号没有相关数据，则返回空设备的功能会按照海极网设备功能配置的顺序输出。

##### 1、接口定义
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



####	查询型号支持场景状态列表

> 通过型号产品编码列表和查询类型查询型号支持场景状态。状态包含否可以作为条件、是否可以作为动作。型号如果不支持场景，则结果集不返回。

##### 1、接口定义
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


####	获取非设备类组件和属性功能列表

> 获取非设备类组件属性功能列表（天气、定时、延时、地理围栏）。

##### 1、接口定义
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




[^-^]:常用图片注释
[IFTTT_type]:_media/_IFTTT/IFTTT_type.png
[IFTTT_liucheng]:_media/_IFTTT/IFTTT_liucheng.png


