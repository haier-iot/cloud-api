# 天气和位置服务
!> **更新时间**：{docsify-updated}  

## 天气服务

### 介绍

**简介**
>  为了方便各产业及APP开发者调用天气服务接口

**公共参数**
- ?> **接入地址：**  `https://uws.haier.net/rcs/weather/`

- **LocationDto：地址信息**
|字段名称|类型|说明|备注|必须|
|:------:|:-----:|:-----:|:------:|:------:|
|latitude|double|纬度|&emsp;|&emsp;|
|longitude|double|经度|&emsp;|&emsp;|
|province|String|省|海外接口不使用|&emsp;|
|city|String|市|海外接口不使用|&emsp;|
|district|String|区/县|海外接口不使用|&emsp;|
|areaId|String|区域id|海外接口不使用|&emsp;|
**说明：**
>Location中参数分为以下三种方式，这三种方式使用一种，传对应参数就可以。
>1. 经纬度（latitude，longitude）通过经纬度查询（国外只能使用）
>2. 名称信息（province，city，district）通过区域名称查询
>3. 区域id查询（国内推荐）

- **Area：区域信息**
|字段名称|类型|说明|备注|必须|
|:------:|:-----:|:-----:|:------:|:------:|
|areaId|String|区域编码|&emsp;|是|
|country|String|国家|&emsp;|是|
|province|String|省|&emsp;|是|
|city|String|市|&emsp;|是|
|district|String|区/县|&emsp;|是|
|status|String|状态|1：有效 0：无效|否|

- **WeatherCondition：气象信息**
|字段名称|类型|说明|单位|weatherType|
|:------:|:-----:|:-----:|:------:|:------:|
|temp|String|温度|摄氏度|weatherCondition hourForecast dayForecast|
|humidity|String|湿度|%|weatherCondition hourForecast dayForecast|
|windLevel|String|风级|&emsp;|weatherCondition hourForecast dayForecast|
|windDir|String|风向|&emsp;|weatherCondition hourForecast dayForecast|
|condition|String|气象信息|&emsp;|weatherCondition hourForecast dayForecast|
|precipitation|String|过去一小时降水量|mm（毫米）|weatherCondition hourForecast|
|updateTime|DateTime|发布时间|&emsp;|weatherCondition hourForecast dayForecast|
|sunRise|DateTime|日出|&emsp;|weatherCondition hourForecast|
|sunSet|DateTime|日落|&emsp;|weatherCondition hourForecast|
|pressure|String|气压|百帕|weatherCondition hourForecast|
|realFeel|String|体感温度|&emsp;|hourForecast|
|iconDay|String|白天图标（url）|&emsp;|hourForecast|
|IconNight|String|晚上图标（url）|&emsp;|hourForecast|
|snow|String|降雪量|mm|hourForecast|

- **AirCondition：空气质量**
|字段名称|类型|说明|必须|
|:------:|:-----:|:-----:|:------:|
|pm25|String|Pm2.5|是|
|aqi|String|aqi|是|
|updateTime|DateTime|最近检测时间|是|

- **Alert:预警内容**
|字段名称|类型|说明|必须|
|:------:|:-----:|:-----:|:------:|
|type|String|预警类别|是|
|level|String|aqi|是|
|content|String|预警内容|是|
|pubtime|DateTime|发布时间|是|

- **Index：指数信息（红色内容，暂不支持，国外目前只支持未来三天）**
|字段名称|类型|说明|必须|
|:------:|:-----:|:-----:|:------:|
|<font color=red>shortName</font>|String|简称|否|
|<font color=red>aliasName</font>|String|别名|否|
|name|String|指数名称|是|
|level|String|指数级别|是|
|desc|String|指数内容|是|

- **DayWeather：每天的天气信息**
|字段名称|类型|说明|必须|
|:------:|:-----:|:-----:|:------:|
|day|WeatherCondition|白天天气信息|是|
|night|WeatherCondition|晚上天气信息|是|
|humidity|String|湿度|是|

