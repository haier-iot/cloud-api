# 场景统计

!> **更新时间**：{docsify-updated}  




## 统计场景使用

**使用说明**

>根据家庭id、用户ID查询场景使用信息
统计规则：filter参数为查询的条件，dimL1和dimL2为需要统计的维度。

**接口描述**

?> **接入地 址：**  `/iftttscene/scene/find/user-data`  
 **HTTP Method：** POST

**输入参数**  

| 参数名  | 类型    | 最大长度  |位置  | 必填|说明|
| ------- |:------:|:-----:|:----:|:----:|:----:|         
| filter| List<Map> |N/A| Body| 必填|[{"name": "familyId","value": "123"	},{	"name": "userId","value": "234"	}] | 
| dimL1| List<String> |N/A| Body| 必填|[“paltform”,”manually”] | 
| dimL2| List<String> |N/A| Body| 选填|[“open”,”close”] | 

**输出参数**  

|   名称      |     类型      | 位置  |必填 |说明|
| ------------- |:----------:|:-----:|:--------:|:---------:|
|  retCode  |String| Body  |必填| &emsp;|
|  retInfo  |String| Body  |必填| &emsp;|
|  data  |SceneTemplateDto| Body  |必填|&emsp;|


