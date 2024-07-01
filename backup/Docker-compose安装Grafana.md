记录使用docker-compose安装grafana

[参考文章](https://kalacloud.com/blog/grafana-with-prometheus-tutorial/)

### docker-compose.yml
```
version: '3.4'
services:
  prometheus:
    image: prom/prometheus
    container_name: prometheus
    hostname: prometheus
    ports:
      - 9090:9090
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
  prometheus-exporter:
    image: prom/node-exporter
    container_name: service
    hostname: service
    ports:
      - 9100:9100
  grafana:
    image: grafana/grafana
    container_name: grafana
    hostname: grafana
    ports:
      - 3000:3000
    volumes:
      - ./grafana.ini:/etc/grafana/grafana.ini
```

### prometheus.yml
```
global:
  scrape_interval: 10s
scrape_configs:
- job_name: node
  static_configs:
  - targets: ['service:9100'] # NOT localhost since we named the host of service in docker-compose file
```

### grafana.ini
```

```
