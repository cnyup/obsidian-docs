## 场景
web后端机器需要升降配置，需要nginx这边先将需要操作的后端服务器移除upstream组，防止流量到达导致服务不可用。

## 操作流程
修改nginx配置文件，将待操作的服务器剔除 -> 测试请求验证(确保没有流量转发到待操作服务器) -> 操作web服务器 -> web服务器验证服务可用性 -> 加入nginx -> 验证整体可用性


## 问题
1.在upstream中设置了 weight=0  reload后 nginx报错
`nginx: [emerg] invalid parameter "weight=0" in xxxxxx`

## 解决
1.通过使用down 来标记服务不可用
``` nginx
upstream backend {
    server backend1.example.com;
    server backend2.example.com;
    server backend3.example.com down;
    server backend4.example.com;
}
```
查阅资料发现有些会使用到backup 但是backup存在一个隐患: 并不是严格意义的下线操作，而是一个主备切换的功能。
