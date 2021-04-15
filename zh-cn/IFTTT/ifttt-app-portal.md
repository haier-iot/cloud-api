

# 智家APP（Portal）

!> **更新时间**：{docsify-updated}  

## 模板场景是否可启用

> 基本信息

?> **接入地址：** `/iftttscene/component/getById`</br>
**HTTP Method：** POST

**接口描述**

```
{
    "retCode":"00000",
    "retInfo":"成功",
    "data":
        {
            "9c6716fa26da44ce8163951d650db839":false,
            "f96b7bb579544a7da192a4999ed712da":false,
            "0293edff46354369a7214ced4f53e7a5":false,
            "e8f531ca3fa54598b1fa7323c68f87f5":false,
            "f351aefb23e04465940ccfdc59e77b15":false
        }
}

```
> 请求参数

**Headers** 

参数名称|参数值|是否必须|示例|备注
:-|:-:|:-:|:-:|:-
Content-Type|application/json|是|&nbsp;|&nbsp;|

**Body** 

名称|类型|是否必须|默认值|备注|其他信息
:-|:-:|:-:|:-:|:-:|:-
productCodeList|string[]|必须|&nbsp;|&nbsp;|item 类型: string|
sceneIdList|string[]|必须|&nbsp;|&nbsp;|item 类型: string|

> 返回数据

名称|类型|是否必须|默认值|备注|其他信息
:-|:-:|:-:|:-:|:-:|:-
retCode|string|必须|&nbsp;|返回码|&nbsp;|
retInfo|string|必须|&nbsp;|返回信息|&nbsp;|
data|object[]|必须|&nbsp;|返回数据|item 类型: object|

## 获取型号支持的功能信息


## 获取型号功能


## 查询型号支持场景状态列表


## 根据中类组件属性ID查询属性信息（为了兼容app老服务暂时返回常量）

## 获取非设备类组件和属性功能列表


## 根据中类组件属性ID查询属性信息


## 获取组件信息接口


## 查询场景的规则信息


## 查询模板场景基本信息




