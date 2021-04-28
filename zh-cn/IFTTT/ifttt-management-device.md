
# 设备场景管理

!> **更新时间**：{docsify-updated}  


## 根据设备mac查询设备功能

**使用说明**

>查询当前家庭下设备的功能信息
**接口描述**

?> **接入地 址：**  `/iftttscene/scene/find/device-functions`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| familyId| String |32| Body| 必填|家庭Id | 
| deviceId| String |N/A| Body| 必填|设备id  | 

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  |String| Body  |必填| &emsp;|
|  retInfo  |String| Body  |必填| &emsp;|
|  data  |FunctionsDto| Body  |必填|&emsp;|



