pwd: 输出当前所在路径

ls：列出当前目录下的所有文件和目录

mkdir： make directory 创建一个新文件夹

cd : 进入某个目录

git 命令： 

1. git init :  初始化   初始化一个git仓库

   Initialized empty Git repository in C:/Users/ASUS/firstblood/.git/

   2 ·git status： 查看git仓库状态
   
   3.git add .
   
   4. git commit -m "标记信息"
   
   ​           git config --global user.email "you@example.com"
   ​          git config --global user.name "Your Name"
   
   
   
   5.git  remote add origin  github的地址
   
   ​       远端仓库重命名 git remote rename [old name]    [new name]
   
   ​    删除远端仓库   git remote remove 仓库名
   
   ```shell
   git pull --rebase origin master  //远程有readme.md，拉一下
   ```
   
   6. git push  -u origin master 

