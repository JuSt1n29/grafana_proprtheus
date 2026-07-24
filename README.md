```markdown
# 📊 Infrastructure Monitoring Stack (Prometheus + Grafana)

[![Docker](https://img.shields.io/badge/Docker-24.0+-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://www.docker.com/)
[![Prometheus](https://img.shields.io/badge/Prometheus-v2.45+-E6522C?style=for-the-badge&logo=prometheus&logoColor=white)](https://prometheus.io/)
[![Grafana](https://img.shields.io/badge/Grafana-v10.0+-F46800?style=for-the-badge&logo=grafana&logoColor=white)](https://grafana.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](https://opensource.org/licenses/MIT)

A lightweight, production-ready local monitoring environment using **Docker Compose**, **Prometheus**, and **Grafana**. Designed for testing, infrastructure development, and as a foundation for enterprise monitoring stacks.

---

## 🏗 System Architecture

```text
+-------------------------------------------------------------------+
|                         DOCKER NETWORK                            |
|                                                                   |
|   +------------------+     Pull Metrics     +-----------------+   |
|   |                  | <------------------  |                 |   |
|   |    Prometheus    |                      |     Grafana     |   |
|   |   (Port: 9090)   |  ------------------> |   (Port: 3000)  |   |
|   |                  |    Query Metrics     |                 |   |
|   +--------+---------+                      +--------+--------+   |
|            |                                         |            |
+------------|-----------------------------------------|------------+
             |                                         |
             v                                         v
     [prometheus_data]                           [grafana_data]
     (Docker Volume)                             (Docker Volume)

```

1. **Prometheus** periodically scrapes metrics from defined targets (by default: itself) and stores time-series data.
2. **Grafana** connects to Prometheus as a primary data source to query, visualize, and alert on metrics.
3. **Docker Volumes** ensure long-term data persistence across container restarts.

---

## 📂 Project Structure

```text
monitoring-stack/
├── docker-compose.yml       # Orchestration file for Grafana & Prometheus
├── README.md                # Documentation
└── prometheus/
    └── prometheus.yml       # Scrape configs & scrape intervals

```

---

## ⚙️ Configuration Files

### 1. `docker-compose.yml`

```yaml
version: '3.8'

services:
  prometheus:
    image: prom/prometheus:latest
    container_name: prometheus
    restart: unless-stopped
    ports:
      - "9090:9090"
    volumes:
      - ./prometheus/prometheus.yml:/etc/prometheus/prometheus.yml
      - prometheus_data:/prometheus
    command:
      - '--config.file=/etc/prometheus/prometheus.yml'
      - '--storage.tsdb.path=/prometheus'
      - '--web.console.libraries=/usr/share/prometheus/console_libraries'
      - '--web.console.templates=/usr/share/prometheus/consoles'
    networks:
      - monitoring

  grafana:
    image: grafana/grafana:latest
    container_name: grafana
    restart: unless-stopped
    ports:
      - "3000:3000"
    environment:
      - GF_SECURITY_ADMIN_USER=admin
      - GF_SECURITY_ADMIN_PASSWORD=admin
    volumes:
      - grafana_data:/var/lib/grafana
    networks:
      - monitoring

networks:
  monitoring:
    driver: bridge

volumes:
  prometheus_data:
  grafana_data:

```

### 2. `prometheus/prometheus.yml`

```yaml
global:
  scrape_interval: 15s
  evaluation_interval: 15s

scrape_configs:
  - job_name: "prometheus"
    static_configs:
      - targets: ["localhost:9090"]

```

---

## 🚀 Quick Start & Installation

### Prerequisites

* [Docker Desktop](https://www.docker.com/products/docker-desktop/) installed (includes Docker Compose).

### 1. Clone & Navigate

```bash
git clone [https://github.com/YOUR_USERNAME/monitoring-stack.git](https://github.com/YOUR_USERNAME/monitoring-stack.git)
cd monitoring-stack

```

### 2. Launch Stack

Start services in detached mode:

```bash
docker compose up -d

```

### 3. Verify Container Status

Ensure both containers are running (`Up` status):

```bash
docker compose ps

```

---

## 🌐 Web Interfaces & Credentials

| Service | URL | Default Credentials | Description |
| --- | --- | --- | --- |
| **Prometheus** | `http://localhost:9090` | *None* | Metric storage & PromQL query engine |
| **Grafana** | `http://localhost:3000` | `admin` / `admin` | Dashboards & Visualizations |

---

## 🛠 Step-by-Step Grafana Configuration

### Step 1: Connect Prometheus Data Source

1. Log into Grafana at `http://localhost:3000`.
2. Go to **Connections** -> **Data Sources** -> Click **Add data source**.
3. Select **Prometheus**.
4. In the **Prometheus server URL** field, enter:
```text
http://prometheus:9090

```


*(Note: Use `http://prometheus:9090` instead of `localhost` due to Docker internal networking).*
5. Scroll down and click **Save & test**. You should see a green success message.

### Step 2: Import Prometheus Self-Monitoring Dashboard

1. Go to **Dashboards** -> Click **New** -> Select **Import**.
2. Enter Dashboard ID `3662` (Prometheus 2.0 Overview).
3. Select your **Prometheus** data source and click **Import**.

---

## 🔍 Sample PromQL Queries

You can test these queries directly in Prometheus (`http://localhost:9090/graph`) or inside Grafana panels:

* **Prometheus Health Status:**
```promql
up{job="prometheus"}

```


* **Memory Usage (Bytes):**
```promql
process_resident_memory_bytes{job="prometheus"}

```


* **HTTP Request Rate (per second):**
```promql
rate(prometheus_http_requests_total[5m])

```



---

## 🧹 Maintenance & Diagnostic Commands

* **View live logs:**
```bash
docker compose logs -f

```


* **Stop containers (keep data):**
```bash
docker compose down

```


* **Purge containers, networks, and persistent data:**
```bash
docker compose down -v

```



---

## 📝 License

This project is open-source under the [MIT License](https://www.google.com/search?q=LICENSE).

```

```