- **History：历史天气**
|字段名称|类型|说明|必须|
|:------:|:-----:|:-----:|:------:|
|dayWeathers|DayWeather|&emsp;|是|
|hourWeather|Map<key, <HourWeather>>|&emsp;|是|

- **HourWeather：每小时的天气信息**
|字段名称|类型|说明|必须|
|:------:|:-----:|:-----:|:------:|
|weather|WeatherCondition|每小时天气信息|是|
|airCondition|AirCondition|每小时空气质量|是|


### 获取当前及未来天气信息  
**使用说明**  
>通过经纬度或区域名称和所需要的天气种类的枚举值来获取相应的天气信息获得当前及未来天气信息。

**接口描述**  

- ?> **接入地 址：**  `/current-forecast?types=current&types=aqi&types=aqiForecast&types=alert&types=hourForecast&types=indexForecast&types=dayForecast`

     **HTTP Method：** POST

- **输入参数**

|参数名称|类型|位置|必填|说明|
|:------:|:-----:|:-----:|:------:|:------:|
|locationDto|LocationDto|Body|是|&emsp;|
|types|List<String>|url|否|选填（不填时，返回所有类型的天气数据）|
|yesterday|Boolean|url|否|选填（是否需要15天预报信息中昨天的信息）默认返回，为false时不返回|
|days|String|url|否|选填（需要15天预报信息中的多少天）默认全部返回，最多返回15天|

**Type为枚举类型，枚举值为以下几种**

|**枚举值**|**说明**|
|:------:|:-----:|
|current|实况天气|
|aqi|空气质量|
|alert|预警信息|
|hourForecast|24小时预报|
|indexForecast|15天指数预报|
|dayForecast|15天预报信息（前一天+当天+未来14天）|
|aqiForecast|未来7天aqi信息|

- **输出参数**

|参数名称|类型|说明|
|:-------:|:----------:|:---------:|
|area|Area|区域信息|
|weatherCondition|WeatherCondition|实况信息|
|airCondition|AirCondition|空气质量（可能为null，目前有些区域不提供空气质量）|
|alert|List<Alert>|预警信息（可能为null，当前区域没有预警信息|
|hourForecast|Map<String,<HourWeather>>|每小时信息，key为小时，格式为DateTime（精确到小时），如： “2019-12-20 20:00:00”|
|indexForecast|Map<String,List<Index>>|Key为日期，格式为Date，如“2019-12-20”，value为指数列表（当前只有未来3天信息）|
|dayForecast|Map<String,<DayWeather >>|Key为日期（天，前一天，当天，未来14天），格式为Date，如“2019-12-20”，Value为预报信息|
|aqiForecast|Map<String, AirCondition>|Key为预测日期，value为aqi信息|

**示例**

- **用户请求** 
    - **用户请求(城市名称方式)**
    ```json
    POST data:
    {
        "province":"北京市",
        "city":"北京市",
        "district":"北京市朝阳区"
    }
    ```
    - **用户请求(区域id方式)**
    ```json
    POST data:
    {
        "areaId":"110100"
    }

    ```
- **请求应答**

