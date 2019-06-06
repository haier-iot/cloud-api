
>  **当前版本**：[UWS 数据订阅 V1.0.0](zh-cn/ChangeLog/DataSubscription)  
 **更新时间**：{docsify-updated}  

## 简介
>  数据订阅服务功能旨在帮助开发者或应用快速获取实时的增量数据，包括设备信息数据、用户数据以及其他开发者根据实际需求订阅的自定义数据。

智能互联设备上传的实时数据会进入IOT平台，当开发者或应用需要参考设备数据进行相关分析或操作时，开发者可以通过数据订阅连接到U+IOT平台设备上报的实时数据。

![数据订阅图片][dataSubscription_type]

第三方可以通过数据推送服务，订阅海尔设备消息功能，实现海尔设备数据与第三方平台的互通。设备消息包括设备的上下线，设备状态属性，设备告警，以及设备的大数据信息。目前支持根据设备的typeid进行推送。
设备数据订阅包括设备基础配置信息以及设备运行状态的实时信息。
1、	设备基础信息，包括设备版本、绑定信息、型号等；
2、	运行状态，包括故障、wifi信息、心跳、离线原因等。


## 应用场景

适用于第三方平台要获取在海尔平台上的设备数据（上下线，设备状态属性，设备告警，以及设备的大数据信息）用来实现某些业务逻辑。

面向使用或销售海尔平台设备的第三方合作伙伴，服务支持海尔U+云平台向第三方合作商推送设备的上下线、属性、报警、大数据信息功能。实现海尔云平台向第三方平台服务的数据推送功能。

![数据订阅场景流程][dataS_flow]

**申请授权**

设备数据权限根据设备种类进行订阅，由typeid限定订阅消息的设备类型，由消息topic限定定于的数据种类

**业务处理**

数据订阅放参考接口，开发消息接收端，完成测试、联调、上线。通过审核后即可在生产环境正常订阅设备消息数据



## 公共说明  

### 接入方式  

本服务提供的所有接口仅支持wss协议请求。  
在开发、联调时，应用开发者应该连接开发者环境，通过配置应用开发者访问外网的路由器（设置路由器的dns），可以连接到不同的开发环境。  

### 接入地址  

| **接口分类** | **接入地址** |**开发者环境DNS配置**|  
| :-------------: |:-------------:|:-----:|  
|数据订阅|`wss://uws.haier.net/wssubscriber`|`123.103.113.62`|  

### 签名认证  

**说明**  

调用方需要对发送到wss的请求进行签名，执行签名计算的签名值需要赋值到URL的sign属性，以便服务端进行签名验证。  

**参数介绍**  

待签名字符串为： systemId + systemKey + timestamp；   
systemId：应用ID，40位以内字符,Haier U+ 云平台全局唯一；  
systemKey：在海极网给应用申请的systemKey，不能明文发送；  
timestamp：Unix时间戳，精确到毫秒；  


**算法**  


签名算法就是对待签名字符串计算32位小写SHA-256值，算法示例:  

```
String getSign(String systemId, String systemKey, String timestamp) {
    systemKey = systemKey.trim();
    systemKey = systemKey.replaceAll("\"", "");

    StringBuffer sb = new StringBuffer();
    sb.append(systemId).append(systemKey).append(timestamp);
    MessageDigest md = null;
    byte[] bytes = null;
    try {
      md = MessageDigest.getInstance("SHA-256");
      bytes = md.digest(sb.toString().getBytes("utf-8"));
    } catch (Exception e) {
      e.printStackTrace();
    }
    return BinaryToHexString(bytes);
  }

  String BinaryToHexString(byte[] bytes) {
    StringBuilder hex = new StringBuilder();
    String hexStr = "0123456789abcdef";
    for (int i = 0; i < bytes.length; i++) {
      hex.append(String.valueOf(hexStr.charAt((bytes[i] & 0xF0) >> 4)));
      hex.append(String.valueOf(hexStr.charAt(bytes[i] & 0x0F)));
    }
    return hex.toString();
  }

```

