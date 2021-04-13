
# 定时类
!> **更新时间**：{docsify-updated}  



## 更新多生失效时间段接口
>更新多个生失效时间段功能（支持平台），场景开启或关闭状态都可以更新。  


 1、接口定义

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
|  retCode  |String| Body  |必填| &emsp;|
|  retInfo  |String| Body  |必填| &emsp;|
|  data  | Object| Body  |必填|显示为null|
