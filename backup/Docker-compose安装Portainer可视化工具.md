记录使用docker-compose安装docker可视化管理工具portainer

### docker-compose.yml
```
version: '3'
services:
  portainer:
    image:portainer/portainer
    container_name: portainer
    restart: always
    volumes:
      - "/var/run/docker.sock:/var/run/docker.sock"
      - "./portainer/data:/data"
      - "./portainer/Portainer-CN:/public"   # 汉化
    environment:          # 设置环境变量,相当于docker run命令中的-e
      TZ: Asia/Shanghai
      LANG: en_US.UTF-8
    ports:               # 映射端口
      - "9000:9000"
```

### docker command
```
docker run -d -p 9000:9000 --restart=always --name portainer -v /var/run/docker.sock:/var/run/docker.sock -v /data/portainer:/data portainer/portainer
```