## 数据通讯模型   

 ![数据订阅场景流程][dataS_model]  
 
客户端向服务端发起连接请求，服务端对连接请求参数进行鉴权，鉴权失败，则断开连接并返回客户端鉴权失败信息；鉴权成功，则建立通道。  

鉴权成功后，客户端可多次向服务端发起订阅关系，多次订阅同一个topic的key组合，服务端则自动合并为一个订阅，默认为同一个客户端订阅；  

针对同一个云服务，发起多次连接，服务端则认为是多个客户端发起请求，所有消息将平衡分发到多个客户端上，每个云服务最多可发起连接数为系统可配置参数。  

取消订阅，则删除订阅关系，服务端返回客户端取消响应结果。  


## 数据订阅服务接口  

#### 建立连接  
> 提供建立连接的功能。客户端发起连接请求，数据订阅系统会对客户端连接请求进行鉴权后，建立与客户端的连接。   
> 客户端发起连接请求，请求时携带对应应用的systemId和systemKey签名信息进行鉴权，鉴权成功返回鉴权结果，鉴权失败，则返回错误信息并断开连接。 

##### 1、接口定义
?> **接入地址：** `/wssubscriber/msgplatform/websocket`</br>

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
systemId|String|url|必填|应用ID，50位以内字符,Haier U+ 云平台全局唯一  
timestamp|long|url|必填|Unix时间戳，精确到毫秒  
sign|String|url|必填|对请求进行签名运算产生的签名 见签名算法   
isSubscribeEarliestMessage|String|url|必填|决定该链接对应的收消息方式。<br>true: 从最早开始收消息<br>false或不填： 从当前开始收最新消息  

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
cmd|String|body|必填|操作说明
data|String|body|必填|响应结果

##### 2、请求样例

**请求地址**

```
wss://uws.haier.net/wssubscriber/msgplatform/websocket?systemId=SV-BLKALPHA21-0001-123&timestamp=1491013448629&sign=c70500c16563b5ccc7d032831bff34a5cb02c147ca6beeffff54d22d262a319e&isSubscribeEarliestMessage=false
```

**用户请求** 无  

**请求应答**

```
{
  "cmd": "authenticate-ack",
  "data": {
    "code": "00000",
    "result": "success",
    "systemId": "SV-BLKALPHA21-0001-123"
  }
}

```

##### 3、状态码

状态码|描述 
:-|:-
00000|成功
00001|失败
   

#### 订阅接口  
> 订阅指定topic消息，消息订阅必须在建立连接成功的前提下进行，如果建立连接返回成功，才可以发送订阅，如果失败，则无法进行订阅。  

注意：Websocket订阅服务的订阅范围只针对详细订阅，不包括全量订阅，这点和SDK是有区别的。  

##### 1、接口定义  

客户端向云端发送的JSON字符串格式数据如下（`DEV_EVENT、101c1200240008101e0a00000141414100000000020000000000000000000000`为示例数据）：  

```  

{
  "cmd": "subscribe",
  "data": {
    "subdata": [
      {
        "topic": "DEV_EVENT",
        "keys": {
          "typeId": [
            "101c1200240008101e0a00000141414100000000020000000000000000000000"
          ]
        }
      }
    ]
  }
}

```
说明：用户一次性可以订阅多个topic多个keys，其中如果有任何一个订阅验证失败，则本次请求全部订阅均失败，只有当全部topic的keys订阅成功，则本次订阅成功。  


云端向客户端返回订阅结果的响应JSON字符串格式数据如下（`SV-BLKALPHA21-0001-123、101c1200240008101e0a00000141414100000000020000000000000000000000_online_DEV_EVENT`为示例数据）：

```  
{
  "cmd": "subscribe-ack",
  "data": {
    "code": "00000",
    "result": "success",
    "systemId": "SV-BLKALPHA21-0001-123",
    "desc":"Topic [101c1200240008101e0a00000141414100000000020000000000000000000000_online_DEV_EVENT] subscribed ok"
  }
}


```

订阅成功之后，当topic有数据更新时，客户端会收到如下JSON字符串格式数据：    

