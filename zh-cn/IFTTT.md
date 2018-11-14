
<<<<<<< HEAD
>  **当前版本**：[uws场景引擎服务v2.6.1]()    
 **更新时间**：{docsify-updated}  
[更新说明](zh-cn/ChangeLog/IFTTT) 

=======
>>>>>>> 85445214a02c29cdd1f4c6504bb56f86f4cc7192
## 简介
>为家庭添加场景模板，支持应用开发场景模板，并为应用用户提供场景模板，用户可以基于场景模板添加设备，让设备支持家庭下的场景调用；

![场景引擎图片][IFTTT_type]

<<<<<<< HEAD
=======
## 名词解释



## 功能介绍
>>>>>>> 85445214a02c29cdd1f4c6504bb56f86f4cc7192
1、设备间场景调用，设备状态变化触发其他设备执行相关命令；</br>
2、设备状态告警推送，设备状态定义为特定值后给APP用户推送相关定义的告警信息；</br>
3、设备解绑消息，设备必须被拥有者分享到家庭，并且绑定了有效场景，则无论场景是否开启，一旦解绑设备都会推出设备取消分享家庭的消息并含带场景信息；</br>
4、设备取消分享家庭，如果设备已经绑定了有效场景，无论场景是否开启，一旦设备从家庭退出都会推出设备取消分享家庭的消息并含带场景信息；</br>
5、删除家庭成员，删除的家庭成员如果之前绑定了设备，并且已经将绑定的设备分享到家庭中，而且根据这些设备创建了场景，则无论场景是否开启，一旦家庭成员被删除都会推出删除家庭成员的消息并含带场景信息；</br>
6、用户退出家庭操作，退出家庭的用户如果之前绑定了设备，并且已经将绑定的设备分享到家庭中，而且根据这些设备创建了场景，则无论场景是否开启，一旦用户退出家庭都会推出用户退出家庭的消息并含带场景信息；</br>
7、删除家庭操作，根据家庭必须有可用的场景信息，一旦家庭被删除都会推出删除家庭的消息，并含带场景信息；</br>

<<<<<<< HEAD
## 应用场景
本服务适用于家庭组下的场景应用;  
应用接入场景引擎API，并且配合使用消息推送SDK（消息推送类场景需APP内置SDK);  
**自定义模板方式：** 海极网—开通场景引擎功能—场景开发portal定义场景模板—提交审核模板—海极网运营审核通过场景模板—在海极网为APP订阅场景模板—APP集成API开发—上线发布使用；
**使用现成模板方式：** 海极网—开通场景引擎功能—选取已发布上线的场景模板—APP集成API开发—上线发布使用；
=======
<<<<<<< HEAD
=======
>>>>>>> 85445214a02c29cdd1f4c6504bb56f86f4cc7192

