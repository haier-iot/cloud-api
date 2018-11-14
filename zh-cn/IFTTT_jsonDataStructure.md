#### 场景对象UserSceneDto Json结构如下:


```
{
  "sceneName": "lyx0605",
  "sceneDesc": "lyx0605描述",
  "familyId" : "aaa",
  "userId": "3"
  "rules": [
    {
      "when": {
        "conditions": [
          {
            "desc": "用户填写提示的条件描述",		//条件描述需要拼接，联想定语
            "key": {
              "id": "87907c5c55114d8896a85780325f8940",
              "desc": "冷冻室显示温度(℃)"
            },
            "operationSign": "equal",
            "value": {
              "value": "26",
              "desc": "26℃"
            },
            "componentId": "3e8a0dfb038d40fab6c5b4936924086c"
          },
		  {
            "desc": "用户填写提示的条件描述",		//条件描述需要拼接，联想定语
            "key": {
              "id": "87907c5c55114d8896a85780325f8940",
              "desc": "冷冻室显示温度(℃)"
            },
            "operationSign": "equal",
            "value": {
              "value": "26",
              "desc": "26℃"
            },
            "componentId": "3e8a0dfb038d40fab6c5b4936924086c",
		    "logicalSign" : "&&"
          }
        ]
      },
      "then": {
        "actions": [
          {
            "type": "DeviceControl",
            "control": {
              "args": [
                {
                  "name": {
                    "id": "18efb47f7cec4cadbe6e15922ca0bad8",
                    "desc": "冷藏室设置档位"
                  },
                  "value": {
                    "value": "0",
                    "desc" : "0°C"
                  }
                }
              ],
              "componentId": "3e8a0dfb038d40fab6c5b4936924086c"
            }
          },
          {
            "type": "MessagePush",
            "pushMessage": {
              "msgStrategy": {
                "notWorkTimeStart": "00:00",
                "notWorkTimeEnd": "00:00"
              },
              "pushType": "family_device",
              "pushContent": {
                "msgName": "",
                "expires": 0,
                "msgTitle": "lyx0605",
                "msgContent": "lyx0605"
              },
              "showTypes": {
                "02": "2",
                "03": "2"
              },
              "priority": 2
            }
          }
        ],
        "desc": "lyx0605执行描述"
      }
    }
  ],
  "taskInfo" : {
	 "cron" : {
		"minutes" : "",
		"hours" : "",
		"day" : "",
		"month" : "",
		"week" : "",
		"year" : ""
	 },
	 "activeBeginTime" : "",
	 "activeEndTime" : "",
	 "status" : ""		//定时开关状态
  }
}
```  

####  查询支持的关系表达式接口输出参数data，Json类型结构如下: 
```  
[
  {
    "value":"greaterThan",
    "desc":"大于",
    "required":false
  },
  {
    "value":"greaterThanEqual",
    "desc":"大于等于",
    "required":false
  },
  {
    "value":"equal",
    "desc":"等于",
    "required":false
  },
  {
    "value":"lessThan",
    "desc":"小于",
    "required":false
  },
  {
    "value":"lessThanEqual",
    "desc":"小于等于",
    "required":false
  }
]

```