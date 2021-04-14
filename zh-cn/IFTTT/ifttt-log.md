
# 组件类
!> **更新时间**：{docsify-updated}  



## 组件ID查询家庭场景列表
>通过组件ID和家庭ID查询家庭下的场景列表。 


 1、接口定义

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

