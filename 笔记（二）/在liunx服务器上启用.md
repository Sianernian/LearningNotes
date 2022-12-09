在liunx

   用 bee 工具打包成 可在liunx上运行的软件 用命令 bee -be GOOS=liunx



liunx 命令   解压全部文件 ： tar -zxvf   XXXXX

​      给予权限  sudo chmod 777 XXXX

​      mkdir  XXXXX 创建目录 

 mk  目标文件   目标目录

​	

kalilinux安装软件 

dpkg  -i +安装包路径(直接把安装包拉入命令行中)

下载go语言 /usr/local/src

先安装 wget  sudo apt-get wget

 wget -c https://storage.googleapis.com/golang/go1.8.3.linux-amd64.tar.gz  /usr/local/src

解压到  sudo tar -zxvf  XXXXXXXX   -C  /usr/local

在 home/Usename/   进行 

sudo mkdir -p goProject {src,bin,pkg}

sudo vim /etc/profile  在最后添加  

export GOROOT=/usr/local/go
export GOPATH=/home/an/goProject 
export GOBIN=$GOPATH/bin
export PATH=$PATH:$GOROOT/bin
export PATH=$PATH:$GOPATH/bin

执行命令立即生效

```shell
$ source /etc/profile
```



**一、配置ip**

查看ip   ifconfig 

配置DNS 

  vim /etc/resolv.conf 

配置临时ip  

 ifconfig eth0 192.168.44.53/24

​           eth 网卡 0 一个网卡 1俩个网卡   24是子网掩码 255.255.255.0

手动添加 route add default gw 192.168.44.53

ping  192.168.44.53

永久配置 vim /etc/network/interfaces

auto eth0  

iface eth0 inet static 配置eth0 使用静态地址

address 192.168.44.53 配置eth0的固定地址

netmask 255.255.255.0 子网掩码

geteway 192.168.1.1  网关



//配置完  重启网络服务

/etc/init.d/networking restart

或  systemctl restart network -service   -service 可加可不加

**二、配置sshd服务并使用xshell链接**

虚拟机安装 ssh服务   

sudo apt-get install openssh-server

查看   sudo ps -e | grep ssh

​        ps -  ef 查看 进程

​    |  进程 临时文件

   grep  ssh 筛选 ssh 进程



启动ssh  sudo /etc/init.d/ssh  start 

查看服务是否开启  sudo service ssh status



允许root用户登录sshd服务

vim/etc/ssh/sshd_config

修改32行和37行

PermitRootLogin yes

PubkeyAuthentication  yes

vim中显示行数 命令    ：set  number

启动服务

/etc/init.d/ssh restart