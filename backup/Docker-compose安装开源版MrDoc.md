本文记录使用docker-compose安装MrDoc的过程，默认已具备docker和docker-compose环境。
### 1. 拉取MrDoc代码

```
git clone https://gitee.com/zmister/MrDoc.git
```
### 2. 编写docker-compose.yml文件

```
version: '3'
services:
mrdoc:
 container_name: mrdoc
 image: jonnyan404/mrdoc-alpine
 ports:
   - 10086:10086
 restart: always
 environment:
   TZ: Asia/Shanghai
 volumes:
   - ./MrDoc:/app/MrDoc # 前者是拉取的代码路径
```

### 3. 修改config.ini配置文件，修改为使用mysql数据库

```
[database]
#engine，指定数据库类型，接受sqlite、mysql、oracle、postgresql
engine = mysql
#name表示数据库的名称
name = mrdoc
#user表示数据库用户名
user = root
#password表示数据库用户密码
password = root123
#host表示数据库主机地址
host = xxx.xxx.xxx.xxx
#port表示数据库端口
port = 3306
```
### 4. 安装容器即可

```
docker-compose up
```
### 5. 访问地址


```
http://xxx.xxx.xxx.xxx:10086
```

### PS. 解决没有初始化超级管理员的问题
```
docker exec -it mrdoc  python manage.py createsuperuser
```
