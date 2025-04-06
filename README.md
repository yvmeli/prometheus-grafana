# Monitoring System Setup with Prometheus and Grafana

This repository contains configuration files to set up a basic monitoring system using Prometheus, Grafana, and Node Exporter with Docker Compose.
Yameli Martínez Taveras. No. 2023-1390

## Overview

The setup consists of three main components:
- **Prometheus**: Time series database for storing metrics
- **Grafana**: Visualization tool for metrics and analytics
- **Node Exporter**: Exports host system metrics to Prometheus

## Prerequisites

- Docker and Docker Compose installed on your system
- Basic knowledge of terminal/command line operations

## Setup Instructions

### 1. Create a `docker-compose.yml` file

Create a file named `docker-compose.yml` with the following content:

```yaml
version: '3.8'

services:
  prometheus:
    image: prom/prometheus:latest
    container_name: prometheus
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
    ports:
      - "9090:9090"

  grafana:
    image: grafana/grafana:latest
    container_name: grafana
    environment:
      - GF_SECURITY_ADMIN_USER=admin
      - GF_SECURITY_ADMIN_PASSWORD=admin
    ports:
      - "3000:3000"

  node-exporter:
    image: prom/node-exporter:latest
    container_name: node-exporter
    ports:
      - "9100:9100"
```

### 2. Create a Prometheus configuration file

Create a file named `prometheus.yml` in the same directory with the following content:

```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']

  - job_name: 'node-exporter'
    static_configs:
      - targets: ['node-exporter:9100']
```

### 3. Launch the containers

Run the following command in your terminal from the directory containing the files:

```bash
docker-compose up -d
```

### 4. Verify services are running

- Access Prometheus web interface: http://localhost:9090
- Access Grafana web interface: http://localhost:3000
  - Log in using the credentials:
    - Username: `admin`
    - Password: `admin`

### 5. Configure Grafana to display metrics

1. Log in to Grafana
2. Add a new data source:
   - Type: **Prometheus**
   - URL: `http://prometheus:9090`
   - Save and test the connection
3. Create a new dashboard:
   - Add a panel
   - Use queries such as `node_cpu_seconds_total` or `node_memory_Active_bytes`

## Result

You now have a basic monitoring system that displays metrics from the Node Exporter container and Prometheus in Grafana! 🎉

## Accessing the interfaces

- Prometheus: http://localhost:9090
- Grafana: http://localhost:3000
- Node Exporter metrics (raw): http://localhost:9100/metrics

## Troubleshooting

If you encounter issues:
- Check that all containers are running with `docker ps`
- Examine container logs with `docker logs <container_name>`
- Verify network connectivity between containers

## Next steps

- Create additional dashboards for specific metrics
- Add alerts for important thresholds
- Integrate additional exporters for monitoring specific services