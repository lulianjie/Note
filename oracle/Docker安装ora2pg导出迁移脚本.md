## 制作镜像或者拉取镜像</br>
```
docker pull georgmoser/ora2pg
```
## 创建存放配置文件以及数据文件路径
```
mkdir ora2pg
mkdir ora2pg/config
mkdir ora2pg/data
```
## 准备ora2pg.conf文件，主要是修改数据库连接属性和选择导出类型
>修改连接属性，注意，oracle18以后sid应该替换为service_name，如果不使用tns方式连接，ORACLE_HOME没用，可以注释
```
ORACLE_DSN	dbi:Oracle:host=mydb.mydom.fr;sid=SIDNAME;port=1521
ORACLE_USER	system
ORACLE_PWD	manager
```
>指定导出SCHEMA
```
SCHEMA      SCOTT
```
>修改导出文件选项，本例中导出表结构，数据
```
TYPE		TABLE,INSERT
```
原始conf文件可以从[GitHub georgmoser/ora2pg](https://github.com/Guy-Incognito/ora2pg)下载，其他参数使用也可以参考。
## 将准备好的conf文件放入ora2pg/config路径下
```
cp ora2pg.conf ora2pg/config/ora2pg.conf
```
## 创建容器
```
docker run  \
    --name ora2pg \
    -it \
    -v /home/lulianjie/ora2pg/config:/config \
    -v /home/lulianjie/ora2pg/data:/data \
    georgmoser/ora2pg
```
## 检查结果及后续处理
```
root@ubuntu:/home/lulianjie# cd ora2pg/data
root@ubuntu:/home/lulianjie/ora2pg/data# ls
INSERT_output.sql  TABLE_output.sql
```
由于本人docker不熟，存在以下情况：在容器创建后表结构和数据导出就完成了，但该容器创建后状态为Exited，即每次导出脚本都需要先删除容器再修改配置，创建容器。
>docker获取容器ID，删除容器方法
```
docker ps -a
docker rm xxx
```