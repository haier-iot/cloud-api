
# 设备场景管理

!> **更新时间**：{docsify-updated}  


## 根据设备mac查询设备功能

**使用说明**

>查询当前家庭下设备的功能信息
>
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



## 查询非设备类组件信息

**使用说明**

>查询非设备类组件信息主要有定时组件、天气组件、延时组件、地理围栏组件


**接口描述**

?> **接入地 址：**  `/iftttscene/scene/find/component-info`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| type| String |N/A| Body| 必填|组件类型</br>WEATHER：天气；</br>TIMER：定时；</br>DELAY： 延时；</br>GEOFENCE： 地理围栏</br>| 


**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  |String| Body  |必填| &emsp;|
|  retInfo  |String| Body  |必填| &emsp;|
|  data  |ComponentDto| Body  |必填|组件信息|

