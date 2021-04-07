
#  签名机制

1. **说明**


调用方需要对发送到uws的请求进行签名，签名后赋值到Header头中的sign属性（见公共部分说明），以便服务端进行签名验证。

2. **参数介绍**


**待签名字符串为：** url字符串 + Body字符串+appId+appKey +timestamp；

**url 字符串：**指请求的接口地址去除https://uws.haier.net 后剩余的路径部分；
```
注意：get 请求不带参数
如：GET https://uws.haier.net/ufm/v1/protected/familyService/868072664569000000/familyMembers?pageNumber=1&pageSize=10

计算签名的url是从/ufm开始，Members结束，即/ufm/v1/protected/familyService/868072664569000000/familyMembers

```

**Body字符串：**指应用发送请求的Body部分去除所有空白字符后的JSON字符串，没有body时为空字符串（不是null）。

**appId：**Header头中的属性（见公共部分说明）；

**appKey：**在海极网给应用申请的appKey，不能明文发送；

**timestamp：**Header头中的属性（见公共部分说明）；

3. **算法**

签名算法就是对待签名字符串计算32位小写SHA-256值，算法示例如下。

```java
String getSign(String appId, String appKey, String timestamp, String body,String url){：
    URL urlObj = new URL(url);
    url=urlObj.getPath();
	appKey = appKey.trim();
	appKey = appKey.replaceAll("\"", "");
	if (body != null) {
		body = body.trim();
	}
	if (!body.equals("")) {
		body = body.replaceAll(" ", "");
		body = body.replaceAll("\t", "");
		body = body.replaceAll("\r", "");
		body = body.replaceAll("\n", "");
	}
	log.info("body:"+body);
	StringBuffer sb = new StringBuffer();
	sb.append(url).append(body).append(appId).append(appKey).append(timestamp);

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

```
//计算签名示例

public static void main(String[] args)  {
		

		String ss = getSign("MB-DEMO-0000","504f37c39bb062a789b28598fe94d9d8","1614331048386","{\"deviceId\":\"2C37C530B5F1\"}","https://uws.haier.net/shadow/v1/info");
		System.out.println(ss);

			} //计算出的ss为7e5ffbf921dabc9dc3db657c4d2fdb7c990444380d638973f26762722d7b09d2


```