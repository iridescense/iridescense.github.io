# Docker_Study

## 底层原理

### Docker是怎么工作的？

Docker是一个Client-Server结构的系统，Docker的守护进程运行在主机上。通过Socket从客户端访问。

## Docker的常用命令

#### 帮助命令

```shell
docker version    #显示docker的版本信息
docker info       #显示docker的系统信息，包括镜像和容器的数量
docker --help		  #帮助命令
```

### 镜像命令

`docker images``

```shell
#解释
List images

Options:
  -a, --all             Show all images (default hides 
  -q, --quiet           Only show image IDs
```

`docker search`

```shell
C:\Users\86153>docker search mysql
NAME                              DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
mysql                             MySQL is a widely used, open-source relation…   11412     [OK]
mariadb                           MariaDB Server is a high performing open sou…   4336      [OK]
mysql/mysql-server                Optimized MySQL Server Docker images. Create…   847                  [OK]

--filter=STARS=3000  # 搜出来的镜像就是STARS大于3000的
```

`docker pull 下载镜像`

```shell
C:\Users\86153>docker pull mysql
Using default tag: latest
#分层下载
C:\Users\86153>docker pull mysql:5.7
5.7: Pulling from library/mysql
a330b6cecb98: Already exists
9c8f656c32b8: Already exists
88e473c3f553: Already exists
062463ea5d2f: Already exists
daf7e3bdf4b6: Already exists
1839c0b7aac9: Already exists
cf0a0cfee6d0: Already exists
fae7a809788c: Pull complete
dae5a82a61f0: Pull complete
7063da9569eb: Pull complete
51a9a9b4ef36: Pull complete
Digest: sha256:d9b934cdf6826629f8d02ea01f28b2c4ddb1ae27c32664b14867324b3e5e1291
Status: Downloaded newer image for mysql:5.7
docker.io/library/mysql:5.7 #真实地址
```

`docker rmi 删除镜像`

```shell
Usage:  docker rmi [OPTIONS] IMAGE [IMAGE...]

Remove one or more images

Options:
  -f, --force      Force removal of the image
      --no-prune   Do not delete untagged parents
```



### 容器命令

说明：我们有了镜像才 可以创建容器，l

```shell
docker pull centos
```

``新建容器使用``

```shell
docker run[可选参数] image

#参数说明
--name s   # 容器名字，用来区分容器
-d               #后台方式运行
-it             #使用交互方式运行，进入容器查看内容
-p                #指定容器的端口 -p  8080：8080
	-p #ip主机端口：容器端口
	-p #主机端口：容器端口
	-p #容器端口
-p                #随机指定端口



#测试  启动并进入容器
C:\Users\86153>docker run -it centos /bin/bash
[root@6ed8a745aad2 /]# ls # 查看容器内的centos，基础版本，很多命令都步完善
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var

```

#### 列出所有运行的容器

```shell
docker ps
    # 列出当前正在运行的容器
-a  # 列出当前正在运行的容器+带出历史运行的容器
-a -n=?#列出？个
-q #只显示容器编号
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

C:\Users\86153>docker ps -a
CONTAINER ID   IMAGE               COMMAND                  CREATED          STATUS                      PORTS     NAMES
6ed8a745aad2   centos              "/bin/bash"              18 minutes ago   Exited (0) 13 seconds ago             keen_bhaskara
029c696cc5e2   hello-world         "/hello"                 5 hours ago      Exited (0) 5 hours ago                magical_goodall
e8b5bb633a67   hello-world         "/hello"                 19 hours ago     Exited (0) 19 hours ago               clever_diffie
8c122e1e6205   docker101tutorial   "/docker-entrypoint.…"   19 hours ago     Created                               docker-tutorial
2ba4798c62cd   alpine/git          "git clone https://g…"   19 hours ago     Exited (0) 19 hours ago               repo
```

`退出容器`

```shell
exit#退出容器
Ctrl + P + Q  #容器步停止退出
```

`删除容器`

```shell
docker rm 容器id         #删除指定的容器，不能删除正在运行的热熔器，如果要强制删除 rm -f
docker rm -f $(docker ps -qa)  #删除所有的容器
docker ps -a -q|xargs docker rm #删除所有的容器
```

`启动和停止容器的操作`

```shell
docker start id	   #启动容器
docker restart id  #重启容器
docker stop id     #停止当前正在运行的容器
docker kill id     #强制停止
```

### 其他命令

`后台启动容器`

```shell
docker run -d centos
14540d540ad8dca07dc771be60fafa92cf28ba589f28ad6a794b4b4d73092a72
docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

#命令 docker run -d 镜像名 发现centos停止了
#常见的坑，docker 容器使用后台运行，就必须有一个前台进程，docker发现没有应用，就会自动停止
#nginx 容器启动后，发现自己没有提供服务，就会停止
```

`查看日志`

```shell
docker logs 
-tf #显示日志
--tail number  #显示日志条数  
```

`查看容器中的进程信息`

```shell
docker top 379d7299540a
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
root                1590                1571                0                   02:41               ?                   00:00:00            /bin/bash

```

`容器元数据`

```shell
docker inspect 379d7299540a
[
    {
        "Id": "379d7299540a0407f2fecf03288e79ec78decdc5d9dc32b78f452fcc4a41fc01",
        "Created": "2021-09-16T02:41:09.344301618Z",
        "Path": "/bin/bash",
        "Args": [],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 1590,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2021-09-16T02:41:10.091263769Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:300e315adb2f96afe5f0b2780b87f28ae95231fe3bdd1e16b9ba606307728f55",
        "ResolvConfPath": "/var/lib/docker/containers/379d7299540a0407f2fecf03288e79ec78decdc5d9dc32b78f452fcc4a41fc01/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/379d7299540a0407f2fecf03288e79ec78decdc5d9dc32b78f452fcc4a41fc01/hostname",
        "HostsPath": "/var/lib/docker/containers/379d7299540a0407f2fecf03288e79ec78decdc5d9dc32b78f452fcc4a41fc01/hosts",
        "LogPath": "/var/lib/docker/containers/379d7299540a0407f2fecf03288e79ec78decdc5d9dc32b78f452fcc4a41fc01/379d7299540a0407f2fecf03288e79ec78decdc5d9dc32b78f452fcc4a41fc01-json.log",
        "Name": "/strange_yalow",
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
                30,
                120
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
                "LowerDir": "/var/lib/docker/overlay2/00677e7f89f4af11112ad090a551eca2d2a3550131671ede5268d7a5a564c2ca-init/diff:/var/lib/docker/overlay2/f207ecd01c8411c0bd6a163a1930baa163ddad180af24be7fce5de80bb38494c/diff",
                "MergedDir": "/var/lib/docker/overlay2/00677e7f89f4af11112ad090a551eca2d2a3550131671ede5268d7a5a564c2ca/merged",
                "UpperDir": "/var/lib/docker/overlay2/00677e7f89f4af11112ad090a551eca2d2a3550131671ede5268d7a5a564c2ca/diff",
                "WorkDir": "/var/lib/docker/overlay2/00677e7f89f4af11112ad090a551eca2d2a3550131671ede5268d7a5a564c2ca/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [],
        "Config": {
            "Hostname": "379d7299540a",
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
            "SandboxID": "074fe7af67d60a1329d9b88db614ba7f06b25e5d26c6cecb26cea48361234095",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {},
            "SandboxKey": "/var/run/docker/netns/074fe7af67d6",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "c7d41687f7ac06907ed4c1bdda3383f75f74c91626215cd93bcf31690a469e04",
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
                    "NetworkID": "83282af879d503af2f624b1446fd6903b63a3dfdcff7790dc79453afbec605ca",
                    "EndpointID": "c7d41687f7ac06907ed4c1bdda3383f75f74c91626215cd93bcf31690a469e04",
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

`进入当前正在运行的容器`

```shell
#我们的容器通常都是使用后台方式运行的，需要进入容器，修改一些配置	
#命令
docker exec -it 容器id bashshell
 docker exec -it 379d7299540a /bin/bash
[root@379d7299540a /]# ls
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
[root@379d7299540a /]# ps -ef
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 02:41 pts/0    00:00:00 /bin/bash
root        15     0  1 02:52 pts/1    00:00:00 /bin/bash
root        30    15  0 02:52 pts/1    00:00:00 ps -ef

#方式2
docker attach 容器id
#当前正在执行的代码


#docker exec     #进入容器后 开一个新的终端，可以在里面操作
#docker attach   #进入容器正在执行的终端，不会启动新的进程
```

`从容器内拷贝文件到主机上`

```shell
docker cp 容器id:/home/test.java 目标地址（绝对）
```

### 命令小结

![image-20210916112646217](.\images\image-20210916112646217.png)

tomcat

```shell
#官方的使用
docker run -it --rm tomcat

#我们之前启动的都是后台，停止了容器之后，容器还是可以查到 这个一般用来测试，用完了就删

docker run -d -p 3344:8080 --name tomcat01 tomcat

#访问没有问题
docker exec -it tomcat01 /bin/bash
root@bea528a7ab13:/usr/local/tomcat# ls
BUILDING.txt     LICENSE  README.md      RUNNING.txt  conf  logs            temp     webapps.dist
CONTRIBUTING.md  NOTICE   RELEASE-NOTES  bin          lib   native-jni-lib  webapps  work

#发现问题  linux命令少了  没有webapps 默认是最小。没必要的都剔除了
#保证了最小的功能

ls
BUILDING.txt     LICENSE  README.md      RUNNING.txt  conf  logs            temp     webapps.dist
CONTRIBUTING.md  NOTICE   RELEASE-NOTES  bin          lib   native-jni-lib  webapps  work
root@bea528a7ab13:/usr/local/tomcat# cd webapps
root@bea528a7ab13:/usr/local/tomcat/webapps# ls
root@bea528a7ab13:/usr/local/tomcat/webapps# cd ..
root@bea528a7ab13:/usr/local/tomcat# cd webapps.dist
root@bea528a7ab13:/usr/local/tomcat/webapps.dist# ls
ROOT  docs  examples  host-manager  manager
root@bea528a7ab13:/usr/local/tomcat/webapps.dist# cd ..
root@bea528a7ab13:/usr/local/tomcat# cp webapps.dist/* webapps
cp: -r not specified; omitting directory 'webapps.dist/ROOT'
cp: -r not specified; omitting directory 'webapps.dist/docs'
cp: -r not specified; omitting directory 'webapps.dist/examples'
cp: -r not specified; omitting directory 'webapps.dist/host-manager'
cp: -r not specified; omitting directory 'webapps.dist/manager'
root@bea528a7ab13:/usr/local/tomcat# cp -r webapps.dist/* webapps
root@bea528a7ab13:/usr/local/tomcat# cd webapps
root@bea528a7ab13:/usr/local/tomcat/webapps# ls
ROOT  docs  examples  host-manager  manager
```

## Docker镜像

### commit镜像

```shell
docker commit 提交容器，成为一个新的版本

#m、
docker commit -m="提交的描述信息" -a="作者" 容器id 目标镜像名：[TAG]
```

## 容器数据卷

### 使用数据卷

> 方式一：使用命令挂载

```shell
 docker run -it -v 主机目录:容器目录
 
 #测试 注意是绝对路径
 D:\Work>docker run -it -v D:/Work/test:/home centos
[root@4c8da4f66ac7 /]# cd home
[root@4c8da4f66ac7 home]# touch test.java
[root@4c8da4f66ac7 home]# ls
test.java
[root@4c8da4f66ac7 home]#

#正向
D:\Work\test>dir
 驱动器 D 中的卷是 Data
 卷的序列号是 E613-1C13
 D:\Work\test 的目录
20/09/2021  10:43    <DIR>          .
20/09/2021  10:43    <DIR>          ..
20/09/2021  10:43                 0 test.java
               1 个文件              0 字节
               2 个目录 247,397,146,624 可用字节
               
#反向  
[root@4c8da4f66ac7 home]# ls
test.java
[root@4c8da4f66ac7 home]# cat test.java
[root@4c8da4f66ac7 home]# cat test.java
hello-world[root@4c8da4f66ac7 home]#
```

### 具名挂载和匿名挂载

```shell
#匿名挂载
docker run -d -P --name nginx01 -v 容器内路径

#查看所有 volume 的情况
docker volume ls

#具名挂载
docker run -d -P --name xxxx -v 名字:容器内目录

#数据卷容器
--volumes-from 容器名
```

## DokerFile

dokcerfile 是用来构建docker镜像的构建文件！命令脚本

通过这个脚本，可以生成镜像

构建步骤：

1. 编写一个dockerfile文件
2. docker build 构建成为一个镜像
3. docker run运行镜像
4. docker push 发布镜像

### dockerfle基础知识

1. 每个保留关键字（指令）都必须是大写字母
2. 执行顺序从上到下
3. #表示注释
4. 每一个指令都会创建一个新的镜像层，并提交

dockerfile是面向开发的，我们以后发布项目，做镜像，就需要编写dockerfile文件，这个文件很简单

### dockerfile的指令 

```shell
FROM        # 基础镜像，一切从这里构建
MAINTAINER  # 镜像是谁写的， 姓名加邮箱
RUN         # 镜像构建是时需要运行的命令
ADD         # 步骤：tomcat镜像，这个tomcat压缩包！添加内容
WORKDIR     # 镜像的工作目录
VILUME      # 挂载的目录
EXPOSE		# 端口配置
CMD   		# 指定这个容器启动的时候，需要运行的命令，只有最后一个会生效，可被替代
ENTRYPOINT  # 指定这个容器启动的时候，需要运行的命令，可以追加命令
ONBUILD    	# 当构建一个被继承 DockerFile 这个时候就会运行 ONBUILD 的指令。触发指令
COPY        # 类似ADD，将我们文件拷贝在镜像中
ENV         # 构建的时候设置环境变量
```

![image-20210920135207760](.\images\image-20210920135207760.png)

### 实战测试

Docker Hub中 99%都是从基础镜像过来的 FROM scratch，然后配置需要的软件和配置的构建

Tomcat镜像

1. 准备镜像文件 tomcat压缩包，jdk的压缩包

   ![image-20210920145245366](.\images\image-20210920145245366.png)

2. 编写dockerfile文件，官方命名 `DockerFile`,build会自动寻找这个文件，就不需要 -f 指定了

   ```dockerfile
   FROM centos
   MAINTAINER xiangzi<2547989204@qq.com>
   
   COPY readme.txt /usr/local/readme.txt
   
   ADD jdk-8ull-linux-x64.tar.gz /usr/local/
   ADD apache-tomcat-9.0.22.tar.gz /usr/local/
   RUN yum -y install vim
   
   ENV MYPATH /usr/local
   WORKDIR $MYPATH
   
   ENV JAVA_HOME /usr/loca/jdk1.8.0_11
   ENV CLASSPATH $JAVA_HOME /lib/dt.jar;$$JAVA_HOME/lib/tools.jar
   ENV CATALINA_HOME /usr/local/apache-tomcat-9.0.22
   ENV CATALINA_BASH /usr/local/apache-tomcat-9.0.22
   ENV PATH $PATH:$JAVA_HOME/bin;$CATALINA_HOME/lib;$CATALINA_HOME/bin
   
   EXPOSE 8080
   ```

3. 构建镜像

   docker build

4. 发布镜像

   登陆后docker pull  注意加标签



### 小结

![image-20210920155039699](.\images\image-20210920155039699.png)
