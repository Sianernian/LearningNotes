uname   系统信息  

df  -h  查看磁盘剩于空间

du  -sh  查看目录大小

du -h file   查看文件大小

stat filename      ls -l  -h  name



查看文件:

​         cat / more  fileName

​        head  -number fileName   显示前N行

​        tail   -number fileName   显示后N行

touch   创建文件

mkdir 创建文件夹

​        -p   如果父目录不存在 则自动创建。

mv 复制或移动

mv -r xxx  xxx  移动文件夹

ll -R  递归列出



" > "将指定文件内容清空之后 添加到文件中

">>"  追加新内容

  eg:  ll .bash > text.txt

|  管道符号

eg ：  ps -ef | grep ssh 



tar 

  		   .tar  是打包文件            tar -cvf fileName 打包文件 eg : tar -cvf .bash.tar   .bash*

   		  .tar.gz  是压缩文件       tar  -xvf  fileName  解压文件  eg:  tar -xvf bash.tar -C  /text 

zip

​         压缩   zip -r fileName.zip  file1 file2 ...       -r 递归压缩

​         解压  unzip -l  fileName.zip       -l 查看压缩包中的文件信息



find  

​       $ find pathName -name  fileName

​       				eg: sudo find / -name host

​                    pathName：从指定的目录下开始搜索 

​       $  find pathName  -size    +10M -type  f    

​                     eg:  sudo find / -size +100 -type -f

​                                     查找指定大小文件

grep     从指定路径 查找文件内容

​    eg:  	grep  -r  -n  "if"   ./text



vi/vim 编辑器

   1 编辑模式      i o a 进入  esc 推出

   2  普通模式   复制 yy 当前光标所在行    yny   复制n行     粘贴  p 粘贴复制的行至当前所在的下一行               删除  dd  dnd      恢复上一步  u

   3  末行模式  ： /搜索    ?   :i   :a   :o    esc 推出   :wq  保存退出  :!q 强制退出   :q  退出

​     eg:  :1,$S 替换单词  替换后单词   /g



显示文件 inode     ll  -i



ln -s   创建 软链接文件

软链接相当与快捷方式  

eg： ln -s filename   指定filename

硬链接相当于备份  inode  和 原文件 一样

eg： ln  filename  指定filename  



用户删除  userdel -r username 删除用户目录

创 建用户   

   sudo  useradd    -d  /home/username  -m  -s /bin/bash  username

用户修改    usermod

  sudo  usermod    -d  /home/username  -m  -s /bin/bash  username

​            -l NewUserName OldUserName

组创建

  sudo groupadd   -g  27000   GroupName

修改GID 必须根据指定组名 

 -g  newGid   name

修该组名必须指定组名

 -n Newname  Oldname

组删除 sudo groupdel  name



eg：sudo groupadd -g 2700 oinstall  sudo groupadd -g 2701 dba

 sudo useradd -d /home/grid  -m  -s  /bin/bash  -G {dba,oinstall}  gird

usermod     -a  -G oinstall    grid 





crontab  -e  编辑定时任务

eg: crontab  -e  3   * * * * * date >> /home/an/fileName;

 -l  查看

 -r  结束进程



tail -f  fileName  监控文件



监控进程 top 

 -d  30 每隔30s 更新





















