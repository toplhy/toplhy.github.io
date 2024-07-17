记录Oracle创建表空间及用户

### 导入数据
#### 1. 创建表空间
```
CREATE TABLESPACE gaoxiao DATAFILE '/opt/develop/oracle/oradata/gaoixao.ora' SIZE 500m;

```

#### 2. 创建用户
```
CREATE USER gaoxiao IDENTIFIED BY gaoxiao123456 DEFAULT TABLESPACE gaoxiao QUOTA 500m ON USERS;
```

#### 3. 用户授权
```
GRANT CONNECT,dba,RESOURCE TO gaoxiao; 4.导入dmp文件 imp gaoxiao/gaoxiao123456 full=y file=/home/oracle/gaoxiao.dmp ignore=y
```

### 导出指定用户的表到dmp文件
```
exp gaoxiao\_new/gaoxiao123456@dbsrv2 file=/home/oracle/gaoxiao\_new.dmp OWNER=gaoxiao\_new
```
