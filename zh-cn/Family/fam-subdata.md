# 家庭数据订阅

!> **更新时间**：{docsify-updated} 



 **TOPIC说明**

|**Topic编码**|**Topic名称** |**Topic描述** |**Topic类型** |**Topic离线客户端消息缓存时间** |  
| :-------------:|:-------------:|:-------------:| :-------------:|:-------------:|  
|DEV_FAMILY|	设备事件|	设备家庭信息上报|设备|24小时|

 
数据订阅服务功能的使用请参考：设备类服务-设备数据订阅。





**收到的消息示例**

```
{
    "header": {
        "ts": 1594781110192, 
        "topic": "bp_dev_family", 
        "type": " type值", 
        "from": "haier", 
        "send": "发送者systemId", 
        "ver": "v1.0.0"
    }, 
    "body": {
        "sn": "277d07eb43584c8eb5763c7b785e6aac", 
        "ts": "1594883543997", 
        "deviceId": "2C37C558FECA", 
        "typeId": "201461071080c0142204e798ea91620000008999a42d6ac5b44957fb1eb45640", 
        "deviceName": "设备在家庭中名称", 
        "familyId": "111111", 
        "familyName": "家庭名称", 
        "floorId": "楼层编号", 
        "floorName": "楼层名称", 
        "roomId": "111222", 
        "roomName": "房间名称", 
        "userId": "49520033", 
        "userRole": "用户角色", 
        "args": {
            "other": "xxx"
        }
    }
}

```

**字段说明**  

type：</br>
share_device_to_family 分享设备到家庭</br>
update_device_family_shareinfo 更新设备分享信息</br>
update_device_family_room 更新设备房间信息</br>
delete_family_share_device 删除分享的设备</br>


"deviceId": "设备ID"</br>
"deviceName": "设备家庭名称"</br>
"familyId": "家庭ID"</br>
"familyName": "家庭名称"</br>
"floorId": "楼层编号"</br>
"floorOrderId": "楼层序号"</br>
"floorName": "楼层名称"</br>
"roomId": "房间ID"</br>
"roomName": "房间名称"</br>
"userId": "49520033


