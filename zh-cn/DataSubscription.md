
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
|数据订阅|`wss://uws.haier.net/wssubscriber`|`221.122.92.13`|  

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


### 建立连接  
> 提供建立连接的功能。客户端发起连接请求，数据订阅系统会对客户端连接请求进行鉴权后，建立与客户端的连接。   
> 客户端发起连接请求，请求时携带对应应用的systemId和systemKey签名信息进行鉴权，鉴权成功返回鉴权结果，鉴权失败，则返回错误信息并断开连接。 

#### 1、接口定义
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

#### 2、请求样例

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

#### 3、状态码

状态码|描述 
:-|:-
00000|成功
00001|失败
   

### 订阅接口  
> 订阅指定topic消息，消息订阅必须在建立连接成功的前提下进行，如果建立连接返回成功，才可以发送订阅，如果失败，则无法进行订阅。  

注意：Websocket订阅服务的订阅范围只针对详细订阅，不包括全量订阅，这点和SDK是有区别的。  

#### 1、接口定义  

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

#### 2、错误码

> 34001,34002,34003,34004,34005,34006,34999


### 取消订阅接口  
> 取消订阅指定topic消息，取消订阅必须在建立连接成功的前提下进行，如果建立连接返回成功，才可以发取消订阅，如果失败，则无法进行取消订阅。


#### 1、接口定义  

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

#### 2、错误码

> 34001,34002,34003,34004,34005,34006,34999  



### 查询当前system的订阅关系 

> 查询当前云应用的所有订阅关系，必须在建立连接成功的前提下进行，如果建立连接返回成功，才可以发起请求，如果失败，则无法进行操作。


#### 1、接口定义  

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

#### 2、错误码

> 34001,34002,34003,34004,34005,34006,34999  


### 查询当前client的订阅关系接口

> 查询当前client的订阅关系，即查询当前client的当前system的订阅关系，必须在建立连接成功的前提下进行，如果建立连接返回成功，才可以发起请求，如果失败，则无法进行操作。  


#### 1、接口定义  

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

#### 2、错误码

> 34001,34002,34003,34004,34005,34006,34999  


### 关闭连接功能  

> 关闭连接具体无交互接口，只需客户端关闭session即可。  

注意：SDK订阅是有具体接口的，此外当group下的所有订阅关系都取消后，Websocket订阅服务会自动检测到并关闭当前session连接。

### 示例  

> 以下提供Client端简单功能示例代码，仅方便引导本地开发使用,具体可根据本地实际情况进行二次开发。  

**1、 客户端Websocket实现包的pom依赖说明**   

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



**2、 初始化客户端建立连接**   

String uri = "wss://uws.haier.net/wssubscriber/msgplatform/websocket?systemId=SV-JCY-TEST&timestamp=1491013448629&sign=a9d30dcea2fc1def236b0dca91bf0d6b5cb2e25f1bc8cbfb588a07f8b59dfb22&isSubscribeEarliestMessage=false";</br>
MsgWebSocketClient msgWebSocketClient;</br>
try {</br>
	&emsp;// 。。。。</br>
	&emsp;// 建立连接</br>
	&emsp;<font color="#FF0000">msgWebSocketClient = new MsgWebSocketClient(uri);</font></br>
	&emsp;// 。。。。</br>
	} catch (Exception e) {</br>
		&emsp;&emsp;msgWebSocketClient = null;</br>
		&emsp;&emsp;e.printStackTrace();</br>
		&emsp;&emsp;return false;</br>
	&emsp;}</br>
	&emsp;return true;</br>
}</br>




**3、 订阅详细消息**   

	
//组织订阅消息JSON</br>
String cmdInfo = "{'cmd':'<font color="#FF0000">subscribe</font>','data':{'subdata':[{'topic':'DEV_STATUS','keys':{'typeId':['201c51890c31c3080401c7f78d41b400000054d990e09c6826eaaf0e08405d40']}}]}}";</br> 
MsgWebSocketClient msgWebSocketClient;</br>
try {</br>
	&emsp;// 建立连接</br>
	&emsp;msgWebSocketClient = new MsgWebSocketClient(uri);</br>
	&emsp;// 发送订阅信息</br>
	&emsp;<font color="#FF0000">msgWebSocketClient.addCmdInfo(cmdInfo);</font></br>
} catch (Exception e) {</br>
		&emsp;msgWebSocketClient = null;</br>
		&emsp;e.printStackTrace();</br>
		&emsp;return false;</br>
}</br>
return true;</br>  


