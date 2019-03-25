


## Internationalization 
The retCode and retInfo returned by the interface response are not internationalized and are processed by the interface caller.  
The internationalization of the interface involving business data is defined by passing the language parameter in the header. The specific international language code is shown below.  

编码|语言
:-:|:-:
de|德语
fr|法语
it|意大利语
es|西班牙语
sv|瑞典语
fi|芬兰语
da|丹麦语
no|挪威语
nl|荷兰语
ru|俄罗斯语
ed|英语
pl|波兰
cs|捷克
zh-cn|中文简体
zh-tw|中国台湾-繁体
he|希伯来语
el|希腊语
pt|葡萄牙语

## Safety Regulations

1. **Data Security**

The App application should ensure the safe use of the appid, appkey and other information obtained through the Haiji network when accessing the UWS. The risk of being disabled by the platform due to the leakage of the UWS service due to the leakage is the responsibility of the APP developer.  
The data acquired by the App through UWS, etc., must ensure storage security.

2. **Interface access security**

It must be executed in strict accordance with the usage documentation of the UWS interface service.


## Common HTTP Status Code Commentary

Status Code|Meaning|Solutions
:-:|:-|:-
200|(success)</br>The request has been successful, and the response header or data body that the request expects will be returned with this response.|
400|（Wrong request）</br>1. The semantics are incorrect and the current request cannot be understood by the server. The client should not submit this request repeatedly unless it is modified. </br>2, the request parameters are incorrect.|Check the request parameters, methods, etc. correctly against the interface documentation.
401|(authentication error)</br> The request requires authentication. The server may return this response for web pages that require login.|Check the request parameters, methods, etc. correctly against the interface documentation.
403|(prohibited)</br> The server has understood the request but refused to execute it. Unlike the 401 response, authentication does not provide any help, and this request should not be submitted repeatedly.|Check the request parameters, methods, etc. correctly against the interface documentation.
404|(Not found)</br> The request failed, and the requested resource was not found on the server.|Check the request parameters, methods, etc. correctly against the interface documentation.
500|(Intra-server error)</br> The server encountered an unexpected condition that prevented it from completing the processing of the request. In general, this problem will occur when the server's code is wrong.|Troubleshoot the corresponding server error or exception
501|(Not yet implemented)</br> The server does not have the ability to complete the request. For example, the server might return this code when the server does not recognize the request method. |Troubleshoot the corresponding server error or exception
502|(Error Gateway)</br> The server received an invalid response from the upstream server as a gateway or proxy. |Troubleshoot the corresponding server error or exception
503|(Service not available)</br> The server is currently unable to process the request due to temporary server maintenance or overload.|Troubleshoot the corresponding server error or exception
504|(Gateway timeout)</br> When a server working as a gateway or proxy attempts to execute a request, it fails to receive a response from the upstream server (the server identified by the URI, such as HTTP, FTP, LDAP) or the secondary server (for example, DNS).|Troubleshoot the corresponding server error or exception


## Public error code
|retCode|	retInfo|Try a solution|
|---|---|---|  
|10000|The login was successful, but the password security level was improved. Please change the password|User uses weak password, please change password|
|00001|The login was successful but the latest version of the privacy agreement was not accepted|Please accept the latest privacy agreement for the old account|
|A00001|	service is not available           |Contact the cloud platform to check if the service is normal|
|A00002|	Network exception                  |Contact the cloud platform to check if the service is normal|
|A00003|	Access or operation timeout        |Contact the cloud platform to check if the service is normal|
|A00004|	Internal System Error              |Contact the cloud platform to check if the service is normal|
|A00005|	Database access exception          |Contact the cloud platform to check if the service is normal|
|A00006|	Unknown exception                  |Contact the cloud platform to check if the service is normal|
|A00007|	Mail service exception             |Contact the cloud platform to check if the service is normal|
|A00008|	Mail failed to send                |Contact the cloud platform to check if the service is normal|
|A00009|	The number of emails sent exceeded |Check whether the required parameter is a valid value (non-null and cannot be an empty string)|
|B00001|	Missing required parameters        |N/A|
|B00002|	Incorrect parameter type           |N/A|
|B00003|	Parameter value is out of range or not enumerated   |N/A|
|B00004|	The parameter does not meet the rule requirements   |N/A|
|B00006|	Incorrect parameter length                          |N/A|
|B00007|	The parameter does not match the interface definition   |N/A|
|C00001|	appId and appKey validation failed                      |N/A|
|C00002|	appServer no access authorization                       |N/A|
|C00003|	Insufficient access                                |N/A|
|C00004|	Insufficient authority                             |N/A|
|C00005|	Repeat request                                     |N/A|
|C00006|	Unknown device type                                 |N/A|
|C00007|	appId configuration information is empty           |N/A|
|C00008|	appKey is empty                                    |N/A|
|D00001|	Sign signature error                               |N/A|
|D00002|	Incorrect username or password                        |N/A|
|D00003|	Token does not exist, not verified by token               |N/A|
|D00004|	Token has expired and has not been verified by token        |N/A|
|D00005|	Token is not created by this application, not verified by token  |N/A|
|D00006|	Session invalidation                                    |N/A|
|D00007|	Not an internal user                              |N/A|
|D00008|	User is not legal                                    |N/A|
|D00009|	Login Failure Number More than Limit, Need Verification Code|N/A|
|D00010|	Account Locked| N/A|
|D00011|	Account Not Activated|  N/A|
|D00012|	Account Already Excit|N/A|
|D00015|	Wrong Verification Code|N/A|
|D00016|Already Logout or Havn't Login|N/A|
|D00017|Account Not Excit|N/A|
|G20003|Unable to get adapter/Application information|N/A|
|G20201|Failed to get the user bound device list|N/A|
|G20202|User Doesn't match the Device| Check if the device id matches the user| 
|G20904|The number of bindings has been exceeded| A user can only bind to 100 devices |
|G20908|Safety check failed| Please re-distribute the network |
|G20910|The non-safety device failed to check|Non-secure device binding:</br>1. Equipment must be online</br>2. The time for reporting version information of the equipment shall be no more than 10 minutes, that is, less than or equal to 10 minutes|
|G20301|Device information query failed|N/A|
|G23401|Device unbound|N/A|
|G23501|User - bound device security verification failed|N/A|
|G24001|The equipment brand model information is empty|N/A|  
|20903|Master data access exception|N/A|
