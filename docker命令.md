---
docker 简单命令
---

[TOC]

###  docker 安装

安装需要的包

```
yum install -y yum-utils device-mapper-persistent-data lvm2
```

docker 删除旧版本

```
yum remove docker  docker-common docker-selinux docker-engine
```

添加阿里云的源

```
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

查看所有仓库中所有docker版本

```
yum list docker-ce --showduplicates | sort -r
```

安装需要的版本

```
yum install docker-ce-19.03

systemctl restart docker

systemctl enable docker
```



```
vim /etc/docker vim daemon.json
{
"registry-mirrors": ["http://373a6594.m.daocloud.io"],
"insecure-registries": ["自己本地库IP:5000"],
"exec-opts": ["native.cgroupdriver=systemd"],
"data-root":"/data/docker",
"bip": "10.188.0.1/16",
"log-driver": "json-file",
 "storage-driver": "overlay2",
"log-opts": { "max-size": "64m", "max-file": "3"}
}
```



### 下载镜像   docker pull  镜像文件名称：版本号   默认从官方Docker  Hub

```
  eg：  docker pull ubuntu:18.04      
           docker pull ubuntu ==>  docker pull ubuntu:laster

docker images  查看 docker 镜像

docker tag    修改 tag

 eg： docker tag name:tag  name:tag
```



###  **查看容器内网的ip地址等信息** 

```
 docker inspect containerid（容器ID） 

docker inspect （-f）（ {{xxxx}} ）  ubuntu:18.04    展示json格式信息

docker history  tag   查看历史信息

docker ps 查看当前运行状态的容器
     -a   查看所有
     
  查看对容器变更历史
docker diff   容器名

查看进程 docker top ID/NAME

查看容器 日志信息 
   docker logs  Name/ID
```



### docker 删除与创建

```
docker rmi  ID/name:tag   或  docker image rm ID/name:tag 删除镜像
  rmi -f  强制删除   

删除容器 
   docker  rm 
   
一次删除所有容器
docker rm -f $(docker ps -aq)

 容器创建一个新的镜像
docker [container] commit (-a  -m -p)
   eg： docker commit -m "test Docker Create Images" -a "an" ee50e767da4f ubuntu:1.0
   docker  images
```



### 保存与加载和导出与导出容器镜像文件   

```
保存  docker save  -o 新文件名.tar   镜像文件名:版本号

 eg: docker save -o ubuntu_18.04.tar ubuntu:18.04 

加载 docker load -i 新文件名称.tar
	docker load <  新文件.tar

导出容器
	docker  export ID/NAME   >  fileName.tar  路径

将指定的文件导入成为镜像文件
	docker import  fileName.tar    NAME:ID
     
将宿主机中的文件复制至容器中指定的路径  
	docker cp   文件名  容器名:绝对路径   
```



### 运行与启动容器

```
运行docker     docker run -itd -p  name:tag  /bin/bash

 -p   port:port   第一个式宿主机端口号 > 映射端口号   -it   i :input   t:tty  >伪终端

进入容器

docker exec -it 容器名 /bin/bash

创建容器  

​    docker create  -it ubuntu:版本号

启动  docker start    ID/NAME

暂停  docker pause   ID/NAME 取消暂停  docker unpase ID/NAME

停止   docker  stop    ID/NAME

以守护台运行（后台运行）  docker run  -d name:tag  /bin/bash

docker run -p 3306:3306 --name qmm-mysql -v ~/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=669988  -d mysql:5.6
//创建容器时，最后mysql:5.6表示mysql镜像的版本，可以写，表示指定该版本；如果不写也可以，docker会自动在本地检测有没有最新的，如果没有会自动去docker hub上去下载。

基于已有镜像创建以守护后台运行（后台运行 docker run -it  -d   --name NAME  name:tag  /bin/bash 

没有使用 -d   要保存容器退出  按  ctrl + Q + P

   --name   给容器起一个别名

进入指定容器  docker exec -it  ID/NAME  /bin/bash
```



### 创建数据卷  映射  持久化存贮

```
创建数据卷  映射  持久化存贮	 -v ：映射（将宿主机中指定文件/目录映射至硬盘 永久化保存

ll  /var/lib/docker/volumes

docker volume create -d local test

docker run -itd --name NAME  --mount  type=bind,source=宿主机绝对路径,destination=Container绝对路径  ContainerName:Version



默认权限为（rw）docker run -itd --name NAME -v  绝对路径（要映射文件路径）:绝对路径（映射到容器的文件路径）:权限（eg：ro）    ContainerNAME:版本号  /bin/bash     如果映射式文件，那这个文件在宿主机修改不会被同步

eg: docker run -itd --name test -v /home/an/text.txt:/temp.txt ubuntu:18.04  /bin/bash
```



```
docker-compase 命令

启动  docker-compase -f  启动的配置文件  -d  up

关闭  docker-compase down
```

### **构建自己私有仓库**

```
#下载Registry镜像并启动
docker pull registry

(这个就相当于启动一个仓库，别人访问需要在daemon文件中配置即可拉取你的镜像
docker run -d -v /edc/images/registry:/var/lib/registry -p 5000:5000 --restart=always --name xdp-registry registry

### 在客户端查看镜像仓库中的所有镜像
curl http://your-server-ip:5000/v2/_catalog
### 要下载的本地仓库镜像都有哪些tag（或版本）
curl http://your-server-ip:5000/v2/your-image-name/tags/list

vim  /etc/docker/daemon.json
{ 
    "insecure-registries" : [ "your-server-ip:5000" ] 
}
```

###### 为要上传的镜像打Tag

```
docker push your-registry-server-ip:5000/your-image-name:tagname
```

###### 下载镜像本地仓库镜像

```
docker pull your-server-ip:5000/your-image-name:tagname
```