```   
{
"topic": "XXXXXXX",
"typeId": "000000001000000000000000000000000000", 
"qos": "0", 
"data": {
          //消息内容，以schema为准
	    }
}


```  

##### 3、错误码

> 34001,34002,34003,34004,34005,34006,34999


#### 取消订阅接口  
> 取消订阅指定topic消息，取消订阅必须在建立连接成功的前提下进行，如果建立连接返回成功，才可以发取消订阅，如果失败，则无法进行取消订阅。


##### 1、接口定义  

客户端向云端发送的JSON字符串格式数据如下（`DEV_EVENT、101c1200240008101e0a00000141414100000000020000000000000000000000`为示例数据）： 

```  

{
  "cmd": "unsubscribe",
  "data": {
    "subdata": [
      {
        "topic": "DEV_EVENT",
        "keys": {
          "typeId": [
            "101c1200240008101e0a00000141414100000000020000000000000000000000"
          ]
        }
      }
    ]
  }
}

```



云端向客户端返回订阅结果的响应JSON字符串格式数据如下（`SV-BLKALPHA21-0001-123、101c1200240008101e0a00000141414100000000020000000000000000000000_online_DEV_EVENT`为示例数据）：

```  
{
  "cmd": "unsubscribe-ack",
  "data": {
    "code": "00000",
    "result": "success",
    "systemId": "SV-BLKALPHA21-0001-123",
    "desc":"Topic [101c1200240008101e0a00000141414100000000020000000000000000000000_online_DEV_EVENT] unsubscribed ok"
  }
}


```

##### 3、错误码

> 34001,34002,34003,34004,34005,34006,34999  



#### 查询当前system的订阅关系 

> 查询当前云应用的所有订阅关系，必须在建立连接成功的前提下进行，如果建立连接返回成功，才可以发起请求，如果失败，则无法进行操作。


##### 1、接口定义  

客户端向云端发送的JSON字符串格式数据如下： 

```  

{
  "cmd": "searchSystem"
}


```



云端向客户端返回当前system的订阅关系的响应JSON字符串格式数据如下（`DEV_EVENT、101c1200240008101e0a00000141414100000000020000000000000000000000`为示例数据）：  

```  
{
  "cmd": "searchSystem-ack",
  "data": {
    "code": "00000",
    "result": "success",
    "retdata": [
      {
        "topic": "DEV_EVENT",
        "keys": {
          "typeId": [
            "101c1200240008101e0a00000141414100000000020000000000000000000000"
          ]
        }
      }
    ]
  }
}

```

##### 3、错误码

> 34001,34002,34003,34004,34005,34006,34999  


#### 查询当前client的订阅关系接口

> 查询当前client的订阅关系，即查询当前client的当前system的订阅关系，必须在建立连接成功的前提下进行，如果建立连接返回成功，才可以发起请求，如果失败，则无法进行操作。  


##### 1、接口定义  

客户端向云端发送的JSON字符串格式数据如下:  

```  

{
  "cmd": "searchClient"
}


```



云端向客户端返回当前client的订阅结果的响应JSON字符串格式数据如下（`DEV_EVENT、101c1200240008101e0a00000141414100000000020000000000000000000000`为示例数据）： 

```  
{
  "cmd": "searchClient-ack",
  "data": {
    "code": "00000",
    "result": "success",
    "retdata": [
      {
        "topic": "DEV_EVENT",
        "keys": {
          "typeId": [
            "101c1200240008101e0a00000141414100000000020000000000000000000000"
          ]
        }
      }
    ]
  }
}

```

##### 3、错误码

> 34001,34002,34003,34004,34005,34006,34999  


#### 关闭连接功能  

> 关闭连接具体无交互接口，只需客户端关闭session即可。  

注意：SDK订阅是有具体接口的，此外当group下的所有订阅关系都取消后，Websocket订阅服务会自动检测到并关闭当前session连接。

#### 示例  

> 以下提供Client端简单功能示例代码，仅方便引导本地开发使用,具体可根据本地实际情况进行二次开发。  