**4、 取消订阅详细消息**   

//组织取消订阅消息JSON</br>
String cmdInfo = "{'cmd':'<font color="#FF0000">unsubscribe</font>','data':{'subdata':[{'topic':'DEV_EVENT','keys':{'typeId':['901c1200240008101e0a00000141414100000000020000000000000000000000']}}]}}"</br>
MsgWebSocketClient msgWebSocketClient;</br>
		try {</br>
			&emsp;&emsp;// 建立连接</br>
			&emsp;&emsp;msgWebSocketClient = new MsgWebSocketClient(uri);</br>
			&emsp;&emsp;// 发送订阅信息</br>
			&emsp;&emsp;<font color="#FF0000">msgWebSocketClient.addCmdInfo(cmdInfo);</font></br>
		} catch (Exception e) {</br>
			&emsp;&emsp;msgWebSocketClient = null;</br>
			&emsp;&emsp;e.printStackTrace();</br>
			&emsp;&emsp;return false;</br>
		}</br>
return true;</br>


**5、 查询当前客户端订阅关系**  	  

//组织查询当前客户端订阅关系消息JSON</br>
String cmdInfo = "{'cmd':'<font color="#FF0000">searchClient</font>'}";</br>
MsgWebSocketClient msgWebSocketClient;</br>
		&emsp;try {</br>
			&emsp;&emsp;// 建立连接</br>
			&emsp;&emsp;msgWebSocketClient = new MsgWebSocketClient(uri);</br>
			&emsp;&emsp;// 发送订阅信息</br>
			&emsp;&emsp;<font color="#FF0000">msgWebSocketClient.addCmdInfo(cmdInfo);</font></br>
		&emsp;} catch (Exception e) {</br>
			&emsp;&emsp;msgWebSocketClient = null;</br>
			&emsp;&emsp;e.printStackTrace();</br>
			&emsp;&emsp;return false;</br>
		}</br>
	return true;</br>  


**6、 查询当前客户对应的systemId下的所有订阅关系**  	

//组织查询当前客户对应的systemId下的所有订阅关系消息JSON</br>
String cmdInfo = "{'cmd':'<font color="#FF0000">searchSystem</font>'}";</br>
MsgWebSocketClient msgWebSocketClient;</br>
		try {</br>
			&emsp;// 建立连接</br>
			&emsp;msgWebSocketClient = new MsgWebSocketClient(uri);</br>
			&emsp;// 发送订阅信息</br>
			&emsp;<font color="#FF0000">msgWebSocketClient.addCmdInfo(cmdInfo);</font></br>
		} catch (Exception e) {</br>
			&emsp;msgWebSocketClient = null;</br>
			&emsp;e.printStackTrace();</br>
			&emsp;return false;</br>
		}</br>
		return true;</br>  


**7、 关闭当前客户端连接**  	

MsgWebSocketClient msgWebSocketClient;</br>
		try {</br>
			&emsp;// 建立连接</br>
			&emsp;msgWebSocketClient = new MsgWebSocketClient(uri);</br>
			&emsp;//关闭连接</br>
			&emsp;<font color="#FF0000">msgWebSocketClient.close();</font></br>
		} catch (Exception e) {</br>
			&emsp;msgWebSocketClient = null;</br>
			&emsp;e.printStackTrace();</br>
			&emsp;return false;</br>
		}</br>
return true;</br>  

注：如果每次建立连接后只是进行查询等一次性操作（即不需要连接长时间保留），则建议操作完后关闭当前链接，同时不建议使用心跳。  

**8、 整示例代码（可复用）**  	

代码功能：  
(1)	支持Client端订阅消息。  
(2)	支持心跳检测，心跳功能简单支持当客户端和服务器端连接处于无数据交互状态时才发送心跳检测消息，有数据交互时不发送。  
(3)	支持连接断开后每55s进行一次重连尝试。  


import 引用头：  

