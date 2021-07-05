# 设备数据订阅

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

设备数据权限根据设备种类进行订阅，由typeid限定订阅消息的设备类型，由消息topic限定订阅的数据种类

**业务处理**

数据订阅参考接口，开发消息接收端，完成测试、联调、上线。通过审核后即可在生产环境正常订阅设备消息数据



## 公共说明  

### 接入方式  

本服务提供的所有接口仅支持wss协议请求。  
在开发、联调时，应用开发者应该连接开发者环境，通过配置应用开发者访问外网的路由器（设置路由器的dns），可以连接到不同的开发环境。  

### 接入地址  

| **接口分类** | **接入地址** |**备注**|
| :-------------: |:-------------:|:-----:|
|数据订阅|`wss://mp-stp.haier.net/wsserver`|外网域名|
|数据订阅|`ws://internal.mp-stp.haier.net/wsserver`|内网域名|
|数据订阅|`wss://mp-stp-kfz.haier.net/wsserver`|开发者环境接入地址|
**说明**
>请优先使用内网域名进行访问，内网域名访问不通时再使用外网域名。内网会更加稳定，访问服务速度也更快。

### 签名认证  

**说明**  

>调用方需要对发送到wss的请求进行签名，执行签名计算的签名值需要赋值到URL的sign属性，以便服务端进行签名验证。  

**参数介绍**  

待签名字符串为： systemId + systemKey + timestamp；   
systemId：应用ID，40位以内字符,Haier U+ 云平台全局唯一；  
systemKey：在海极网给应用申请的systemKey，不能明文发送；  
timestamp：Unix时间戳，精确到毫秒；  


**算法**  


签名算法就是对待签名字符串计算32位小写SHA-256值，算法示例:  

```java  
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
### 公共错误码   

| **错误码** | **描述** |
| :-------------: |:-------------:|
|34001|Illegal parameters.（参数非法）|
|34003|Add subscribe relationship fail. （添加订阅关系鉴权失败）|
|34004|Delete subscribe relationship fail. （取消订阅关系鉴权失败）|
|34006|Invalid connection. （连接异常）|
|34999|UnKnown error. （未知异常）|

### TOPIC说明
|**Topic编码**|**Topic名称** |**Topic描述** |**Topic类型** |**Topic离线客户端消息缓存时间** |
| :-------------:|:-------------:|:-------------:| :-------------:|:-------------:|
|DEV_AUTH|	设备授权|	设备授权类消息，包括设备被用户绑定授权、设备被用户解绑授权的实时消息。|	设备|24小时|
|DEV_EVENT|	设备事件|	物联设备与网关的连接事件类消息，包括设备上线（与网关建立连接）、设备下线（与网关断开连接）的实时消息。|设备|24小时|
|DEV_FAULT|	设备告警|	物联设备告警消息，主要是设备上报的告警实时消息。|	设备|24小时|
|DEV_OPERATION|	设备操控|	物联设备操作控制相关消息，包括设备操作下发、设备应答的实时消息。|	设备|24小时|
|DEV_STATUS|设备状态上报|	物联设备上报的状态消息，主要包括物联设备状态发生变化时上报的最新状态消息。|设备|24小时|
|DEV_BIGDATA|设备大数据上报|	物联设备上报的大数据相关的消息，主要包括物联设备传感器类设备状态发生变化时上报的最新状态消息。|设备|24小时|
|DEV_CUSTOM|设备自定义内容上报|	物联设备上报的自定义消息，主要包括业务上行、数据上行实时消息。|设备|24小时|
|DEV_FIRMWARE|设备固件信息上报|	物联设备固件相关信息，包括设备升级指令下发、设备升级应答、设备版本信息的实时消息。|设备|24小时|
|DEV_COMMON_EVENT|设备公共事件|	设备公共事件相关信息。|设备|24小时|


## 接口列表  

 ![数据订阅场景流程][dataS_model]  

客户端向服务端发起连接请求，服务端对连接请求参数进行鉴权，鉴权失败，则断开连接并返回客户端鉴权失败信息；鉴权成功，则建立通道。  

鉴权成功后，客户端可多次向服务端发起订阅关系，多次订阅同一个topic的key组合，服务端则自动合并为一个订阅，默认为同一个客户端订阅；  

针对同一个云服务，发起多次连接，服务端则认为是多个客户端发起请求，所有消息将平衡分发到多个客户端上，每个云服务最多可发起连接数为系统可配置参数。  

取消订阅，则删除订阅关系，服务端返回客户端取消响应结果。  


### 建立连接

**使用说明**

> 提供建立连接的功能。客户端发起连接请求，数据订阅系统会对客户端连接请求进行鉴权后，建立与客户端的连接。   
> 客户端发起连接请求，请求时携带对应应用的systemId和systemKey签名信息进行鉴权，鉴权成功返回鉴权结果，鉴权失败，则返回错误信息并断开连接。 

**接口描述**  

?> **接入地址：** `/websocket`</br>

**输入参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
systemId|String|url|必填|应用ID，50位以内字符,Haier U+ 云平台全局唯一  
timestamp|long|url|必填|Unix时间戳，精确到毫秒  
sign|String|url|必填|对请求进行签名运算产生的签名 见签名算法   
resetTime|Integer|url|选填|指定消息消费的时间，以分钟为单位。取值范围：可以最多指定到2个小时之前的数据（0-120）；不传当前参数时，从最后一次消费的时间开始消费; 当传入时间时，从指定时间开始消费消息。如：传入0时，从当前时间开始消费；传入60，从当前时间1小时前开始消费 

**输出参数**

参数名|类型|位置|必填|说明
:-|:-:|:-:|:-:|:-
cmd|String|body|必填|操作说明
data|String|body|必填|响应结果

**示例**

**请求地址**

```java  
wss://mp-stp.haier.net/wsserver/websocket?systemId=SV-BLKALPHA21-0001-123&timestamp=1491013448629&sign=c70500c16563b5ccc7d032831bff34a5cb02c147ca6beeffff54d22d262a319e
```

**用户请求** 无  

**请求应答**

```json  
{
    "cmd":"authenticate-ack",
    "data":{
        "code":"00000",
        "result":"success"
    }
}
```

**状态码**

状态码|描述 
:-|:-
00000|成功
00001|失败


### 订阅接口

**使用说明**

> 订阅指定topic消息，消息订阅必须在建立连接成功的前提下进行，如果建立连接返回成功，才可以发送订阅，如果失败，则无法进行订阅。  

**接口描述**  
客户端向云端发送的JSON字符串格式数据如下：  

```json  

