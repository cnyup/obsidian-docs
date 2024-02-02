## 1 Nginx中时间变量
### 1.1 request_time
[官方文档](https://nginx.org/en/docs/http/ngx_http_log_module.html)
**官方含义解释**: request processing time in seconds with a milliseconds resolution; time elapsed between the first bytes were read from the client and the log write after the last bytes were sent to the client
可以发现该时间指的是`从接受用户请求的第一个字节到发送完响应数据后写入日志的时间`，因此$request_time包括接受客户端请求数据的时间、后端服务响应的时间、发送响应数据给客户端的时间、以及`写入日志`的时间
### 1.2 upstream_response_time
[官方文档](http://nginx.org/en/docs/http/ngx_http_upstream_module.html)
**官方含义解释**: keeps time spent on receiving the response from the upstream server; the time is kept in seconds with millisecond resolution. Times of several responses are separated by commas and colons like addresses in the [$upstream_addr](http://nginx.org/en/docs/http/ngx_http_upstream_module.html#var_upstream_addr) variable.
这个时间记录的是`从后端建立连接到接受完后端响应数据关闭nginx连接`所花费的时间
### 1.3 upstream_connect_time(1.9.1后)
[官方文档](http://nginx.org/en/docs/http/ngx_http_upstream_module.html#var_upstream_addr)
**官方含义解释**: keeps time spent on establishing a connection with the upstream server (1.9.1); the time is kept in seconds with millisecond resolution. In case of SSL, includes time spent on handshake. Times of several connections are separated by commas and colons like addresses in the [$upstream_addr](http://nginx.org/en/docs/http/ngx_http_upstream_module.html#var_upstream_addr) variable.
该时间记录的是`与后端建立连接`所花费的时间(如果是加密传输ssl，则包括握手的时间)
### 1.4 upstream_header_time(1.7.10后)
[官方文档](http://nginx.org/en/docs/http/ngx_http_upstream_module.html#var_upstream_addr)

**官方含义解释**: keeps time spent on receiving the response header from the upstream server (1.7.10); the time is kept in seconds with millisecond resolution. Times of several responses are separated by commas and colons like addresses in the [$upstream_addr](http://nginx.org/en/docs/http/ngx_http_upstream_module.html#var_upstream_addr) variable.
该时间记录的是`接收到后端响应头`所花费的时间

## 2.  时间分析
### 2.1 流程
整个链路流程:    
	1. 用户请求 [1]  
	2. 建立Nginx连接 [2]   
	3. 发送响应到后端  [3]  
	4. 接受后端响应 [4]  
	5. 关闭Nginx连接 [5]  

那么：
		request_time = 1 + 2 + 3 + 4 + 5 
		upstream_response_time = 2 + 3 + 4 +5
其中第五步关闭Nginx连接时间一般可以忽略不记

整个流程以及相关时间变量关系可以借用下图来进行说明(图源自网络)

[nginx请求与时间变量关系](https://github.com/cnyup/obsidian-docs/blob/main/Nginx/img/nginx%E8%AF%B7%E6%B1%82%E4%B8%8E%E6%97%B6%E9%97%B4%E5%8F%98%E9%87%8F%E5%85%B3%E7%B3%BB.png)
