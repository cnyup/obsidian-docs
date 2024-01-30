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
