## 制作镜像或者拉取镜像</br>
  [docker部署oracle--19c为例_hanwen112的博客-CSDN博客](https://blog.csdn.net/hanwen112/article/details/117197372)
```
docker pull registry.cn-hangzhou.aliyuncs.com/zhuyijun/oracle:19c
```

## 挂载目录授权
```
chmod o+w /opt/oracle/oradata
```

## 创建容器
```
docker run \
--name oracle19c \
-p 1521:1521 \
-p 5500:5500 \
-e ORACLE_SID=dev \
-e ORACLE_PDB=devpdb \
-e ORACLE_PWD=password \
-e INIT_SGA_SIZE=2000 \
-e INIT_PGA_SIZE=500 \
-v /opt/oracle/oradata:/opt/oracle/oradata \
7b5eb4597688
```
看到DATABASE IS READY TO USE!代表oracle已经启动成功，后续显示为oracle日志，可以直接关掉窗口，
连不上的话是连接方式有问题，按照一下连接数据库方式即可。

## 启动容器(如需)
```
docker start oracle19c
docker exec -it oracle19c bash
```

## 连接数据库(如需)
```
sqlplus sys/password@devpdb as sysdba
select con_id,dbid,NAME,OPEN_MODE from v$pdbs;
alter session set container=DEVPDB;
```
## 修改数据库参数(如需)
>修改UNDO保存时间为36000秒
```
ALTER SYSTEM SET undo_retention=36000 SCOPE=BOTH;
```
>修改数据文件大小
```
ALTER DATABASE DATAFILE '/opt/oracle/oradata/DEV/DEVPDB/system01.dbf'
AUTOEXTEND ON NEXT 100M MAXSIZE 1024M;
```
>增加数据文件
```
ALTER TABLESPACE app_data ADD DATAFILE
'/opt/oracle/oradata/DEV/DEVPDB/users02.dbf' SIZE 2048M;
```
## 查看容器日志(如需)
```
docker logs -ft 7e1beeb8a823
```
## 删除容器(如需)
```
sudo service docker stop
docker rm $(docker ps -aq)
sudo rm /var/lib/docker/network/files/local-kv.db
sudo systemctl restart docker
```