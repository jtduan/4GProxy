# 4G代理搭建方案与使用

### 服务器地址
219.150.218.96:6666

### 认证方式: HTTP Baic
并发限制：单账号50M带宽(获取测试账号密码QQ: 897949268)

### 使用方式(curl示例)：
curl -x 219.150.218.96:6666 --proxy-user ***:*** 'http://ip.taobao.com/outGetIpInfo' --data 'ip=myip&accessKey=alibaba-inc' --compressed

### 调试
在http response中带有xxx-ip(后端请求ip地址) 和 xxx-serial(后端请求机器) 用于业务方判断请求是否符合预期
```shell script
curl -v -x 219.150.218.96:6666 --proxy-user common:123 'http://ip.taobao.com/outGetIpInfo' --data 'ip=myip&accessKey=alibaba-inc' --compressed

*   Trying 219.150.218.96...
* TCP_NODELAY set
* Connected to 219.150.218.96 (219.150.218.96) port 6666 (#0)
* Proxy auth using Basic with user 'common'
> POST http://ip.taobao.com/outGetIpInfo HTTP/1.1
> Host: ip.taobao.com
> Proxy-Authorization: Basic Y29tbW9uOjEyMw==
> User-Agent: curl/7.64.1
> Accept: */*
> Accept-Encoding: deflate, gzip
> Proxy-Connection: Keep-Alive
> Content-Length: 29
> Content-Type: application/x-www-form-urlencoded
>
* upload completely sent off: 29 out of 29 bytes
< HTTP/1.1 200
< Date: Sun, 06 Dec 2020 15:22:35 GMT
< Content-Type: application/json;charset=UTF-8
< Transfer-Encoding: chunked
< Vary: Accept-Encoding
< Set-Cookie: XSRF-TOKEN=c84c612f-4d69-4f11-8954-2e9c90e07b8d; Path=/
< X-Content-Type-Options: nosniff
< X-XSS-Protection: 1; mode=block
< Cache-Control: no-cache, no-store, max-age=0, must-revalidate
< Pragma: no-cache
< Expires: 0
< X-Frame-Options: DENY
< Content-Encoding: gzip
< Connection: close
< xxx-serial: 320000
< xxx-ip: 117.61.16.150
<
* Closing connection 0
{"data":{"area":"","country":"中国","isp_id":"100017","queryIp":"117.61.16.150","city":"南京","ip":"117.61.16.150","isp":"电信","county":"","region_id":"320000","area_id":"","county_id":null,"region":"江苏","country_id":"CN","city_id":"320100"},"msg":"query success","code":0}
```

https中， 调试信息处于CONNECT请求的相应之后
```shell script
curl -v -x 219.150.218.96:6666 --proxy-user common:123 'https://www.baidu.com'

*   Trying 219.150.218.96...
* TCP_NODELAY set
* Connected to 219.150.218.96 (219.150.218.96) port 6666 (#0)
* allocate connect buffer!
* Establish HTTP proxy tunnel to www.baidu.com:443
* Proxy auth using Basic with user 'common'
> CONNECT www.baidu.com:443 HTTP/1.1
> Host: www.baidu.com:443
> Proxy-Authorization: Basic Y29tbW9uOjEyMw==
> User-Agent: curl/7.64.1
> Proxy-Connection: Keep-Alive
>
< HTTP/1.1 200 HTTP/1.1 200 Connection established
< Connection: keep-alive
< Proxy-Connection: keep-alive
< xxx-serial: 320000
< xxx-ip: 117.61.16.180
<
* Proxy replied 200 to CONNECT request
* CONNECT phase completed!
* ALPN, offering h2
* ALPN, offering http/1.1
* successfully set certificate verify locations:
*   CAfile: /etc/ssl/cert.pem
  CApath: none
* TLSv1.2 (OUT), TLS handshake, Client hello (1):
* CONNECT phase completed!
* CONNECT phase completed!
* TLSv1.2 (IN), TLS handshake, Server hello (2):
* TLSv1.2 (IN), TLS handshake, Certificate (11):
* TLSv1.2 (IN), TLS handshake, Server key exchange (12):
* TLSv1.2 (IN), TLS handshake, Server finished (14):
* TLSv1.2 (OUT), TLS handshake, Client key exchange (16):
* TLSv1.2 (OUT), TLS change cipher, Change cipher spec (1):
* TLSv1.2 (OUT), TLS handshake, Finished (20):
* TLSv1.2 (IN), TLS change cipher, Change cipher spec (1):
* TLSv1.2 (IN), TLS handshake, Finished (20):
* SSL connection using TLSv1.2 / ECDHE-RSA-AES128-GCM-SHA256
* ALPN, server accepted to use http/1.1
* Server certificate:
*  subject: C=CN; ST=beijing; L=beijing; OU=service operation department; O=Beijing Baidu Netcom Science Technology Co., Ltd; CN=baidu.com
*  start date: Apr  2 07:04:58 2020 GMT
*  expire date: Jul 26 05:31:02 2021 GMT
*  subjectAltName: host "www.baidu.com" matched cert's "*.baidu.com"
*  issuer: C=BE; O=GlobalSign nv-sa; CN=GlobalSign Organization Validation CA - SHA256 - G2
*  SSL certificate verify ok.
> GET / HTTP/1.1
> Host: www.baidu.com
> User-Agent: curl/7.64.1
> Accept: */*
>
< HTTP/1.1 200 OK
...
```

### Todo: 
支持自定义城市和后端机器

扩容，扩容，扩容

支持定制化需求