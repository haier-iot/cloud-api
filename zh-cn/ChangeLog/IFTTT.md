
##  2018-11-02

!> **uws场景引擎服务 V2.6.1**：  
 
- [x]  【新增】新增公共结构ActionResultDto动作执行结果信息；
- [x]  【新增】公共结构SceneOperationLogDto新增字段status（场景状态）、triggerType（场景类型）、actionResultList（场景动作执行信息）
影响接口如下:</br>获取场景最新操作日志: `/iftttscene/scene/getNewOperationLog`</br>获取场景历史操作日志:`/iftttscene/scene/getHistoryOperationLog`  

##  2018-11-24
!> **场景Portal**：  
- [x]  【新增】新增根据中类组件属性ID列表批量查询属性信息接口；  

##  2019-02-28

!> **uws场景引擎服务 V2.7.1**：  
 
- [x]  【新增】接口公共部分修改version默认值为0.3；
- [x]  【新增】场景类接口公共结构SceneDto删除taskInfo字段，新增taskInfoList字段；     
- [x]  【新增】场景类接口公共结构UserSceneDto删除taskInfo字段，新增taskInfoList字段；  
- [x]  【新增】新增更新多个生失效时间段接口；  
- [x]  【修改】修改公共结构数据RuleSettings、UserSceneDto、RuleThenDeviceControl；   
- [x]  【修改】修改查询支持的关系表达式接口； 



