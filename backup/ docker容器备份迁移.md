记录docker容器备份迁移的过程

### 1.创建容器快照
```
docker commit -p f4ef8866aeaf kingbase-bak
```

### 2.tar包备份
```
docker save -o ~/kingbase-bak.tar kingbase-bak
```

### 3.恢复容器
```
docker load -i ~/kingbase-bak.tar
```

### 4.查看并运行
```
docker images
docker run ...
```
