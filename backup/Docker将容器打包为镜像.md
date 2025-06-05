1. 使用```commit```命令将容器打包为镜像，例如将容器cc打包为ccnew:2.0
```docker
docker commit cc ccnew:2.0
```
2.将镜像打包为tar格式，例如将ccnew:2.0镜像打包为ccnew.tar
```
docker save -o ccnew.tar ccnew:2.0
```