{
    "cmd":"subscribe",
    "topics":[
        "DEV_STATUS",
        "DEV_BIGDATA"
    ]
}

```
说明  
>
(1)	客户端一个连接情况下只能发起一次订阅消息(服务器端做了限制，多发不起作用)，订阅信息中的多个topic多个keys，其中如果有任何一个订阅验证失败，则本次请求全部订阅均失败，只有当全部topic的keys订阅成功，则本次订阅成功。  
(2)	订阅端一次最多能发起500个订阅关系（约typeId数乘以topic数）。

云端向客户端返回订阅结果的响应JSON字符串格式数据如下：

```json  
{
    "cmd":"subscribe-ack",
    "data":{
        "code":"00000",
        "result":"success",
        "desc":"subscribed ok"
    }
}
```

**错误码**

> 34001, 34003,34006, 34999


### 取消订阅接口

**使用说明**

> 取消订阅指定topic消息，取消订阅必须在建立连接成功的前提下进行，如果建立连接返回成功，才可以发取消订阅，如果失败，则无法进行取消订阅。


**接口描述**  
客户端向云端发送的JSON字符串格式数据如下：
```json  
{
    "cmd":"unsubscribe",
    "topics":[
        "DEV_STATUS",
        "DEV_BIGDATA"
    ]
}

```
说明  
>
(1)	客户端调用取消订阅接口后，服务端将不再存储取消订阅topic的消息。当客户端重新订阅topic后，服务端向客户端推送当前最新消息。如果客户端想保留历史消息，直接关闭当前session连接即可。  

云端向客户端返回订阅结果的响应JSON字符串格式数据如下：

```json  
{
    "cmd":"unsubscribe-ack",
    "data":{
        "code":"00000",
        "result":"success",
        "desc":"unsubscribed ok"
    }
}

