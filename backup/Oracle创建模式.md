1. 以管理员身份登录SQL*Plus
```sqlplus / as sysdba ```
2. 查看当前所有PDB
```SHOW PDBS;```
3. 切换到目标PDB
```ALTER SESSION SET CONTAINER = ORCLPDB1;```
4. 确认当前操作容器：
```SHOW CON_NAME;```
5. 创建表空间
```CREATE TABLESPACE test_tablespace DATAFILE 'test_tablespace.dbf' SIZE 100M AUTOEXTEND ON NEXT 50M MAXSIZE UNLIMITED;```
6. 创建用户并关联表空间
```CREATE USER test_user IDENTIFIED BY test_pwd DEFAULT TABLESPACE test_tablespace TEMPORARY TABLESPACE temp;```
7. 为用户授权
```
GRANT CREATE SESSION, RESOURCE TO test_user;
ALTER USER test_user QUOTA UNLIMITED ON test_tablespace;
```
8. 验证与连接