```json
{
    "payload":{
        "airCondition":{
            "aqi":"22",
            "pm25":"13",
            "updateTime":"2020-09-03 15:00:00"
        },
        "alerts":[
            {
                "content":"市气象台2020年9月2日20时00分发布大风蓝色预警信号：受冷空气影响，本市2日夜间至3日白天偏北风将逐渐增大至4级左右，阵风可达7级左右，请注意防范。（预警信息来源：国家预警信息发布中心）",
                "level":"蓝色",
                "pubtime":"2020-09-02 20:06:57",
                "type":"大风蓝色"
            }
        ],
        "area":{
            "areaId":"110100",
            "city":"北京市",
            "country":"中国",
            "province":"北京市"
        },
        "dayForecast":{
            "2020-09-02":{
                "day":{
                    "condition":"多云",
                    "conditionId":"http://resource.haier.net/download/weather/W1.png",
                    "sunRise":"2020-09-02 05:43:00",
                    "temp":"29",
                    "windDir":"西北风",
                    "windLevel":"3-4"
                },
                "night":{
                    "condition":"多云",
                    "conditionId":"http://resource.haier.net/download/weather/W31.png",
                    "sunSet":"2020-09-02 18:45:00",
                    "temp":"19",
                    "windDir":"北风",
                    "windLevel":"3-4"
                },
                "humidity":"55"
            }
        },
        "hourForecast":{
            "2020-09-03 15:00:00":{
                "condition":"多云",
                "conditionId":"82",
                "humidity":"29",
                "iconDay":"http://resource.haier.net/download/weather/W1.png",
                "iconNight":"http://resource.haier.net/download/weather/W31.png",
                "pressure":"1000",
                "qpf":"0.0",
                "realFeel":"29",
                "snow":"0.0",
                "temp":"28",
                "windDir":"西风"
            }
        },
        "indexForecast":{
            "2020-09-03":[
                {
                    "desc":"您将感到很舒适，一般不需要开启空调。",
                    "level":"较少开启",
                    "name":"空调开启指数"
                }
            ]
        },
        "language":"zh-cn",
        "weatherCondition":{
            "condition":"多云",
            "conditionId":"http://resource.haier.net/download/weather/W1.png",
            "humidity":"29",
            "precipitation":"0",
            "pressure":"1000",
            "sunRise":"2020-09-03 05:44:00",
            "sunSet":"2020-09-03 18:43:00",
            "temp":"28",
            "updateTime":"2020-09-03 16:00:08",
            "windDir":"西风",
            "windLevel":"4"
        },
        "aqiForecast":{
            "2020-09-02":{获取历史天气信息
                "pm25":"6",
                "aqi":"20",
                "updateTime":"2020-09-03 14:00:00"
            }
        }
    },
    "retCode":"00000",
    "retInfo":"成功"
}

```  

### 获取历史天气信息  

**使用说明**  
>提供通过位置信息获取区域历史天气信息的服务（只支持国内）。用户提交位置信息（经纬度或区域名称），系统通过位置信息计算所在的区域，并返回区域的历史天气信息。

**接口描述**  

- ?> **接入地 址：**  `/history?beginDate=2020-02-18&endDate=2020-02-19`

     **HTTP Method：** POST

- **输入参数**

|参数名称|类型|位置|必填|说明|
|:------:|:-----:|:-----:|:------:|:------:|
|location|LocationDto|Body|是|&emsp;|
|beginDate|Date|url|否|批量查询开始日期(2019-12-10)|
|endDate|Date|url|否|批量查询结束日期(2019-12-13)|
**说明：**
>beginDate为开始日期，endDate为结束日期，beginDate和endDate的范围在当前时间的过去3天之内，当需要查询过去某一天时，开始日期与结束日期为同一天，开始日期为空时，默认从前3天开始，结束日期为空时，默认到当前时间的前一天，都为空时，默认返回当前时间的过去3天之内的全部信息。

- **输出参数**

|参数名称|类型|说明|
|:-------:|:----------:|:---------:|
|area|Area|区域信息|
|historyMap|Map<String,History>|Key为日期，格式为Date，如“2019-12-20”|

**示例**

- **用户请求** 
``` json
POST data:
{
    "province":"北京",
    "city":"北京",
    "district":"朝阳"
}
```

