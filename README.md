```markdown
# 📊 Infrastructure Monitoring Stack (Prometheus + Grafana)

[![Docker](https://img.shields.io/badge/Docker-24.0+-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://www.docker.com/)
[![Prometheus](https://img.shields.io/badge/Prometheus-v2.45+-E6522C?style=for-the-badge&logo=prometheus&logoColor=white)](https://prometheus.io/)
[![Grafana](https://img.shields.io/badge/Grafana-v10.0+-F46800?style=for-the-badge&logo=grafana&logoColor=white)](https://grafana.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](https://opensource.org/licenses/MIT)

A ready-to-use solution for local infrastructure monitoring using **Docker Compose**, **Prometheus**, and **Grafana**. This project is ideal for your portfolio, testing, and setting up a foundational observability stack.

---

## 🏗 System Architecture

``` 
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

1. **Prometheus** scrapes metrics and stores them in a time-series database.
2. **Grafana** connects to Prometheus as a data source and visualizes metrics via dashboards.
3. **Docker Volumes** ensure data persistence across container restarts.

---

## 📂 Repository Structure

```text
monitoring-stack/
├── docker-compose.yml       # Configuration for running services
├── README.md                # Project documentation
└── prometheus/
    └── prometheus.yml       # Prometheus scrape configuration

```

---

## 🚀 Quick Start & Installation

### Prerequisites

Make sure you have the following installed on your machine:

* Docker
* Docker Compose

### 1. Clone the Repository

```bash
git clone [https://github.com/YOUR_USERNAME/monitoring-stack.git](https://github.com/YOUR_USERNAME/monitoring-stack.git)
cd monitoring-stack

```

### 2. Start the Stack

Spin up the containers in detached mode:

```bash
docker compose up -d

```

### 3. Verify Status

Ensure both containers are up and running (`Up` status):

```bash
docker compose ps

```

---

## 🌐 Web Interfaces & Credentials

| Service | URL | Credentials | Description |
| --- | --- | --- | --- |
| **Prometheus** | http://localhost:9090 | *None required* | Metric collection & PromQL console |
| **Grafana** | http://localhost:3000 | `admin` / `admin` | Dashboards & visualization |

---

## 🛠 Grafana Configuration

### Step 1: Connect Data Source

1. Open Grafana (`http://localhost:3000`) and log in (`admin` / `admin`).
2. Go to **Connections** -> **Data Sources** -> **Add data source**.
3. Select **Prometheus**.
4. In the **Prometheus server URL** field, enter: `http://prometheus:9090`
5. Click **Save & test**.

### Step 2: Import Dashboard

1. Go to **Dashboards** -> **New** -> **Import**.
2. Enter the dashboard ID: `3662`.
3. Select your Prometheus data source and click **Import**.

---

## 🧹 Management & Useful Commands

* **View logs:** `docker compose logs -f`
* **Stop containers:** `docker compose down`
* **Full cleanup (remove containers, network, and data volumes):** `docker compose down -v`

---

## 📝 License

This project is licensed under the MIT License.



```
