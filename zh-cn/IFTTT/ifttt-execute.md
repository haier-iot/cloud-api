
# 场景执行
!> **更新时间**：{docsify-updated}  




## 领域模型执行动作后回调执行结果

**使用说明**

>领域模型执行动作后回调返回执行结果。 


**接口描述**

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



## 洗烘联动执行后回调执行结果

**使用说明**

>洗烘联动执行动作后回调返回执行结果。 


**接口描述**

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
 

## 开启或关闭用户场景

**使用说明**

>开启或关闭用户场景。
**接口描述**

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



## 手动执行用户场景

**使用说明**


>手动执行用户场景。


**接口描述**

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


## 手动执行用户场景带返回值

**使用说明**

>手动执行用户场景，返回sn，执行请求唯一标识。


**接口描述**

?> **接入地 址：**  `/iftttscene/scene/v2/triggerUserScene `  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| familyId| String |32| Body| 必填|&emsp; | 
| sceneId| String |32| Body| 必填|&emsp; | 

     

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  data  | Object| Body  |必填|返回场景执行唯一标识sn|