- **请求应答**
```json
{
    "payload":{
        "area":{
            "areaId":"110100",
            "city":"北京市",
            "country":"中国",
            "province":"北京市"
        },
        "historyMap":{
            "2020-08-31":{
                "dayWeathers":{
                    "day":{
                        "condition":"小雨",
                        "sunRise":"2020-08-31 05:42:00",
                        "temp":"25",
                        "windDir":"北风",
                        "windLevel":"2"
                    },
                    "night":{
                        "condition":"晴",
                        "sunSet":"2020-08-31 18:48:00",
                        "temp":"18",
                        "windDir":"西南风",
                        "windLevel":"2"
                    }
                },
                "hourWeather":{
                    "2020-08-31 00:00:00":{
                        "airCondition":{
                            "aqi":"42",
                            "pm25":"39",
                            "updateTime":"2020-08-31 00:00:00"
                        },
                        "weather":{
                            "condition":"多云",
                            "humidity":"74",
                            "precipitation":"0",
                            "pressure":"1004",
                            "temp":"25",
                            "windDir":"东南风",
                            "windLevel":"2"
                        }
                    }
                }
            }
        }
    },
    "retCode":"00000",
    "retInfo":"成功"
}
```

### 对应表
- **实况天气conditionId与icon对应关系**

**原天气服务供应商：**

|conditionId|icon（白天）|icon（夜间）|中文|
|:-----:|:-----:|:-----:|:-----:|
|1|0|30|晴|
|2|0|30|晴|
|3|0|30|晴|
|4|0|30|晴|
|5|0|30|晴|
|6|0|30|大部晴朗|
|7|0|30|大部晴朗|
|8|1|31|多云|
|9|1|31|多云|
|10|1|31|多云|
|11|1|31|多云|
|12|1|31|少云|
|13|2|2|阴|
|14|2|2|阴|
|15|3|33|阵雨|
|16|3|33|阵雨|
|17|3|33|阵雨|
|18|3|33|阵雨|
|19|3|33|阵雨|
|20|3|33|局部阵雨|
|21|3|33|局部阵雨|
|22|3|33|小阵雨|
|23|3|33|强阵雨|
|24|13|34|阵雪|
|25|13|34|小阵雪|
|26|18|32|雾|
|27|18|32|雾|
|28|18|32|冻雾|
|29|20|36|沙尘暴|
|30|29|35|浮尘|
|31|29|35|尘卷风|
|32|29|35|扬沙|
|33|20|36|强沙尘暴|
|34|45|46|霾|
|35|45|46|霾|
|36|2|2|阴|
|37|4|4|雷阵雨|
|38|4|4|雷阵雨|
|39|4|4|雷阵雨|
|40|4|4|雷阵雨|
|41|4|4|雷阵雨|
|42|4|4|雷电|
|43|4|4|雷暴|
|44|5|5|雷阵雨伴有冰雹|
|45|5|5|雷阵雨伴有冰雹|
|46|5|5|冰雹|
|47|5|5|冰针|
|48|5|5|冰粒|
|49|6|6|雨夹雪|
|50|6|6|雨夹雪|
|51|7|7|小雨|
|52|7|7|小雨|
|53|8|8|中雨|
|54|9|9|大雨|
|55|10|10|暴雨|
|56|10|10|大暴雨|
|57|10|10|特大暴雨|
|58|14|14|小雪|
|59|14|14|小雪|
|60|15|15|中雪|
|61|15|15|中雪|
|62|16|16|大雪|
|63|17|17|暴雪|
|64|19|19|冻雨|
|65|19|19|冻雨|
|66|7|7|小雨|
|67|8|8|中雨|
|68|9|9|大雨|
|69|10|10|大暴雨|
|70|10|10|大暴雨|
|71|14|14|小雪|
|72|14|14|小雪|
|73|14|14|小雪|
|74|15|15|大雪|
|75|15|15|大雪|
|76|16|16|大雪|
|77|15|15|雪|
|78|8|8|雨|
|79|45|46|霾|
|80|1|31|多云|
|82|1|31|多云|
|81|1|31|多云|
|83|18|32|雾|
|84|18|32|雾|
|85|2|2|阴|
|86|3|33|阵雨|
|87|4|4|雷阵雨|
|88|4|4|雷阵雨|
|89|4|4|雷阵雨|
|90|4|4|雷阵雨|
|91|7|7|小到中雨|
|92|9|9|中到大雨|
|93|10|10|大到暴雨|
|94|15|15|小到中雪|

**切源后：**