```

**错误码**

> 34001, 34004, 34006, 34999  


### 关闭连接功能

**使用说明**  

> 关闭连接具体无交互接口，只需客户端关闭session即可。  

注意：SDK订阅是有具体接口的，此外当group下的所有订阅关系都取消后，Websocket订阅服务会自动检测到并关闭当前session连接。


### 示例  

> 以下提供Client端简单功能示例代码，仅方便引导本地开发使用,具体可根据本地实际情况进行二次开发。  

#### 1、 客户端Websocket实现包的pom依赖说明   

Tomcat的Websocket实现依赖包举例如下：  

```xml
<dependency>
	<groupId>org.apache.tomcat.embed</groupId>
	<artifactId>tomcat-embed-websocket</artifactId>
	<version>9.0.35</version>
</dependency>
```


说明：  
>
(1)	如上插件版本最低适用于JDK8环境。  
(2)	如果本地项目是SpringBoot Web工程，因为其已经默认内嵌了Tomcat相关jar（同时包含了Tomcat针对Websocket的相关实现jar），所以不必在pom.xml中单独做Websocket相关引入，但必须注意Tomcat相关jar的引用范围，如下示意：

```xml
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
<!-- 打war包时加入此项， 告诉spring-boot tomcat相关jar包用外部tomcat服务器的，不要打进去 -->
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-tomcat</artifactId>
	<scope>provided</scope>
</dependency>

```


#### 2、 初始化客户端建立连接

```java
        try {
            String url = "wss://mp-stp.haier.net/wsserver/ /websocket?systemId=SV-IOTFWNCZY628-0000&timestamp=1598929399737&sign=a8fecc091d188bceb5efca840e42337c8d3e5a7c4ee6dcf5fa68f58596dd3ca0"
            WebsocketSubscriberClient client = new WebsocketSubscriberClient(url,cmdInfos,processor);
        } catch (Exception e) {
            e.printStackTrace();
        }

```



####  3、 订阅详细消息

```java
        // 组织订阅消息JSON
        try {
            String cmdInfo = "{\"cmd\":\"subscribe\", \"topics\":[\"DEV_STATUS\", \"DEV_BIGDATA\"]}";
            List<String> cmdInfos = new ArrayList<>();
            cmdInfos.add(cmdInfo);
            WebsocketSubscriberClient client = new WebsocketSubscriberClient(url, cmdInfos, processor);
            client.addCmdInfo(cmdInfo);
        } catch (Exception e) {
            e.printStackTrace();
        }
```

####  4、 取消订阅详细消息   

```java
        //组织取消订阅消息JSON
        try {
            String cmdInfo= "{\"cmd\":\"unsubscribe\", \"topics\":[\"DEV_STATUS\", \"DEV_BIGDATA\"]}";
            WebsocketSubscriberClient client = new WebsocketSubscriberClient(url,cmdInfos,processor);
            client.addCmdInfo(cmdInfo);
        } catch (Exception e) {
            e.printStackTrace();
        }



```

#### 5、 关闭当前客户端连接

```java
        try {

            WebsocketSubscriberClient client = new WebsocketSubscriberClient(url, cmdInfos, processor);
            client.close();
        } catch (Exception e) {
            e.printStackTrace();
        }

```

注：如果每次建立连接后只是进行查询等一次性操作（即不需要连接长时间保留），则建议操作完后关闭当前链接，同时不建议使用心跳。  

####  6、 整示例代码（可复用）

代码功能：  
>
(1)	支持Client端订阅消息。  
(2)	支持心跳检测，心跳功能简单支持当客户端和服务器端连接处于无数据交互状态时才发送心跳检测消息，有数据交互时不发送。支持连接断开后指定时间进行重连尝试。  

```java
import com.haier.iot.business.msgplatform.websocket.client.MsgProcessor;
import com.haier.iot.business.msgplatform.websocket.client.WebsocketSubscriberClient;
import java.util.ArrayList;
import java.util.List;

public class TestSubscribeMsg {
    public static void main(String[] args) {
        buildConnectionAndSubscribeMsg();
    }