```
import java.net.URI;
import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;
import java.util.concurrent.atomic.AtomicBoolean;
import java.util.concurrent.atomic.AtomicInteger;
import java.util.concurrent.atomic.AtomicLong;
import javax.websocket.ClientEndpoint;
import javax.websocket.ContainerProvider;
import javax.websocket.OnClose;
import javax.websocket.OnMessage;
import javax.websocket.Session;
import javax.websocket.WebSocketContainer;
import javax.websocket.CloseReason;
import org.eclipse.jetty.util.component.LifeCycle;


```

整体示例代码如下（可复用），建立连接代码见红色标注部分  

  public class TestSubscribeMsg {</br>
  &emsp;public static AtomicInteger reConnectCount = new AtomicInteger(0);</br>
  &emsp;public static void main(final String[] args) {</br>
  &emsp;&emsp;buildConnectionAndSubscribeMsg();// 简单建立连接并订阅信息</br>
  &emsp;}</br>
  &emsp;private static boolean buildConnectionAndSubscribeMsg() {</br>
  &emsp;&emsp;String uri = "wss://uws.haier.net/wssubscriber/msgplatform/websocket?systemId=SV-JCY-TEST&timestamp=1491013448629&sign=a9d30dcea2fc1def236b0dca91bf0d6b5cb2e25f1bc8cbfb588a07f8b59dfb22&isSubscribeEarliestMessage=false";</br>
  &emsp;&emsp;String cmdInfo = "{'cmd':'subscribe','data':{'subdata':[{'topic':'DEV_STATUS','keys':{'typeId':['201c51890c31c3080401c7f78d41b400000054d990e09c6826eaaf0e08405d40']}}]}}"; </br>
  &emsp;&emsp;MsgWebSocketClient msgWebSocketClient;</br>
  &emsp;&emsp;try {</br>
  &emsp;&emsp;&emsp;// 建立连接</br>
  &emsp;&emsp;&emsp;<font color="#FF0000">msgWebSocketClient = new MsgWebSocketClient(uri);</font></br>
  &emsp;&emsp;&emsp;// 发送订阅信息</br>
  &emsp;&emsp;&emsp;<font color="#FF0000">msgWebSocketClient.addCmdInfo(cmdInfo);</font></br>
  &emsp;&emsp;} catch (Exception e) {</br>
  &emsp;&emsp;&emsp;msgWebSocketClient = null;</br>
  &emsp;&emsp;&emsp;e.printStackTrace();</br>
  &emsp;&emsp;&emsp;return false;</br>
  &emsp;&emsp;}</br>
  &emsp;&emsp;return true;</br>
  &emsp;}</br>
  &emsp;<font color="#FF0000">@ClientEndpoint</font></br>
  &emsp;public static class MsgWebSocketClient {</br>
  &emsp;&emsp;// 最新收消息时间,心跳发消息时做时间间隔计算</br>
  &emsp;&emsp;public static AtomicLong lastReceiveTime = null;</br>
  &emsp;&emsp;// 是否继续心跳标志位</br>
  &emsp;&emsp;public static AtomicBoolean isHeartBeatContinue = null;</br>
  &emsp;&emsp;private DateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd hh:mm:ss");</br>
  &emsp;&emsp;// 创建容器对象（建立连接时要用）</br>
  &emsp;&emsp;private WebSocketContainer container = ContainerProvider.getWebSocketContainer();</br>
  &emsp;&emsp;// 一个连接对应一个session</br>
  &emsp;&emsp;private Session session = null;</br>
  &emsp;&emsp;// 心跳线程池</br>
  &emsp;&emsp;private ExecutorService executorService = null;</br>
  &emsp;&emsp;// 关闭心跳线程要用到</br>
  &emsp;&emsp;private Future<?> future = null;</br>
  
  &emsp;&emsp;// 无参构造方法(必须)</br>
  &emsp;&emsp;public MsgWebSocketClient() {</br>
  &emsp;&emsp;}</br>
  
  &emsp;&emsp;public MsgWebSocketClient(String uri) throws Exception {</br>
  &emsp;&emsp;&emsp;session = container.connectToServer(MsgWebSocketClient.class, URI.create(uri));</br>
  &emsp;&emsp;&emsp;if (session.isOpen()) {</br>
  &emsp;&emsp;&emsp;&emsp;executorService = Executors.newFixedThreadPool(1);</br>
  &emsp;&emsp;&emsp;&emsp;lastReceiveTime = new AtomicLong(System.currentTimeMillis());</br>
  &emsp;&emsp;&emsp;&emsp;isHeartBeatContinue = new AtomicBoolean(true);</br>
  &emsp;&emsp;&emsp;&emsp;session.setMaxIdleTimeout(0);// 值为0表示会话连接不会因为长时间无数据交互而超时</br>
  &emsp;&emsp;&emsp;&emsp;startHeartBeat();// 启动心跳</br>
  &emsp;&emsp;&emsp;} else {</br>
  &emsp;&emsp;&emsp;&emsp;container = null;</br>
  &emsp;&emsp;&emsp;}</br>
  &emsp;&emsp;}</br>
  
  &emsp;&emsp;@OnMessage</br>
  &emsp;&emsp;public void onMessage(final String message) {</br>
  &emsp;&emsp;&emsp;long currentTime = System.currentTimeMillis();</br>
  &emsp;&emsp;&emsp;// 更新最新收消息时间</br>
  &emsp;&emsp;&emsp;lastReceiveTime.set(currentTime);</br>
  &emsp;&emsp;&emsp;String currentDateStr = dateFormat.format(currentTime);</br>
  &emsp;&emsp;&emsp;if (message.contains("-ack")) {</br>
  &emsp;&emsp;&emsp;&emsp;System.out.println((currentDateStr + " 收到系统答信息：" + message));</br>
  &emsp;&emsp;&emsp;} else {</br>
  &emsp;&emsp;&emsp;&emsp;System.out.println((currentDateStr + " 收到 message：" + message));</br>
  &emsp;&emsp;&emsp;}</br>
  &emsp;&emsp;}</br>
  &emsp;&emsp;@OnClose</br>
  &emsp;&emsp;public void onClose(Session session, CloseReason closeReason) {</br>
  &emsp;&emsp;&emsp;System.out.println("Connection closed!");</br>
  &emsp;&emsp;&emsp;stopHeartBeat();</br>
  &emsp;&emsp;&emsp;stopContainer();</br>
  &emsp;&emsp;&emsp;try {</br>
  &emsp;&emsp;&emsp;&emsp;if (closeReason.getCloseCode().getCode() == 1006) {// 连接异常关闭CLOSED_ABNORMALLY(1006)</br>
  &emsp;&emsp;&emsp;&emsp;&emsp;System.out.println("Try restarting the connection after 5s...");</br>
  &emsp;&emsp;&emsp;&emsp;&emsp;Thread.sleep(5000);</br>
  &emsp;&emsp;&emsp;&emsp;&emsp;boolean isSuccess = buildConnectionAndSubscribeMsg();</br>
  &emsp;&emsp;&emsp;&emsp;&emsp;if (!isSuccess) {</br>
  &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;System.out.println("Try restarting the connection after 5min...");</br>
  &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;Thread.sleep(300000);</br>
  &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;isSuccess = buildConnectionAndSubscribeMsg();</br>
  &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;if (!isSuccess) {</br>
  &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;System.out.println("Try restarting the connection after 24h...");</br>
  &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;Thread.sleep(86400000);</br>
  &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;buildConnectionAndSubscribeMsg();</br>
  &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;}</br>
  &emsp;&emsp;&emsp;&emsp;&emsp;}</br>
  &emsp;&emsp;&emsp;&emsp;}</br>
  &emsp;&emsp;&emsp;} catch (Exception e) {</br>
  &emsp;&emsp;&emsp;&emsp;e.printStackTrace();</br>
  &emsp;&emsp;&emsp;}</br>
  &emsp;&emsp;}</br>
  
  &emsp;&emsp;public void addCmdInfo(final String cmdInfo) {</br>
  &emsp;&emsp;&emsp;new Thread() {</br>
  &emsp;&emsp;&emsp;&emsp;public void run() {</br>
  &emsp;&emsp;&emsp;&emsp;&emsp;try {</br>
  &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;if (session.isOpen()) {</br>
  &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;session.getBasicRemote().sendObject(cmdInfo);</br>
  &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;}</br>
  &emsp;&emsp;&emsp;&emsp;&emsp;} catch (Exception e) {</br>
  &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;e.printStackTrace();</br>
  &emsp;&emsp;&emsp;&emsp;&emsp;}</br>
  &emsp;&emsp;&emsp;&emsp;}</br>
  &emsp;&emsp;&emsp;}.start();</br>
  &emsp;&emsp;}</br>
  
  &emsp;&emsp;public void close() {</br>
  &emsp;&emsp;&emsp;try {</br>
  &emsp;&emsp;&emsp;&emsp;stopHeartBeat(); // 关闭心跳</br>
  &emsp;&emsp;&emsp;&emsp;session.close(new CloseReason(CloseReason.CloseCodes.NORMAL_CLOSURE, "")); // 关闭session连接</br>
  &emsp;&emsp;&emsp;&emsp;stopContainer(); // 关闭容器</br>
  &emsp;&emsp;&emsp;} catch (Exception e) {</br>
  &emsp;&emsp;&emsp;&emsp;e.printStackTrace();</br>
  &emsp;&emsp;&emsp;}</br>
  &emsp;&emsp;}</br>
  
  &emsp;&emsp;private void stopContainer() {</br>
  &emsp;&emsp;    /\*\*</br>
  &emsp;&emsp;&emsp; \* 如果项目引用的是Jetty的websocket实现，则采用如下代码</br>
  &emsp;&emsp;&emsp; \* import org.eclipse.jetty.util.component.LifeCycle;</br>
  &emsp;&emsp;&emsp; \*/</br>
  &emsp;&emsp;&emsp;if (container instanceof LifeCycle) {</br>
  &emsp;&emsp;&emsp;&emsp;try {</br>
  &emsp;&emsp;&emsp;&emsp;&emsp;((LifeCycle) container).stop();</br>
  &emsp;&emsp;&emsp;&emsp;} catch (Exception e) {</br>
  &emsp;&emsp;&emsp;&emsp;&emsp;e.printStackTrace();</br>
  &emsp;&emsp;&emsp;&emsp;}</br>
  &emsp;&emsp;&emsp;}</br>
  &emsp;&emsp;&emsp;
  &emsp;&emsp;&emsp;/\*\*</br>
  &emsp;&emsp;&emsp; \* 如果项目引用的是Glassfish的websocket实现，则采用如下代码</br>
  &emsp;&emsp;&emsp; \* import org.glassfish.tyrus.client.ClientManager;</br>
  &emsp;&emsp;&emsp; \*/</br>
  //&emsp;&emsp;&emsp;if (container instanceof ClientManager) {</br>
  //&emsp;&emsp;&emsp;&emsp;try {</br>
  //&emsp;&emsp;&emsp;&emsp;&emsp;((ClientManager) container).shutdown();</br>
  //&emsp;&emsp;&emsp;&emsp;} catch (Exception e) {</br>
  //&emsp;&emsp;&emsp;&emsp;&emsp;e.printStackTrace();</br>
  //&emsp;&emsp;&emsp;&emsp;}</br>
  //&emsp;&emsp;&emsp;}</br>
  &emsp;&emsp;&emsp;
  &emsp;&emsp;&emsp;/\*\*</br>
  &emsp;&emsp;&emsp; \* 如果项目引用的是Tomcat的websocket实现，则采用如下代码</br>
  &emsp;&emsp;&emsp; \* import org.apache.tomcat.websocket.WsWebSocketContainer;</br>
  &emsp;&emsp;&emsp; \*/</br>
  //&emsp;&emsp;&emsp;if(container instanceof WsWebSocketContainer){</br>
  //&emsp;             try {</br>
  //&emsp;&emsp;&emsp;&emsp;&emsp;((WsWebSocketContainer) container).destroy();</br>
  //&emsp;&emsp;&emsp;&emsp; } catch (Exception e) {</br>
  //&emsp;&emsp;&emsp;&emsp;&emsp;e.printStackTrace();</br>
  //&emsp;&emsp;&emsp;&emsp; }&emsp;</br>
  //&emsp;&emsp;&emsp;}</br>
  }
  
  &emsp;&emsp;private void startHeartBeat() {</br>
  &emsp;&emsp;&emsp;future = executorService.submit(new HeartBeat());</br>
  &emsp;&emsp;}</br>
  
  &emsp;&emsp;private void stopHeartBeat() {</br>
  &emsp;&emsp;&emsp;if (future != null) {</br>
  &emsp;&emsp;&emsp;&emsp;try {</br>
  &emsp;&emsp;&emsp;&emsp;&emsp;isHeartBeatContinue.set(false);</br>
  &emsp;&emsp;&emsp;&emsp;&emsp;future.cancel(true);</br>
  &emsp;&emsp;&emsp;&emsp;&emsp;executorService.shutdown();</br>
  &emsp;&emsp;&emsp;&emsp;} catch (Exception e) {</br>
  &emsp;&emsp;&emsp;&emsp;&emsp;e.printStackTrace();</br>
  &emsp;&emsp;&emsp;&emsp;}</br>
  &emsp;&emsp;&emsp;}</br>
  &emsp;&emsp;}</br>
  
  &emsp;&emsp;private class HeartBeat implements Runnable {</br>
  &emsp;&emsp;&emsp;// 心跳间隔时间，具体根据实际情况设置,如nginx默认websocket超时时间是60s，则可以在连接无数据交互情况持续到快过期前发送一次心跳（这里设置55s时）</br>
  &emsp;&emsp;&emsp;private long heartBeatInterval = 55000;</br>
  
  &emsp;&emsp;&emsp;public void run() {</br>
  &emsp;&emsp;&emsp;&emsp;try {</br>
  &emsp;&emsp;&emsp;&emsp;&emsp;while (isHeartBeatContinue.get()) {</br>
  &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;long startTime = System.currentTimeMillis();</br>
  &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;if (startTime - lastReceiveTime.get() > heartBeatInterval) {</br>
  &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;if (session.isOpen()) {</br>
  &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;String cmdInfo = "{'cmd': 'keepAlive'}";</br>
  &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;session.getBasicRemote().sendObject(cmdInfo);</br>
  &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;// 更新最新收消息时间</br>
  &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;lastReceiveTime.set(System.currentTimeMillis());</br>
  &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;}</br>
  &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;}</br>
  &emsp;&emsp;&emsp;&emsp;&emsp;}</br>
  &emsp;&emsp;&emsp;&emsp;} catch (Exception e) {</br>
  &emsp;&emsp;&emsp;&emsp;&emsp;e.printStackTrace();</br>
  &emsp;&emsp;&emsp;&emsp;}</br>
  &emsp;&emsp;&emsp;}</br>
  &emsp;&emsp;}</br>
  &emsp;}</br>
  }</br>