> 由于新的天气服务供应商未提供天气现象和conditionId的对应关系，因此天气服务不再提供和维护旧的关系

|conditionId|icon（白天）|icon（夜间）|中文|
|:-----:|:-----:|:-----:|:-----:|
|晴_白天|W晴_白天|W晴_白天|晴|
|晴_夜间|W晴_夜间|W晴_夜间|晴|
|多云_白天|W多云_白天|W多云_白天|多云|
|多云_夜间|W多云_夜间|W多云_夜间|多云|
|阴|W阴|W阴|阴|
|轻度雾霾|W轻度雾霾|W轻度雾霾|轻度雾霾|
|中度雾霾|W中度雾霾|W中度雾霾|中度雾霾|
|重度雾霾|W重度雾霾|W重度雾霾|重度雾霾|
|小雨|W小雨|W小雨|小雨|
|中雨|W中雨|W中雨|中雨|
|大雨|W大雨|W大雨|大雨|
|雾|W雾|W雾|雾|
|暴雨|W暴雨|W暴雨|暴雨|
|小雪|W小雪|W小雪|小雪|
|中雪|W中雪|W中雪|中雪|
|大雪|W大雪|W大雪|大雪|
|暴雪|W暴雪|W暴雪|暴雪|
|浮尘|W浮尘|W浮尘|浮尘|
|沙尘|W沙尘|W沙尘|沙尘|
|大风_白天|W大风_白天|W大风_白天|大风|
|大风_夜间|W大风_夜间|W大风_夜间|大风|


- **预报接口conditionIdDay与conditionIdNight对应关系**

|conditionIdDay|conditionIdNight|简体|conditionIdDay|condirionIdNight|简体|
|:--------:|:--------:|:--------:|:--------:|:--------:|:--------:|
|0|30|晴|5|5|冰粒|
|0|30|晴|6|6|雨夹雪|
|0|30|晴|6|6|雨夹雪|
|0|30|晴|7|7|小雨|
|0|30|晴|7|7|小雨|
|0|30|大部晴朗|8|8|中雨|
|0|30|大部晴朗|9|9|大雨|
|1|31|多云|10|10|暴雨|
|1|31|多云|10|10|大暴雨|
|1|31|多云|10|10|特大暴雨|
|1|31|多云|14|14|小雪|
|1|31|少云|14|14|小雪|
|2|2|阴|15|15|中雪|
|2|2|阴|15|15|中雪|
|3|33|阵雨|16|16|大雪|
|3|33|阵雨|17|17|暴雪|
|3|33|阵雨|19|19|冻雨|
|3|33|阵雨|19|19|冻雨|
|3|33|阵雨|7|7|小雨|
|3|33|局部阵雨|8|8|中雨|
|3|33|局部阵雨|9|9|大雨|
|3|33|小阵雨|10|10|大暴雨|
|3|33|强阵雨|10|10|大暴雨|
|13|34|阵雪|14|14|小雪|
|13|34|小阵雪|14|14|小雪|
|18|32|雾|14|14|小雪|
|18|32|雾|15|15|大雪|
|18|32|冻雾|15|15|大雪|
|20|36|沙尘暴|16|16|大雪|
|29|35|浮尘|15|15|雪|
|29|35|尘卷风|8|8|雨|
|29|35|扬沙|45|46|霾|
|20|36|强沙尘暴|1|31|多云|
|45|46|霾|1|31|多云|
|45|46|霾|1|31|多云|
|2|2|阴|18|32|雾|
|4|4|雷阵雨|18|32|雾|
|4|4|雷阵雨|2|2|阴|
|4|4|雷阵雨|3|33|阵雨|
|4|4|雷阵雨|4|4|雷阵雨|
|4|4|雷阵雨|4|4|雷阵雨|
|4|4|雷电|4|4|雷阵雨|
|4|4|雷暴|4|4|雷阵雨|
|5|5|雷阵雨伴有冰雹|7|7|小到中雨|
|5|5|雷阵雨伴有冰雹|9|9|中到大雨|
|5|5|冰雹|10|10|大到暴雨|
|5|5|冰针|15|15|小到中雪|

