# Prometheus 学习笔记

## 部署方式
使用 Docker Compose 部署 4 个服务：
- redis（缓存）
- redis-exporter（暴露 Redis 监控指标）
- prometheus（采集和存储数据）
- grafana（可视化展示）

## docker-compose.yml

version: '3'
services:
  redis:
    image: redis:7.0-alpine
    container_name: redis
    ports:
      - "6379:6379"

  redis-exporter:
    image: oliver006/redis_exporter
    container_name: redis-exporter
    ports:
      - "9121:9121"
    environment:
      - REDIS_ADDR=redis://redis:6379

  prometheus:
    image: prom/prometheus
    container_name: prometheus
    ports:
      - "9090:9090"
    volumes:
      - /tmp/prometheus.yml:/etc/prometheus/prometheus.yml

  grafana:
    image: grafana/grafana
    container_name: grafana
    ports:
      - "3000:3000"
  

## prometheusp配置文件
global:
  scrape_interval: 15s
scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']
  - job_name: 'redis'
    static_configs:
      - targets: ['redis-exporter:9121']