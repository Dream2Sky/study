### Docker常用命令

> https://www.bilibili.com/video/BV1og4y1q7M4?p=9
> https://www.bilibili.com/video/BV1og4y1q7M4?p=10
> https://www.bilibili.com/video/BV1og4y1q7M4?p=11
> https://www.bilibili.com/video/BV1og4y1q7M4?p=12

#### Docker镜像命令

**docker images** 查看本地镜像


```shell
[root@VM-0-15-centos ~]# docker images
REPOSITORY    TAG       IMAGE ID       CREATED         SIZE
hello-world   latest    bf756fb1ae65   13 months ago   13.3kB

# 解释
REPOSITORY  镜像的仓库源
TAG			镜像的标签
IMAGE ID	镜像的id
CREATED		镜像的创建日期
SIZE		镜像的大小

Options:
  -a, --all             Show all images (default hides intermediate images)
  -q, --quiet           Only show image IDs
```

**docker search** 搜索镜像

```shell
[root@VM-0-15-centos ~]# docker search mysql
NAME                              DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
mysql                             MySQL is a widely used, open-source relation…   10520     [OK]       
mariadb                           MariaDB is a community-developed fork of MyS…   3928      [OK]       
mysql/mysql-server                Optimized MySQL Server Docker images. Create…   772                  [OK]
percona                           Percona Server is a fork of the MySQL relati…   527       [OK]       
centos/mysql-57-centos7           MySQL 5.7 SQL database server                   86                   
mysql/mysql-cluster               Experimental MySQL Cluster Docker images. Cr…   79                   
centurylink/mysql                 Image containing mysql. Optimized to be link…   59                   [OK]

Options:
  -f, --filter filter   Filter output based on conditions provided
      --format string   Pretty-print search using a Go template
      --limit int       Max number of search results (default 25)
      --no-trunc        Don't truncate output
      
pg:
[root@VM-0-15-centos ~]# docker search mysql --filter=STARS=3000 获取stars数量大于3000的镜像
```

**docker pull** 拉取镜像

```shell
# 下载镜像  docker pull 镜像名[:tag]

[root@VM-0-15-centos ~]# docker pull mysql
Using default tag: latest   # 不写tag的话，默认用lastst这个tag的镜像
latest: Pulling from library/mysql
45b42c59be33: Pull complete   # 分层下载， docker image的核心--联合文件系统
b4f790bd91da: Pull complete 
325ae51788e9: Pull complete 
adcb9439d751: Pull complete 
174c7fe16c78: Pull complete 
698058ef136c: Pull complete 
4690143a669e: Pull complete 
f7599a246fd6: Pull complete 
35a55bf0c196: Pull complete 
790ac54f4c47: Pull complete 
18602acc97e1: Pull complete 
365caa3500d0: Pull complete 
Digest: sha256:b1cc887ed32cc6c2f217b12703bd05f503f2037892c8bb226047fe5dff85a109
Status: Downloaded newer image for mysql:latest
docker.io/library/mysql:latest # 镜像的真实地址

# 以下两个命令等价
[root@VM-0-15-centos ~]# docker pull mysql
[root@VM-0-15-centos ~]# docker pull docker.io/library/mysql:latest

# 指定版本下载
[root@VM-0-15-centos ~]# docker pull mysql:5.7
```

**docker rmi** 删除镜像

```shell
# 删除指定镜像
[root@VM-0-15-centos ~]# docker rmi -f 5f47254ca581
Untagged: mysql:5.7
Untagged: mysql@sha256:853105ad984a9fe87dd109be6756e1fbdba8b003b303d88ac0dda6b455f36556
Deleted: sha256:5f47254ca5817f99cdd387ce7345d43e770e0682a4c81b62776f3347551b1d85
Deleted: sha256:63f5d2725ff0ecffe0a7345e749d39b269a8cef04984661f0f4e752869b9fbb1
Deleted: sha256:acbe85abff4e7bbdd75a1f56ee9a095a72fcba4c226d0194d46b9a8471b1fe18
Deleted: sha256:b851a484b18c5d3d25497260c111631ae3adf924eb10baa533b2a5b03b339d1a
Deleted: sha256:b5133b076285236e7fd98c42c1f18f57e2b4ed98daaed7b0afb3b98b804d6f25

# 删除多个镜像
[root@VM-0-15-centos ~]# docker rmi -f 5f47254ca581 5f47254ca581 5f47254ca581

# 批量删除镜像
docker rmi -f $(docker images -aq)
```



#### Docker容器命令

**docker run** 运行镜像

```shell
# docker run [可选参数] 镜像名

[root@VM-0-15-centos ~]# docker run -p 8080 -it centos /bin/bash

# 225fdbca546d 运行之后进入容器，225fdbca546d是容器id
[root@225fdbca546d /]# ls
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var

# Options：
# -it 交互的方式运行，进入容器查看内容 -it 镜像名 控制台路径（一般都是/bin/bash）
# -p  指定容器端口 -p 8080:8080
# 	-p ip:主机端口:容器端口
#   -p 主机端口:容器端口
#   -p 容器端口
#   容器端口
# -P  随机端口
# -d  后台方式运行容器

```