- **空气质量指数对应关系**
|数值|简体|数值|简体|
|:--------:|:--------:|:--------:|:--------:|
|-100000, -1|其他|151, 200|中度污染|
|0, 50|优|201, 300|重度污染|
|51, 100|良|301, 500|严重污染|
|101, 150|轻度污染|501, 100000|爆表|

- **风向**

![风向][pic1]


- **预警种类**

![预警种类][pic2]

- **指数种类说明**

![指数种类说明][pic3]

[^-^]:常用图片注释
[pic1]:../Data/_media/weather-location/pic1.png
[pic2]:../Data/_media/weather-location/pic2.png
[pic3]:../Data/_media/weather-location/pic3.png


## 地址服务

### 介绍

**简介**

>  为了方便各产业及APP开发者调用位置服务接口

**公共参数**
- ?> **接入地址：**  `https://uws.haier.net/rcs/location/`

- **Area：区域信息（schemaVersion ="v1"的数据结构）**
|字段名称|类型|说明|备注|必须|
|:------:|:-----:|:-----:|:------:|:------:|
|areaId|String|区域编码|&emsp;|是|
|country|String|国家|&emsp;|是|
|province|String|省|&emsp;|是|
|city|String|市|&emsp;|是|
|district|String|区/县|&emsp;|是|
|status|String|状态|1：有效; 0：无效|否|

- **NewArea：区域信息（schemaVersion="v2"的数据结构）**
|字段名称|类型|说明|备注|必须|
|:------:|:-----:|:-----:|:------:|:------:|
|areaId|String|区域编码|&emsp;|&emsp;|
|areaName|String|区域名称|&emsp;|&emsp;|
|level|int|区域级别|&emsp;|&emsp;|
|status|String|当前区域是否有效|1：有效; 0：无效|&emsp;|
|latitude|double|纬度|&emsp;|&emsp;|
|longitude|double|经度|&emsp;|&emsp;|
|pinyinName|String|拼音名称|&emsp;|&emsp;|

- **区域Json格式示例**

    - **V1版本**
    ```json
    {
        "language":"zh_cn",
        "version":"v1",
        "areas":[
            {
                "areaId":"101010100",
                "district":"西城区",
                "city":"北京市",
                "country":"中国",
                "province":"北京市",
                "status":"0"
            },
            {
                "areaId":"101010200",
                "district":"海淀区",
                "city":"北京市",
                "country":"中国",
                "province":"北京市",
                "status":"1"
            }
        ]
    }
    ```

    - **V2版本**
    ```json
    {
        "areas":[
            {
                "areaId":"000000",
                "areaName":"中国",
                "areas":[
                    {
                        "areaId":"110000",
                        "areaName":"北京市",
                        "areas":[
                            {
                                "areaId":"110100",
                                "areaName":"北京市",
                                "areas":[
                                    {
                                       "areaId":"110101",
                                        "areaName":"东城区",
                                        "areas":[
                                            {
                                                "areaId":"110101004000",
                                                "areaName":"安定门街道",
                                                "level":5,
                                                "pinyinName":"an ding men jie dao",
                                                "status":1
                                            }
                                        ],
                                        "latitude":39.921224,
                                        "level":4,
                                        "longitude":116.443244,
                                       "pinyinName":"dong cheng qu",
                                    "status":1
                                    }
                                    ],
                                "latitude":12.11,
                                "level":3,
                                "longitude":33.111,
                                "pinyinName":"bei jing shi",
                                "status":1
                            }
                        ],
                        "level":2,
                        "pinyinName":"bei jing shi",
                        "status":1
                    }
                ],
                "level":1,
                "pinyinName":"zhong guo",
                "status":1
            }
        ],
        "language":"zh-cn",
        "version":"v2"
    }
    ```  


