## docker-compose 安装 ElasticSearch 和 Kibana 8.8.1

### 一. 创建容器编排脚本
PS. 注释先不去掉
```yaml
version: "3.1"
# 服务配置
services:
  elasticsearch:
    container_name: elasticsearch-8.8.1
    image: docker.elastic.co/elasticsearch/elasticsearch:8.8.1
    # 用来给容器root权限（不安全）可移除
    privileged: true
    # 在linux里ulimit命令可以对shell生成的进程的资源进行限制
    ulimits:
      memlock:
        soft: -1
        hard: -1
    environment:
      - "ES_JAVA_OPTS=-Xms1024m -Xmx1024m"
      - "http.host=0.0.0.0"
      - "node.name=elastic01"
      - "cluster.name=cluster_elasticsearch"
      - "discovery.type=single-node"
    ports:
      - "9200:9200"
      - "9300:9300"
    volumes:
      # - ./elasticsearch/config:/usr/share/elasticsearch/config
      - ./elasticsearch/data:/usr/share/elasticsearch/data
      - ./elasticsearch/plugins:/usr/share/elasticsearch/plugins
    networks: 
      - elastic_net
  kibana:
    container_name: kibana-8.8.1
    image: docker.elastic.co/kibana/kibana:8.8.1
    ports:
      - "5601:5601"
    #volumes:
      #- ./kibana/config:/usr/share/kibana/config
    networks:
      - elastic_net
# 网络配置
networks:
  elastic_net:
    driver: bridge
```
### 二. 映射目录配置
#### 1. 创建挂载目录，同步配置
```bash
# 创建挂载目录
mkdir -p elasticsearch/data
mkdir -p elasticsearch/plugins
mkdir -p kibana

# 启动服务
docker-compose up -d

# 将默认配置拷贝到挂载目录
docker cp elasticsearch-8.8.1:/usr/share/elasticsearch/config ./elasticsearch/config
docker cp kibana-8.8.1:/usr/share/kibana/config ./kibana/config
```
#### 2. elasticsearch 配置
```yaml
# 集群节点名称
node.name: "elastic01"
# 设置集群名称为elasticsearch
cluster.name: "cluster_elasticsearch"
# 网络访问限制
network.host: 0.0.0.0
# 以单一节点模式启动
discovery.type: single-node


# 是否支持跨域
http.cors.enabled: true
# 表示支持所有域名
http.cors.allow-origin: "*"
# 内存交换的选项，官网建议为true
bootstrap.memory_lock: true


# 修改安全配置 关闭 证书校验
xpack.security.http.ssl:
  enabled: false
xpack.security.transport.ssl:
  enabled: false
```
#### 3. kibana 配置
```yaml
server.host: "0.0.0.0"
server.shutdownTimeout: "5s"
monitoring.ui.container.elasticsearch.enabled: true
# 国际化：中文
i18n.locale: zh-CN
```
#### 4. 重启服务加载配置
```bash
# 1. 首先放开 docker-compose.yml 中的注释
- ./elasticsearch/config:/usr/share/elasticsearch/config
- ./kibana/config:/usr/share/kibana/config

# 2. 更新容器
docker-compose up -d

```

### 三. 访问设置
#### 1. 重置密码
```bash
# 重置 elastic 用户密码
docker exec -it elasticsearch-8.8.1 /usr/share/elasticsearch/bin/elasticsearch-reset-password -uelastic

# 重置 kibana_system 用户密码
docker exec -it elasticsearch-8.8.1 /usr/share/elasticsearch/bin/elasticsearch-reset-password -ukibana_system

```
#### 2. elasticsearch 访问
```bash
http://localhost:9200
elastic
z76UglKCYr-WRCzs5Z*l（重置后的密码）
```
#### 3. kibana 访问
```bash
http://localhost:5601

# 1. 手动配置es地址
http://elasticsearch:9200

# 2. 获取验证码
docker exec -it kibana-8.8.1 /usr/share/kibana/bin/kibana-verification-code

# 3. 配置完成使用elastic用户登录即可
```