**docker ps** 查看容器

```shell
# docker ps [options] 查看运行中的容器
# docker ps 查看当前运行中的容器
# docker ps -a 查看当前运行中的容器，并带出历史运行过的容器
# docker ps -a -n=2 指定最近创建的容器
[root@VM-0-15-centos ~]# docker ps -a
CONTAINER ID   IMAGE         COMMAND                  CREATED          STATUS                      PORTS     NAMES
225fdbca546d   centos        "/bin/bash"              30 minutes ago   Exited (0) 29 minutes ago             hardcore_gauss
9645c809bae6   centos        "-p 8080 -it /bin/ba…"   32 minutes ago   Created                               happy_sammet
9b512f234170   centos        "/bin/bash"              32 minutes ago   Exited (0) 32 minutes ago             peaceful_burnell
f58dc9b7431c   hello-world   "/hello"                 2 hours ago      Exited (0) 2 hours ago                youthful_rhodes
519fff117e3b   hello-world   "/hello"                 8 hours ago      Exited (0) 8 hours ago                admiring_ptolemy
9202336dd4e9   hello-world   "/hello"                 11 days ago      Exited (0) 11 days ago                condescending_rosalind
```

**退出容器**

```shell
# 从容器中退出， 并停止容器
[root@225fdbca546d /]# exit
exit
You have new mail in /var/spool/mail/root
# 从容器中退出，但不停止容器
# Ctrl + P + Q
```

**docker rm **删除容器

```shell
[root@VM-0-15-centos ~]# docker ps
CONTAINER ID   IMAGE     COMMAND       CREATED         STATUS         PORTS     NAMES
08fd1d4a7c94   centos    "/bin/bash"   4 minutes ago   Up 4 minutes             goofy_knuth
[root@VM-0-15-centos ~]# docker rm 08fd1d4a7c94
Error response from daemon: You cannot remove a running container 08fd1d4a7c94713381496434abcf85f5cb7c857f087a2cfa9d36145cc6242601. Stop the container before attempting removal or force remove

# docker rm 容器id
# docker rm 删除已经停止的容器，不能删除运行中的容器
# docker rm -f 强制删除，可以删除运行中的容器
# docker rm -f $(docker ps -aq) 批量删除所有容器
# docker ps -a -q|xargs docker rm 利用linux的管道，批量删除容器
```

**启动和停止容器**

```shell
docker start 容器id		# 启动容器
docker restart 容器id		# 重启容器
docker stop 容器id		# 停止容器
docker kill 容器id		# 强制停止容器
```



#### Docker其他常用命令

**后台启动容器**

```shell
# docker run -d 镜像名
[root@VM-0-15-centos ~]# docker run -d centos
0785901fbfb6b6614f588351dc8e59ef0af1b9682cb603334a1e80253485eabd
You have new mail in /var/spool/mail/root

# 如果当前镜像没有前台进程的话，后台启动后会直接退出容器！！！
# 也就是说docker容器在启动之后，发现自己没有对外提供服务，就会自行退出

```

**查看日志**

```shell
# docker logs -f -t --tail number 容器id

Options:
      --details        Show extra details provided to logs
  -f, --follow         Follow log output
      --since string   Show logs since timestamp (e.g. 2013-01-02T13:23:37Z) or relative (e.g. 42m for 42 minutes)
  -n, --tail string    Number of lines to show from the end of the logs (default "all")
  -t, --timestamps     Show timestamps
      --until string   Show logs before a timestamp (e.g. 2013-01-02T13:23:37Z) or relative (e.g. 42m for 42 minutes)
      
 [root@VM-0-15-centos ~]# docker logs -f -t --tail 10 08fd1d4a7c94
 
 # 手动写脚本的例子
[root@VM-0-15-centos ~]# docker run -d centos /bin/sh -c "while true; do echo hehe123; sleep 1; done"
81b5bf182a70472bd410cbc74bf7cf22c0a8ca8cc6c8fb443453050eef42b52a

[root@VM-0-15-centos ~]# docker logs -f -t --tail 10 81b5bf182a70
2021-02-19T09:00:11.105509115Z hehe123
2021-02-19T09:00:12.108255849Z hehe123
2021-02-19T09:00:13.110952592Z hehe123
2021-02-19T09:00:14.112933416Z hehe123
2021-02-19T09:00:15.114934234Z hehe123
2021-02-19T09:00:16.116945635Z hehe123
2021-02-19T09:00:17.119360103Z hehe123
2021-02-19T09:00:18.121383795Z hehe123
2021-02-19T09:00:19.123910535Z hehe123
2021-02-19T09:00:20.126393026Z hehe123
2021-02-19T09:00:21.128361525Z hehe123

# docker run -c 执行命令
```

**查看容器中的进程**