注:  
(1)	心跳的JSON字符串格式如下：  
{"cmd": "keepAlive"}  
服务端相应如下（红色部分为示例数据）：  
{"cmd":"keepAlive-ack","data":{"code":"00000","result":"success","systemId":"<font color="#FF0000">SV-BLKALPHA21-0001-124</font>"}}  
(2)	心跳根据实际情况决定是否启用，一般适用订阅数据等需要保持长连接场景，如果非长连接场景，如建立一次连接只是为简单查询订阅关系则可酌情是否需要。  
(3)	如果项目引用的是Tomcat的Websocket实现，则项目中还必须创建如下解码器类代码，同时示例代码中注解<font color="#FF0000">@ClientEndpoint改为@ClientEndpoint(encoders = {TextEncoder.class})</font>  

```
import javax.websocket.EncodeException;
import javax.websocket.Encoder;
import javax.websocket.EndpointConfig;

public class TextEncoder  implements Encoder.Text<String>{
	@Override
	public void init(EndpointConfig config) {
	}

	@Override
	public void destroy() {
	}

	@Override
	public String encode(String object) throws EncodeException {
		return object;
	}
}


```


## 返回码列表公共错误码   

| **错误码** | **描述** |  
| :-------------: |:-------------:|  
|34001|Illegal parameters.（参数非法）|  
|34002|The type information obtained is illegal. （获取到的类型信息非法）|  
|34003|Add subscribe relationship fail. （添加订阅关系鉴权失败）|  
|34004|Delete subscribe relationship fail. （取消订阅关系鉴权失败）|  
|34005|Illegal result of search. （查询结果异常）|  
|34006|Invalid connection. （连接异常）|  
|34999|UnKnown error. （未知异常）|  


[^-^]:常用图片注释
[dataSubscription_type]:_media/_dataSubscription/dataSubscription_type.png
[dataSubscription_liucheng]:_media/_dataSubscription/dataSubscription_liucheng.png
[dataS_flow]:_media/_dataSubscription/dataS_flow.png  
[dataS_model]:_media/_dataSubscription/dataS_model.png  