### 根据位置信息（经纬度）定位区域的详细地址
**使用说明**  
>提供根据位置信息（经纬度）定位区域的详细地址的功能。用户提交位置信息（经纬度），系统通过位置信息计算所在区域的详细地址（街道、门牌号等），并返回地址信息。

**接口描述**  

- ?> **接入地 址：**  `/place？latitude=39.92999& longitude=123.78906`

     **HTTP Method：** POST

- **输入参数**

|参数名称|类型|位置|必填|说明|
|:------:|:-----:|:-----:|:------:|:------:|
|latitude|double|url|是|纬度|
|longitude|double|url|是|经度|

- **输出参数**

|参数名称|类型|示例|说明|
|:------:|:-----:|:-----:|:------:|
|language|String|zh-cn|此版本详细地址信息的语言标识（目前只支持中文）|
|place|Place|&emsp;|详细地址信息|

**Place：详细地址信息**

|字段名称|类型|示例|说明|
|:------:|:-----:|:-----:|:------:|
|areaId|String|101010200|区域唯一标识(可能为null)|
|country|String|中国|区域所在国家|
|province|String|北京|区域所在省/直辖市/特别行政区|
|city|String|北京市|区域所在市|
|district|String|海淀区|区域所在区/县|
|street|String|长春桥路|区域所在街道|
|doorplate|String|17号|区域所在街道门牌|
|street|String|北京市海淀区长春桥路17号北京市海淀区人民政府(东南门)|区域所在地址|


**示例**

- **用户请求**
    ```
    POST data:
    ```

- **请求应答**
    ```json
    {
        "place":{
            "areaId":"101010300",
            "district":"北京市朝阳区",
            "city":"北京市",
            "country":"中国",
            "province":"北京市"
        },
        " language ":"zh_cn"
    }
    ```

### 查询区域列表
**使用说明**  
>获得指定版本区域信息的json文件。

**接口描述**  

- ?> **接入地 址：**  `/area-json?schemaVersion=v2`

     **HTTP Method：** POST

- **输入参数**

|参数名称|类型|位置|必填|说明|
|:------:|:-----:|:-----:|:------:|:------:|
|schemaVersion|String|url|否|schemaVersion为空，返回v1版本区域文件|
|language|String|Header|是|zh-cn/en-us(v2版本只支持zh-cn)|

- **输出参数**

|参数名称|类型|示例|说明|
|:------:|:-----:|:-----:|:------:|
|areaUrls|List<AreaUrl>|&emsp;|区域json文件的url|

**AreaUrl**

|字段名|类型|说明|备注|必须|
|:------:|:-----:|:-----:|:------:|:------:|
|md5|String|（32位）|&emsp;|是|
|url|String||url中文件为json格式，下方5中有示例（utf-8）|是|
|version|String|&emsp;|&emsp;|是|
|level|Integer|2.省 3.省市 4.省市区 5.省市区镇|&emsp;|V2版本返回|


**示例（schemaVersion = v2）**

- **用户请求**

?> https:// uws.haier.net/rcs/location /area-json?schemaVersion=v2

- **请求应答**
    ```json
    {
        "retCode":"00000",
        "retInfo":"正确",
        "payload":[
            {
                "md5":"e10adc3949ba59abbe56",
                "url":"http://resource.haier.net/download/resource/location/town-v2.json",
                "version":"2.0",
                "level":"5"
            },
            {
                "md5":"22222222222222",
                "url":"http://resource.haier.net/download/resource/location/province-v2.json",
                "version":"2.0",
                "level":"2"
            },
            {
                "md5":"1111111111111111111",
                "url":"http://resource.haier.net/download/resource/location/city-v2.json",
                "version":"2.0",
                "level":"3"
            },
            {
                "md5":"555555555555555555",
                "url":"http://resource.haier.net/download/resource/location/district-v2.json",
                "version":"2.0",
                "level":"4"
            }
        ]
    }
    ```

**示例（schemaVersion不传）**

- **用户请求**

?> https://uws.haier.net/rcs/location/area-json

