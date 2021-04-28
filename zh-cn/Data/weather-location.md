# 天气和位置服务
!> **更新时间**：{docsify-updated}  

## 天气服务

### 介绍  
**简介**
>  为了方便各产业及APP开发者调用天气服务接口

**公共参数**
- **接入地址：**  `https://uws.haier.net/rcs/weather/`

- **LocationDto：地址信息**
|字段名|类型|说明|备注|必须|
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
|字段名|类型|说明|备注|必须|
|:------:|:-----:|:-----:|:------:|:------:|
|areaId|String|区域编码|&emsp;|必须|
|country|String|国家|&emsp;|必须|
|province|String|省|&emsp;|必须|
|city|String|市|&emsp;|必须|
|district|String|区/县|&emsp;|必须|
|status|String|状态|1：有效 0：无效|必须|

- **WeatherCondition：气象信息**
|字段名|类型|说明|单位|weatherType|
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
|字段名|类型|说明|必须|
|:------:|:-----:|:-----:|:------:|
|pm25|String|Pm2.5|必须|
|aqi|String|aqi|必须|
|updateTime|DateTime|最近检测时间|必须|

- **Alert:预警内容**
|字段名|类型|说明|必须|
|:------:|:-----:|:-----:|:------:|
|type|String|预警类别|必须|
|level|String|aqi|必须|
|content|String|预警内容|必须|
|pubtime|DateTime|发布时间|必须|

- **Index：指数信息（红色内容，暂不支持，国外目前只支持未来三天）**
|字段名|类型|说明|必须|
|:------:|:-----:|:-----:|:------:|
|<font color=red>shortName</font>|String|简称|非必须|
|<font color=red>aliasName</font>|String|别名|非必须|
|name|String|指数名称|必须|
|level|String|指数级别|必须|
|desc|String|指数内容|必须|

- **DayWeather：每天的天气信息**
|字段名|类型|说明|必须|
|:------:|:-----:|:-----:|:------:|
|day|WeatherCondition|白天天气信息|必须|
|night|WeatherCondition|晚上天气信息|必须|
|humidity|String|湿度|必须|

- **History：历史天气**
|字段名|类型|说明|必须|
|:------:|:-----:|:-----:|:------:|
|dayWeathers|DayWeather|&emsp;|必须|
|hourWeather|Map<key, <HourWeather>>|&emsp;|必须|

- **HourWeather：每小时的天气信息**
|字段名|类型|说明|必须|
|:------:|:-----:|:-----:|:------:|
|weather|WeatherCondition|每小时天气信息|必须|
|airCondition|AirCondition|每小时空气质量|必须|


### 获取当前及未来天气信息  
**使用说明**  
>通过经纬度或区域名称和所需要的天气种类的枚举值来获取相应的天气信息获得当前及未来天气信息。

**接口描述**  

- ?> **接入地 址：**  `/current-forecast？types=current&types=aqi&types=aqiForecast&types=alert&types=hourForecast&types= indexForecast&types= dayForecast`

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

- **请求样例** 
    - **用户请求(城市名称方式)**
    ```
    POST data:
    {
        “province”: “北京市”，
        “city”:”北京市”，
        “district”:北京市朝阳区”
    }
    ```
    - **用户请求(区域id方式)**
    ```
    POST data:
    {
        “areaId”: “110100”
    }

    ```
- **请求应答**
```
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
                "humidity":"55",
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
            "2020-09-02":{
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


