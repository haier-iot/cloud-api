#  设备SDK相关问题

**1. linux版设备SDK初始化错误**

?> 答：sdk初始化报错，调用运行 ugw_dev_init("gw.haier.net",56810,"/root","wlan0")  提示setsockopt -SO_RECV_ANYIF:Protocol not available
排查过程,核实项目的硬件底层是否支持mDNS服务，SDK是必须依赖这个服务：  
①核实初始化接口、参数填写正确  
②核实5353端口有无占用（mDNS服务指定端口）


