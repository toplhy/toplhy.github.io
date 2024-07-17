记录使用docker-compose安装人大金仓kingbase数据库

### docker hub
[kingbase](https://hub.docker.com/r/godmeowicesun/kingbase)

### docker command
```
docker run -d -it --privileged=true -p 54321:54321 -v /opt/docker/kingbase-v8/opt:/opt --name kingbase-rv1 godmeowicesun/kingbase:es-v8-r3-rv1
```

### docker-compose
```
version: '3'
services:
  kingbase:
    container_name: kingbase-v8
    image: godmeowicesun/kingbase:es-v8-r3-rv1
    ports:
      - 54321:54321
    restart: always
    environment:
      TZ: Asia/Shanghai
      LANG: en_US.UTF-8
    volumes:
      - /LHY/data/kingbase-v8/opt:/opt
    privileged: true
```
