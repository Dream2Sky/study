> https://www.bilibili.com/video/BV1og4y1q7M4?p=14&spm_id_from=pageDriver

### 从dockerhub里拉取最新版nginx镜像

```shell
[root@VM-0-15-centos home]# docker pull nginx
Using default tag: latest
latest: Pulling from library/nginx
45b42c59be33: Already exists 
8acc495f1d91: Pull complete 
ec3bd7de90d7: Pull complete 
19e2441aeeab: Pull complete 
f5a38c5f8d4e: Pull complete 
83500d851118: Pull complete 
Digest: sha256:f3693fe50d5b1df1ecd315d54813a77afd56b0245a404055a946574deb6b34fc
Status: Downloaded newer image for nginx:latest
docker.io/library/nginx:latest
You have new mail in /var/spool/mail/root
```

### 运行nginx容器并映射端口

```shell
[root@VM-0-15-centos home]# docker run -d --name nginx -p 8080:80 nginx
7cd366876e3b429c06c9d9bcfaed06633340de817d9a5f428b46e9851bfad3a2
[root@VM-0-15-centos home]# docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                  NAMES
7cd366876e3b   nginx     "/docker-entrypoint.…"   32 seconds ago   Up 31 seconds   0.0.0.0:8080->80/tcp   nginx

# 命令解释
-p 主机端口:容器端口  # 意思是将容器中nginx使用的端口映射到主机的端口上, 或者说是端口暴露
```

**docker端口映射过程**

![image](C:\apps\笔记\docker\docker部署nginx.png)

### 访问nginx首页

```shell
[root@VM-0-15-centos home]# curl http://xxx.xxx.xxx.xxx:8080
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

