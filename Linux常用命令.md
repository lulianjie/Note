>删除当前目录下的所有文件及目录，并且是直接删除，无需逐一确认命令：
```
rm  -rf  要删除的文件名或目录
```
>查找文件
```
find / -iname "sshd_config"
```
>将一个文件夹下的所有内容复制到另一个文件夹下
```
cp -r /home/packageA/* /home/cp/packageB/
```
>清屏
```
ctrl + l
clear
```
>自动补全
```
Tab
```
>全部删除
```
Ctrl + u
```
>回到起始位置
```
Ctrl + a
```
>复制,粘贴
```
Ctrl + Insert
Shift + Insert
```
>查找软件包
```
apt search pldebugger
yum search pldebugger
```
>安装软件包
```
apt install postgresql-13-pldebugger
yum install pldebugger13.x86_64
```
>查看内存,IP
```
free
ip a
```