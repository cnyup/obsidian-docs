
**目前Ingress-Nginx默认支持`TLS V1.2以及V1.3`版本，因此对于旧版本的浏览器以及客户端TLS版本低于`1.2`时，会导致客户端在与Ingress-Nginx服务SSL版本协商时报错。**

**ingress-nginx v1.6.0 及更高版本不支持 TLSv1、TLSv1.1**
### 解决办法
1. 修改configmap kube-system/nginx-configuration 在data中添加以下配置
```yaml
ssl-ciphers: "ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:DHE-RSA-CHACHA20-POLY1305:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES256-SHA384:ECDHE-RSA-AES256-SHA384:ECDHE-ECDSA-AES256-SHA:ECDHE-RSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES256-SHA256:AES128-GCM-SHA256:AES256-GCM-SHA384:AES128-SHA256:AES256-SHA256:AES128-SHA:AES256-SHA:DES-CBC3-SHA"
ssl-protocols: "TLSv1 TLSv1.1 TLSv1.2 TLSv1.3"
```
**Nginx Ingress Controller 1.7.0及以后版本，如要启用TLS v1.0与TLS v1.1，需要将`@SECLEVEL=0`添加到`ssl-ciphers`中。**

2. 重启nginx-ingress-controller
3. 验证
	 -  使用curl验证  curl --tlsv1 --tls-max 1.0  [url]
	 -  使用testssl.sh验证  testssl.sh [url]
	 -  使用已封装好的docker镜像验证  docker run --rm -it mvance/testssl -p [url]
### 参考文献及方法
- [阿里云 Nginx Ingress FAQ](https://help.aliyun.com/zh/ack/ack-managed-and-ack-dedicated/user-guide/nginx-ingress-faq)

[kubernetes Ingress-Nginx](https://kubernetes.github.io/ingress-nginx/user-guide/tls/#default-tls-version-and-ciphers)
[Mozilla SSL 配置生成器](https://ssl-config.mozilla.org/)
[testssl脚本](https://github.com/drwetter/testssl.sh)