- **请求应答**
    ```json
    {
        "retCode":"00000",
        "retInfo":"正确",
        "payload":[
            {
                "md5":"1 4CA5A54E56ECA64581",
                "url":"http://resource.haier.net/download/resource/location/area-zh.json",
                "version":"1.0"
            }
        ]
    }
    ```

### 根据areaId查询区域信息
**使用说明**  
>根据区域id返回区域信息，只返回有效区域信息。

**接口描述**  

- ?> **接入地 址：**  `/area？areaId=101000100`

     **HTTP Method：** POST

- **输入参数**

|参数名称|类型|示例|说明|
|:------:|:-----:|:-----:|:------:|
|areaId|String|&emsp;|区域编码|

- **输出参数**

|参数名称|类型|示例|说明|
|:------:|:-----:|:-----:|:------:|
|area|Area|&emsp;|area中status一直为1，只返回有效区域信息|
|language|Stringzh_cn|&emsp;|

**请求示例**

- **用户请求**
    ```
    POST data:
    ```

- **请求应答**
    ```json
    {
        "area":{
            "areaId":101010100,
            "district ":"北京",
            "city":"北京",
            "country":"中国",
            "province":"北京",
            "status":"1"
        },
        "language":"zh_cn"
    }
    ```

### 根据区域id或区域名称查询下级区域列表
**使用说明**  
>根据区域id或区域名称查询下级区域列表（只返回有效状态的区域）。

**接口描述**  

- ?> **接入地 址：**  `/find/child-areas`

     **HTTP Method：** POST
- **输入参数**

|参数名|类型|位置|必填|说明|
|:--------:|:--------:|:--------:|:--------:|:--------:|
|areaId|String|Body|否|只支持schemaVersion = v2的区域编码|
|city|String|Body|否|市名称|
|district|String|Body|否|区名称|
|province|String|Body|否|省名称|
|country|String|Body|否|国家名称，默认“中国”|
|areaLevel|String|url|否|[areaLevel备注](#jump1)|

<a id="jump1"> </a>
**areaLevel备注**
> 1.	当请求参数为areaId时，可选填areaLevel，为当前编码级别，不填当下级存在两种级别区域编码时，默认都返回，填入时，只返回当前级别的下一级
> 2.	当请求参数为区域名称时，不填这个参数，填入时，也不生效

**备注**

> areaId和国家，省，市，区名称选填一种即可，都填按照areaId处理。
>1.	区域信息为区域编码时，只在areaId字段填写上级城市区域编码，其他字段不填。
>2.	区域信息为区域名称时，areaId字段不填，当前区域，包括其上级所有的城市名称都必须填，其余字段不填。例如：district字段填入”西城区”时,city字段必须填入”北京市”,province字段必须填入”北京市”,不可以只填入district字段、而city或province字段不填。

- **输出参数**

|参数名|类型|示例|说明|
|:--------:|:--------:|:--------:|:--------:|
|newAreas|List<NewArea>|&emsp;|下级区域列表（只返回有效状态的区域）|
|language|String|zh-cn|&emsp;|
|version|String|v2|NewArea的schemaVersion|

**示例**

- **用户请求** 
    - **请求参数为区域编码时**
    ```json
    POST data:
    {
        "areaId":"110105"
    }
    ```
    - **请求参数为区域名称时**
    ```json
    POST data:
    {
        "country": "中国",
        "province": "北京市",
        "city": "北京市"
    }
    ```
- **请求应答**
```json
    {
        "newAreas":[
            {
                "areaId":"110105",
                "areaName":"朝阳区",
                "level":3,
                "status":1,
                "latitude":39.92122,
                " longitude":116.443244,
                "pinyinName":"an ding men jie dao"
            },
            {
                "areaId":"110102",
                "areaName":"西城区",
                "level":3,
                "status":1,
                "latitude":39.92122,
                "longitude":116.443244,
                "pinyinName":"an ding men jie dao"
            }
        ],
        "language":"zh-cn",
        "version":"v2"
    }
```