>>>>>>> e418d6379dc53412f28256dc96c46f72a7d461f7
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
<<<<<<< HEAD
|familyId| String | 家庭ID |选填；| 
=======
|familyId| String | 家庭ID |选填；|
>>>>>>> e418d6379dc53412f28256dc96c46f72a7d461f7
|type| String| 场景类型|必填；最大长度32 用户自建场景忽略| 
|rules| RuleTemplateDto[] | 规则模板 |必填；可以是多个规则片段组成场景整体规则；一个场景最多支持10个片段；每个片段最多支持25个条件的运算；| 
|auto| boolean | 是否支持自动实例化 |必填；默认为false 用户自建场景忽略 | 
|canAppTrigger| boolean | 是否支持App手动触发执行 |选填，默认是不支持 0:开启后自动执行  1：一键离家| 
|status| String | 基础场景当前状态 |选填；目前只关注三个状态，默认不填为草稿：draft；已发布：publish;已删除:deleted；用户不关注（场景测试app可以根据该字段显示场景模板开发状态）| 
<<<<<<< HEAD
|appId| String | appId |应用标识| 
=======
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
|taskInfo| TaskInfoDto | 定时策略信息 |&emsp;|
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
|taskInfo             |   TaskInfoDto      |  时间策略信息|                                 选填；有俩策略，周期生效（只支持平台触发类），定期执行（只支持手动触发类）如果是定时执行类场景则必填 同一个场景只能支持一个策略|
|aiKeyWord            |   String           |  语音标签|                                     选填；最大长度256|
|banner               |   String           |  场景banner图URL|                              最大长度256 如果场景为下载类型，则来源于场景开发者相关信息，如果为用户自建，则需用户配置该字段；|
|icon                 |   String           |  场景图标URL|                                  选填；最大长度256|
|createUserNickname   |   String           |  场景开发者展示名称|                           最大长度64 场景开发者在海极网填写的昵称信息 用户自建场景忽略|
|createUserLogo       |   String           |  场景开发者展示LOGO|                           最大长度256 场景开发者在海极网配置的logo信息PNG或JPG格式，128*128或64*64规则 用户自建场景忽略|
|engineVersion        |   String           |  引擎执行版本号|                               最大32位，该值为引擎赋值 用户自建场景忽略|
|createType           |   String           |  场景创建方式 Basis：下载方式； User：自建方式|最大16位 用户自建场景忽略|


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
|object         | StatusDesc          |条件载体 如果条件为设备条件则该值为设备信息，value为设备mac；desc为设备昵称                                            |  &emsp;                                                                                                                                                        |
|desc           | String              |条件描述                                                                                                               |  目前是程序组装，只有叶子节点条件才会组装，上层结构不会 用户自建场景忽略                                                                                       |
|key            | StatusDesc          |标准模型name；OS需要录入StatusDesc的ID；标识某个组件的属性ID；OS查询条件返回组件属性的描述（值和描述，还有组件属性的ID）| 选填；                                                                                                                                                        |
|operationSign  | OperationSign       |关系运算符（枚举类型）                                                                                                 |  选填；取值如下：</br>`greaterThan(">"),`</br>`greaterThanEqual(">="),`</br>`equal("=="),`</br>`lessThan("<"), `</br>`lessThanEqual("<="),`</br>`unequal(“!=)` |
|value          | StatusDesc          |标准模型值 |  选填；  |                                                                                                                                                                                                                                                                 
|logicalSign    | LogicalSign         |逻辑运算符（枚举类型）|该条件对应上一个平行条件的逻辑运算，取值如下</br>`AND(”&&”) OR("`&#124;&#124;`")` |                                                                                                                                                                                      
|conditions     | RuleWhenCondition[] |子条件对象数组|选填；说明：举例</br>`if(a`&#124;&#124;`b)` 这种格式那么`conditions`为`null`；</br>除了`logicalSign`其它必填；</br>`if((a`&#124;&#124;`b)&&(c`&#124;&#124;`d))`这种格式则`conditions`必填；</br>其它都为`null`；      |                                                                                                        
|componentId    | String              |所属组件Id  |  必填；由组件区分是设备组件还是其它组件   |                                                                                                                                                                                                                                                                                                                         


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
|desc        |String                |行为动作描述             |必填|
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
|operation     |StatusDesc         |操作名称                                          |  选填；V2.2版本args跟operation选填 单命令控制不需要设置该值；组命令控制需要设置操作名称，|

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
|device|Map<String,String>|具体的设备 |必填；数据结构定义：</br>`{`</br>`“mac”:”A123456”,`</br>`clazz”:”00123”  //设备大类加中类；这个必须跟海极网一致`</br>`}`</br>如果同一型号需要传入多个设备，数据格式如下：</br>`{"mac":"mac1,mac2,mac3","clazz":"02012"}`</br>和V2.3不一样的是去除了wifitype|
|value  |Map<String,String> |条件或者行为中的取值    |选填；数据结构定义：</br>`{`</br>`“value”:”open”, //具体的值；`</br>`“desc”:”开机”   //具体值的描述`</br> `}`|  



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
|sceneDesc       |String¬              |场景描述          |必填；最大长度600                                                                           |
|userId          |String               |用户ID            |选填；最大长度32                                                                            |
|familyId        |String               |家庭ID            |必填；最大长度32                                                                            |
|operationType   |Int                  |操作类型          |必填；</br>1： 开启场景</br>2： 关闭场景</br>3： 触发场景                                   |
|operationStatus |Boolean              |操作状态          |必填；true : 成功   false: 失败                                                             |
|operationResult |String               |操作结果          |必填；最大长度32</br>打开成功 打开失败</br>关闭成功 关闭失败</br>执行成功 执行失败          |
|time            |Long                 |操作时间          |必填；时间戳</br>例如： 1530467982731                                                       |
|sn              |String               |sn号              |选填；默认值为0                                                                             |
|triggerType     |String               |场景类型          |场景类型分为:（必填）</br>开关类:platform</br>执行类:manually                               |
|status          |Int                  |场景状态          |场景分三个状态:（必填）</br>0:成功</br>1:处理中</br>2:失败                                  |
|actionResultList|List<ActionResultDto>|动作执行结果信息  |场景执行失败的原因列表（选填）</br>（执行成功不需要显示执行信息，执行失败列表显示失败原因） |



