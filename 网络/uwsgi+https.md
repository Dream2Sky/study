

#### 秘钥和证书生成

- 第一步，为服务器端和客户端准备公钥、私钥

  ```shell
  # 生成服务器端私钥
  openssl genrsa -out server.key 1024
  # 生成服务器端公钥
  openssl rsa -in server.key -pubout -out server.pem
   
   
  # 生成客户端私钥
  openssl genrsa -out client.key 1024
  # 生成客户端公钥
  openssl rsa -in client.key -pubout -out client.pem
  ```

- 第二步，生成 CA 证书

  ```shell
  # 生成 CA 私钥
  openssl genrsa -out ca.key 1024
  # X.509 Certificate Signing Request (CSR) Management.
  openssl req -new -key ca.key -out ca.csr
  # X.509 Certificate Data Management.
  openssl x509 -req -in ca.csr -signkey ca.key -out ca.crt
  ```

  在执行第二步时会出现

  ```shell
  You are about to be asked to enter information that will be incorporated
  into your certificate request.
  What you are about to enter is what is called a Distinguished Name or a DN.
  There are quite a few fields but you can leave some blank
  For some fields there will be a default value,
  If you enter '.', the field will be left blank.
  -----
  # 国家
  Country Name (2 letter code) [AU]:CN
  # 省份
  State or Province Name (full name) [Some-State]:Guangdong
  # 城市
  Locality Name (eg, city) []:Shenzhen
  # 组织或公司
  Organization Name (eg, company) [Internet Widgits Pty Ltd]:Sangfor
  # 单位
  Organizational Unit Name (eg, section) []:VS
  # 这一项，是最后可以访问的域名，如果是为了给网站生成证书，需要写成真实域名。
  # ！！！！当生成ca证书时，common name不能和client和server端的一样，否则会出现tlsv1 alert unknown ca错误 ！！！！
  # ca: 0.0.0.0 server&client: 127.0.0.1
  Common Name (e.g. server FQDN or YOUR name) []:localhost
  Email Address []:
  ```

- 第三步，生成服务器端证书和客户端证书

  ```shell
  # 服务器端需要向 CA 机构申请签名证书，在申请签名证书之前依然是创建自己的 CSR 文件
  openssl req -new -key server.key -out server.csr
  # 向自己的 CA 机构申请证书，签名过程需要 CA 的证书和私钥参与，最终颁发一个带有 CA 签名的证书
  openssl x509 -req -CA ca.crt -CAkey ca.key -CAcreateserial -in server.csr -out server.crt
   
  # client 端
  openssl req -new -key client.key -out client.csr
  # client 端到 CA 签名
  openssl x509 -req -CA ca.crt -CAkey ca.key -CAcreateserial -in client.csr -out client.crt
  ```

- 第四步，将客户端证书打包成p12格式

  ```shell
  openssl pkcs12 -export -clcerts -in client.crt -inkey client.key -out client.p12
  ```

- 第五步， 将客户端p12文件解压成pem文件，供curl和Python使用

  ```shell
  openssl pkcs12 -in client.p12 -out all.pem -nodes                #客户端公钥与私钥，一起存在all.pem中 
  ```

  

### uwsgi启动文件配置

```shell
[uwsgi]

# 配置http和https，单机访问可以使用http协议，对外提供https服务协议
http=0.0.0.0:80

# 此处配置支持https，增加!/root/ca/ca.crt参数，表示需要验证客户端证书，！表示强制验证
https = 0.0.0.0:443,/root/server/server.crt,/root/server/server.key,HIGH,!/root/ca/ca.crt

chdir = /root/www

wsgi-file = www/wsgi.py

master = true

pidfile = /var/run/uwsgi.pid
# 通过pid停止重启服务

daemonnize = true
# 守护进程，后台运行
```

