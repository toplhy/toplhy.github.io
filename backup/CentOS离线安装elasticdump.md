1.  安装包准备
```
1. 下载node
wget https://nodejs.org/dist/v14.15.0/node-v14.15.0-linux-x64.tar.xz

2. 解压
tar -xvf node-v14.15.0-linux-x64.tar.xz

3. 创建链接
ln -s /data/dump/node-v14.15.0-linux-x64/bin/node /usr/bin/node
ln -s /data/dump/node-v14.15.0-linux-x64/bin/npm /usr/bin/npm

4. 验证
node -v
npm -v

5. 安装elasticdump
npm install elasticdump -g

6. 获取npm缓存位置
npm config get cache
输出 /root/.npm

7. 压缩缓存
cd /root/
tar -cf npm-cache.tar .npm
```
2. 离线安装
将上面的到的两个包上传到无法连接外网的服务器
```
1. 安装node和npm
# 解压
tar -xvf node-v14.15.0-linux-x64.tar.xz
# 创建软链
ln -s /root/node-v14.15.0-linux-x64/bin/node /usr/bin/node
ln -s /root/node-v14.15.0-linux-x64/bin/npm /usr/bin/npm
# 验证
node -v
npm -v

2. 导入缓存安装elasticdump
# 解压缓存
tar -xvf npm-cache.tar
# 进入目录执行命令
cd /root/node-v14.15.0-linux-x64/lib/
npm install --cache /root/.npm --optional --cache-min 99999999999 --shrinkwrap false elasticdump
# 创建软链
ln -s /root/node-v14.15.0-linux-x64/lib/node_modules/elasticdump/bin/elasticdump /usr/bin/elasticdump
# 验证
elasticdump --help
```
3. 测试与验证
```
# 导出json
elasticdump --input=http://192.168.1.30:9200/project_evaluate --output=/root/test.json --type=data
# 导入json
elasticdump --input=/root/test.json --output=http://192.168.1.30:9200/project_evaluate --type=data
```