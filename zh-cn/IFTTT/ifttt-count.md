# 回调类

!> **更新时间**：{docsify-updated}  




## 领域模型执行动作后回调执行结果
>领域模型执行动作后回调返回执行结果。 


 1、接口定义

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
>洗烘联动执行动作后回调返回执行结果。 


 1、接口定义

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
 