**客户端Websocket实现包的pom依赖说明**   

可以选择一个具体的开源实现包，如Jetty的或Glassfish或Tomcat的，区别只是部分代码稍微不同。
  
Jetty的Websocket实现依赖包举例如下：  


	<dependency>
			<groupId>org.eclipse.jetty.websocket</groupId>
			<artifactId>javax-websocket-server-impl</artifactId>
			<version>9.3.0.RC0 </version>
	</dependency>


 
Glassfish的Websocket实现依赖包举例如下：  


	<dependency>
		<groupId>org.glassfish.tyrus</groupId>
		<artifactId>tyrus-container-jdk-client</artifactId>
		<version>1.14</version>
	</dependency>



Tomcat的Websocket实现依赖包举例如下：  


	<dependency>
		<groupId>org.apache.tomcat.embed</groupId>
		<artifactId>tomcat-embed-websocket</artifactId>
		<version>8.5.37</version>
	</dependency>



注：  
(1) 如上插件版本最低适用于JDK7环境,如果本地大于JDK7,也可酌情尝试插件最新版本。  
(2) 后续各示意代码以Jetty的Websocket实现依赖包为前提。  
(3) 如果本地项目是SpringBoot Web工程，因为其已经默认内嵌了Tomcat相关jar（同时包含了Tomcat针对Websocket的相关实现jar），所以不必在pom.xml中单独做Websocket相关引入，但必须注意Tomcat相关jar的引用范围，如下示意：  


	<dependency>
		  <groupId>org.springframework.boot</groupId>
		  <artifactId>spring-boot-starter-web</artifactId>
		  <!-- 移除嵌入式tomcat插件 -->
				<exclusions>
					<exclusion>
						<groupId>org.springframework.boot</groupId>
						<artifactId>spring-boot-starter-tomcat</artifactId>
					</exclusion>
				</exclusions>
	</dependency>
	<!-- \*\*打war包时加入此项， 告诉spring-boot tomcat相关jar包用外部tomcat服务器的，不要打进去\*\* -->
	<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-tomcat</artifactId>
			<scope>provided</scope>
	</dependency>


	
		  
		&lt;dependency&gt;  
			&emsp;&lt;groupId&gt;org.springframework.boot&lt;/groupId&gt;  
			&emsp;&lt;artifactId&gt;spring-boot-starter-web&lt;/artifactId&gt;  
			&emsp;&lt;!-- 移除嵌入式tomcat插件 --&gt;  
		 			&emsp;&emsp;&lt;exclusions&gt;  
						&emsp;&emsp;&emsp;&lt;exclusion&gt;  
							&emsp;&emsp;&emsp;&emsp;&lt;groupId&gt;org.springframework.boot&lt;/groupId&gt;  
							&emsp;&emsp;&emsp;&emsp;&lt;artifactId&gt;spring-boot-starter-tomcat&lt;/artifactId&gt;  
						&emsp;&emsp;&emsp;&lt;/exclusion&gt;  
					&emsp;&emsp;&lt;/exclusions&gt;  
		&lt;/dependency&gt;  
		&lt;!-- <font color="#FF0000">打war包时加入此项， 告诉spring-boot tomcat相关jar包用外部tomcat服务器的，不要打进去</font> --&gt;  
		&lt;dependency&gt;  
				&emsp;&lt;groupId&gt;org.springframework.boot&lt;/groupId&gt;  
				&emsp;&lt;artifactId&gt;spring-boot-starter-tomcat&lt;/artifactId&gt;  
				&emsp;<font color="#FF0000">&lt;scope&gt;provided&lt;/scope&gt;</font>  
		&lt;/dependency&gt;  
	
	

[^-^]:常用图片注释
[dataSubscription_type]:_media/_dataSubscription/dataSubscription_type.png
[dataSubscription_liucheng]:_media/_dataSubscription/dataSubscription_liucheng.png
[dataS_flow]:_media/_dataSubscription/dataS_flow.png  
[dataS_model]:_media/_dataSubscription/dataS_model.png