    private static void buildConnectionAndSubscribeMsg() {
        try {
         String url = "wss://mp-stp.haier.net/wsserver/ /websocket?systemId=SV-IOTFWNCZY628-0000&timestamp=1598929399737&sign=a8fecc091d188bceb5efca840e42337c8d3e5a7c4ee6dcf5fa68f58596dd3ca0"
            String cmdInfo= "{\"cmd\":\"subscribe\", \"topics\":[\"DEV_STATUS\", \"DEV_BIGDATA\"]}";
            List<String> cmdInfos = new ArrayList<>();
            cmdInfos.add(cmdInfo);
            MsgProcessor processor = new MsgProcessor() {
                @Override
                public void processorMsg(String msg) {
                    System.out.println("receive msg: " + msg);
                }
            };
            WebsocketSubscriberClient client = new WebsocketSubscriberClient(url,cmdInfos,processor);
            
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}



public interface MsgProcessor {
    void processorMsg(String msg);
}



import com.google.common.base.Throwables;
import org.apache.tomcat.websocket.WsWebSocketContainer;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.websocket.*;
import java.net.URI;
import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.List;
import java.util.concurrent.*;
import java.util.concurrent.atomic.AtomicBoolean;
import java.util.concurrent.atomic.AtomicLong;

@ClientEndpoint
public class WebsocketSubscriberClient {
    private Logger logger = LoggerFactory.getLogger(WebsocketSubscriberClient.class);
    // 最新收消息时间,心跳发消息时做时间间隔计算
    public static AtomicLong lastReceiveTime = null;
    // 是否继续心跳标志位
    public static AtomicBoolean isHeartBeatContinue = null;
    private DateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd hh:mm:ss");
    // 创建容器对象（建立连接时要用）
    private WebSocketContainer container = ContainerProvider.getWebSocketContainer();
    // 一个连接对应一个session
    private static Session session = null;
    // 心跳线程池
    private ExecutorService executorService = null;
    // 关闭心跳线程要用到
    private Future<?> future = null;
    private static MsgProcessor msgProcessor;
    private static String url;
    private static List<String> cmdInfos;

    public WebsocketSubscriberClient() {}

    public WebsocketSubscriberClient(String uri, List<String> cmdInfos, MsgProcessor msgProcessor) throws Exception {
        WebsocketSubscriberClient.url = uri;
        WebsocketSubscriberClient.msgProcessor = msgProcessor;
        WebsocketSubscriberClient.cmdInfos = cmdInfos;
        session = container.connectToServer(WebsocketSubscriberClient.class, URI.create(uri));
        logger.info("WebsocketSubscriberClient init finished...");
        if (session.isOpen()) {
            executorService =  new ThreadPoolExecutor(1,1,6000,
                    TimeUnit.MILLISECONDS, new LinkedBlockingDeque<Runnable>());
            lastReceiveTime = new AtomicLong(System.currentTimeMillis());
            isHeartBeatContinue = new AtomicBoolean(true);
            // 值为0表示会话连接不会因为长时间无数据交互而超时
            session.setMaxIdleTimeout(0);
            //1Mb
            session.setMaxTextMessageBufferSize(1024 * 1024);
            // 启动心跳
            startHeartBeat();
        } else {
            container = null;
        }
    }

    @OnMessage
    public void onMessage(final String message) {
        try {
            long currentTime = System.currentTimeMillis();
            // 更新最新收消息时间
            lastReceiveTime.set(currentTime);
            String currentDateStr = dateFormat.format(currentTime);
            logger.debug(currentDateStr + " 收到 message：" + message);
            msgProcessor.processorMsg(message);
        } catch (Exception e) {
            logger.error("Exception = {}", Throwables.getStackTraceAsString(e));
        }
    }

    @OnClose
    public void onClose(CloseReason closeReason) {
        stopHeartBeat();
        stopContainer();
        logger.error("Connection closed!  CloseReason : code:{},reason:{}", closeReason.getCloseCode().getCode(), closeReason.getReasonPhrase());
        if (closeReason.getCloseCode().getCode() != CloseReason.CloseCodes.NORMAL_CLOSURE.getCode()) {
            boolean isSuccess = false;
            while (!isSuccess) {
                try {
                    logger.error("Try restarting the connection after 5s...");
                    Thread.sleep(5000);
                    isSuccess = buildClient();
                    if (!isSuccess) {
                        logger.error("Try restarting the connection after 2min...");
                        Thread.sleep(120000);
                        isSuccess = buildClient();
                        if (!isSuccess) {
                            logger.error("Try restarting the connection after 5min...");
                            Thread.sleep(300000);
                            isSuccess = buildClient();
                        }
                    }
                } catch (Exception e) {
                    logger.error("Exception = {}", Throwables.getStackTraceAsString(e));
                }
            }
        }
    }

    private boolean buildClient() {
        try {
            new WebsocketSubscriberClient(url, cmdInfos, msgProcessor);
            cmdInfos.forEach(cmd -> {
                addCmdInfo(cmd);
            });
            return true;
        } catch (Exception e) {
            logger.error("Exception = {}", Throwables.getStackTraceAsString(e));
            return false;
        }
    }

    public void addCmdInfo(final String cmdInfo) {
        try {
            if (session.isOpen()) {
                synchronized (session) {
                    session.getBasicRemote().sendText(cmdInfo);
                }
            }
        } catch (Exception e) {
            logger.error("Exception = {}", Throwables.getStackTraceAsString(e));
        }
    }

    public void close() {
        try {
            // 关闭心跳
            stopHeartBeat();
            // 关闭session连接
            session.close(new CloseReason(CloseReason.CloseCodes.NORMAL_CLOSURE, "NORMAL_CLOSE"));
            // 关闭容器
            stopContainer();
        } catch (Exception e) {
            logger.error("Exception = {}", Throwables.getStackTraceAsString(e));
        }
    }

    private void stopContainer() {
        if (container instanceof WsWebSocketContainer) {
            try {
                ((WsWebSocketContainer) container).destroy();
            } catch (Exception e) {
                logger.error("Exception = {}", Throwables.getStackTraceAsString(e));
            }
        }
    }

    private void startHeartBeat() {
        future = executorService.submit(new HeartBeat());
    }

    private void stopHeartBeat() {
        if (future != null) {
            try {
                isHeartBeatContinue.set(false);
                future.cancel(true);
                executorService.shutdown();
            } catch (Exception e) {
                logger.error("Exception = {}", Throwables.getStackTraceAsString(e));
            }
        }
    }

    private class HeartBeat implements Runnable {
        // 心跳间隔时间，具体根据实际情况设置,如nginx默认websocket超时时间是60s，则可以在连接无数据交互情况持续到快过期前发送一次心跳（这里设置55s时），
        private long heartBeatInterval = 55000;
        @Override
        public void run() {
            try {
                while (isHeartBeatContinue.get()) {
                    long startTime = System.currentTimeMillis();
                    if (startTime - lastReceiveTime.get() > heartBeatInterval) {
                        String cmdInfo = "{\"cmd\":\"keepAlive\"}";
                        if (session.isOpen()) {
                            synchronized (session) {
                                session.getBasicRemote().sendText(cmdInfo);
                            }
                            // 更新最新收消息时间
                            lastReceiveTime.set(System.currentTimeMillis());
                        }
                    }
                     Thread.sleep(1000);
                }
            } catch (Exception e) {
                logger.error("Exception = {}", Throwables.getStackTraceAsString(e));
            }
        }
    }
}


```

说明：
(1)	心跳的JSON字符串格式如下：  

```json
  `{"cmd": "keepAlive"}`  

```
(2)	服务端相应如下：  
```json
{
    "cmd":"keepAlive",
    "code":"000000",
    "desc":"success"
}
```


### 自动重连机制    

由于网络闪断、Websocket Server服务端重启升级等原因，势必造成已有Websocket Client接入端连接中断，所以强烈建议Websocket Client接入端代码增加自动重连机制，可参照3.9示例中绿色标注代码或在此基础上优化。

说明：
>
(1)	自动重连尝试间隔可逐步递增，如5s尝试一次连接，如果不成功则2min后再尝试一次连接，如果还未成功则5min后再尝试连接一次。</br>
(2)	如果(1)未重连成功，则可尝试在以(1)为一个周期，持续循环重连。</br>
(3)	建议重连尝试间隔不易过短或频繁,如几秒钟一循环,以防止瞬间大量访问,对服务端造成连接压力。





[^-^]:常用图片注释
[dataSubscription_type]:../Device/_media/_dataSubscription/dataSubscription_type.png
[dataS_flow]:../Device/_media/_dataSubscription/dataS_flow.png
[dataS_model]:../Device/_media/_dataSubscription/dataS_model.png