```shell
# docker top 容器id
[root@VM-0-15-centos ~]# docker top 08fd1d4a7c94
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
root                24395               24373               0                   15:04               pts/0               00:00:00            /bin/bash
```

**查看容器中的元数据**

```shell
# docker inspect 容器id
[root@VM-0-15-centos ~]# docker inspect 08fd1d4a7c94
[
    {
        "Id": "08fd1d4a7c94713381496434abcf85f5cb7c857f087a2cfa9d36145cc6242601",
        "Created": "2021-02-19T07:04:35.390237538Z",
        "Path": "/bin/bash",
        "Args": [],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 24395,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2021-02-19T07:04:35.735722739Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:300e315adb2f96afe5f0b2780b87f28ae95231fe3bdd1e16b9ba606307728f55",
        "ResolvConfPath": "/var/lib/docker/containers/08fd1d4a7c94713381496434abcf85f5cb7c857f087a2cfa9d36145cc6242601/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/08fd1d4a7c94713381496434abcf85f5cb7c857f087a2cfa9d36145cc6242601/hostname",
        "HostsPath": "/var/lib/docker/containers/08fd1d4a7c94713381496434abcf85f5cb7c857f087a2cfa9d36145cc6242601/hosts",
        "LogPath": "/var/lib/docker/containers/08fd1d4a7c94713381496434abcf85f5cb7c857f087a2cfa9d36145cc6242601/08fd1d4a7c94713381496434abcf85f5cb7c857f087a2cfa9d36145cc6242601-json.log",
        "Name": "/goofy_knuth",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "default",
            "PortBindings": {},
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "CapAdd": null,
            "CapDrop": null,
            "CgroupnsMode": "host",
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "ConsoleSize": [
                0,
                0
            ],
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": null,
            "BlkioDeviceWriteBps": null,
            "BlkioDeviceReadIOps": null,
            "BlkioDeviceWriteIOps": null,
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "KernelMemory": 0,
            "KernelMemoryTCP": 0,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": false,
            "PidsLimit": null,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/asound",
                "/proc/acpi",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/a19d2dd9fe1492d03c0d06e7c3d84ab3dd6a5c6cc40b33023911d234a6a8de17-init/diff:/var/lib/docker/overlay2/d1a8845ceefc70a0a68a13b8e4a888a330edcb1d908ff82b396b42e74733e276/diff",
                "MergedDir": "/var/lib/docker/overlay2/a19d2dd9fe1492d03c0d06e7c3d84ab3dd6a5c6cc40b33023911d234a6a8de17/merged",
                "UpperDir": "/var/lib/docker/overlay2/a19d2dd9fe1492d03c0d06e7c3d84ab3dd6a5c6cc40b33023911d234a6a8de17/diff",
                "WorkDir": "/var/lib/docker/overlay2/a19d2dd9fe1492d03c0d06e7c3d84ab3dd6a5c6cc40b33023911d234a6a8de17/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [],
        "Config": {
            "Hostname": "08fd1d4a7c94",
            "Domainname": "",
            "User": "",
            "AttachStdin": true,
            "AttachStdout": true,
            "AttachStderr": true,
            "Tty": true,
            "OpenStdin": true,
            "StdinOnce": true,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/bash"
            ],
            "Image": "centos",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {
                "org.label-schema.build-date": "20201204",
                "org.label-schema.license": "GPLv2",
                "org.label-schema.name": "CentOS Base Image",
                "org.label-schema.schema-version": "1.0",
                "org.label-schema.vendor": "CentOS"
            }
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "6a270bff31bba0c356361bcf7d72ffa99a8ce407e2ea71c7032767eb26c5d297",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {},
            "SandboxKey": "/var/run/docker/netns/6a270bff31bb",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "3acc89d41a2ac09222b2b0eee149cebebe1a602eab7b1fe0bbb2cb742cbbe94d",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.2",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:02",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "29f6cbc0df13305a11fbbeb68194a2c788fe850362363cc901c2c0aa3253f283",
                    "EndpointID": "3acc89d41a2ac09222b2b0eee149cebebe1a602eab7b1fe0bbb2cb742cbbe94d",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null
                }
            }
        }
    }
]
```

![image](C:\apps\笔记\docker\1613725997(1).png)

**进入已经运行着的容器**

```shell
# docker exec -it 容器id bashShell
[root@VM-0-15-centos ~]# docker exec -it 08fd1d4a7c94 /bin/bash
[root@08fd1d4a7c94 /]# 

# docker attach 容器id
[root@VM-0-15-centos ~]# docker attach 08fd1d4a7c94
[root@08fd1d4a7c94 /]# 

docker exec   # 进入容器，打开新的终端（常用）
docker attach # 进入容器，打开正在运行的终端
```

**手动从容器内拷贝文件到主机**

```shell
# docker cp 容器id:容器内文件路径 主机路径
[root@VM-0-15-centos ~]# docker cp 08fd1d4a7c94:/home/text.java /home/

# 拷贝时，容器不必运行中！！！
```









