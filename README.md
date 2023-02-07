# PostgreSQL Monitoring with Grafana, Prometheus, postgres-exporter

## Requirements
```
docker
docker-composer

```

### docker-compose.yaml

```
version: "3.9"
services:
  grafana:
    image: grafana/grafana
    ports:
      - 3000:3000
    volumes:
      - grafana_data:/var/lib/grafana:rw

  prometheus:
    image: prom/prometheus
    ports:
      - 9090:9090
    volumes:
      - ./prometheus.yaml:/etc/prometheus/prometheus.yaml:ro

  postgres-exporter:
    image: prometheuscommunity/postgres-exporter
    ports:
      - 9187:9187
    environment:
      DATA_SOURCE_NAME: "postgresql://db_monitor:db_password@db_host:5432/postgres?sslmode=disable"
    links:
      - prometheus

volumes:
  grafana_data:

```

### prometheus.yaml

```

global:
  scrape_interval: 15s
  evaluation_interval: 15s

scrape_configs:
  - job_name: prometheus
    static_configs:
      - targets: ["localhost:9090"]
  - job_name: postgres-exporter
    static_configs:
      - targets: ["postgres-exporter:9187"]

```

## Grafana

Add a Prometheus DataSource

```

Host: http://prometheus:9090

Set as default

```

### Grafana Dashboard Template

```

Import grafana dashboard template id: 9628

```

https://grafana.com/grafana/dashboards/9628