### ActionResultDto
动作执行结果信息  
  
| **名称** | 动作执行结果信息 |&emsp;| ActionResultDto |   
| ------------- |:----------:|:-----:|:-----:|  
|**字段名**|**类型**|**说明**|**备注**|
|errorCode |String   |错误码 |必填：场景动作执行返回的错误码信息|
|errorMessage |String   |错误信息 |必填; 场景动作执行失败原因 |


>>>>>>> e418d6379dc53412f28256dc96c46f72a7d461f7

## 接口清单

### 场景类
> API接口总览

| API名称        | 作用          | 是否开放  | 特别说明|
| ------------- |:-------------:|:-----:|:-------------:|
| 批量下载场景 | 从场景Store中批量下载场景 | 是| 无|  
| 批量下载基础场景| 从场景Store中下载基础场景 | 是| 无|  
| 批量下载应用场景 | 批量下载应用场景 | 是| 无|  
| 查询家庭下场景列表| 包括用户自建场景和通过模板下载的场景 | 是| 无|  
| 获取家庭下应用场景列表| 需要区分具体的终端(AppId区分) | 是| 无|  
| 根据家庭下的场景Id批量查询场景列表| 根据家庭下的场景Id批量查询场景列表 | 是| 无| 
| 修改基础场景昵称| 修改基础场景昵称 | 是| 无| 
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

<<<<<<< HEAD
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


#### 查询家庭下场景列表
>获取家庭下场景列表（包括用户自建场景和通过模板下载的场景）。


##### 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/listSceneByFamily`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|
| familyId| String |32| Body| 必填|家庭Id|   
| limit| Int|N/A| Body| 必填|默认每次向下滑加载10条数据| 
| cursor| Int|N/A| Body| 必填|从0开始| 

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  |Pagination<SceneDto>| Body  |  必填 |显示场景中的描述信息,其中的规则rules中带有规则Id和规则名称以及规则描述,同时记录按照创建时间倒序|



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


#### 修改基础场景昵称
>修改基础场景昵称。

##### 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/store/updateStoreSceneAlias`  
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


#### 修改场景生效时间段
>修改场景生效时间段。

##### 1、接口定义

?> **接入地 址：**  `/iftttscene/scene/updateAppSceneAlias`  
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
=======
## 开通流程
>>>>>>> 85445214a02c29cdd1f4c6504bb56f86f4cc7192



<<<<<<< HEAD
=======
## 应用场景
本服务适用于家庭组下的场景应用
应用接入场景引擎API，并且配合使用消息推送SDK（消息推送类场景需APP内置SDK）
**自定义模板方式：** 海极网—开通场景引擎功能—场景开发portal定义场景模板—提交审核模板—海极网运营审核通过场景模板—在海极网为APP订阅场景模板—APP集成API开发—上线发布使用；
**使用现成模板方式：** 海极网—开通场景引擎功能—选取已发布上线的场景模板—APP集成API开发—上线发布使用；
>>>>>>> 85445214a02c29cdd1f4c6504bb56f86f4cc7192



[^-^]:## 常见问题



[^-^]:常用图片注释
[IFTTT_type]:_media/_IFTTT/IFTTT_type.png
[IFTTT_liucheng]:_media/_IFTTT/IFTTT_liucheng.png


