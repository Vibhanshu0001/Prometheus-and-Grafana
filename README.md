
# 📊 Linux Node Monitoring with Prometheus + Grafana + Node Exporter (Dockerized)

This project sets up a **Linux monitoring stack** using **Docker Compose**. It includes:
- **Node Exporter** for exposing system metrics
- **Prometheus** to scrape metrics
- **Grafana** to visualize metrics

---

## 📦 Tech Stack Overview

| Component     | Port  | Description                          |
|---------------|-------|--------------------------------------|
| Prometheus    | 9090  | Metrics collector                    |
| Node Exporter | 9100  | System-level metrics exporter        |
| Grafana       | 3000  | Beautiful visual dashboard frontend  |

---

## 🛠️ Prerequisites

- Docker & Docker Compose installed
- Internet access to pull images
- Port 9100 **free** (or modify if in use)

---

## 🧾 Step-by-Step Setup

### 📁 1. Project Structure

```
node-exporter-docker/
├── docker-compose.yml
├── prometheus/
│   └── prometheus.yml
└── README.md
```

---

### 🧩 2. Create `docker-compose.yml`

```yaml
version: '3'

services:
  prometheus:
    image: prom/prometheus:latest
    container_name: prometheus
    volumes:
      - ./prometheus:/etc/prometheus
    command:
      - '--config.file=/etc/prometheus/prometheus.yml'
    ports:
      - "9090:9090"

  node-exporter:
    image: prom/node-exporter:latest
    container_name: node-exporter
    ports:
      - "9100:9100"

  grafana:
    image: grafana/grafana:latest
    container_name: grafana
    ports:
      - "3000:3000"
```

---

### 📝 3. Create `prometheus.yml` in `/prometheus/`

```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'node-exporter'
    static_configs:
      - targets: ['node-exporter:9100']
```

---

### ▶️ 4. Start All Containers

```bash
docker-compose up -d
```

To shut them down later:

```bash
docker-compose down
```

---

## 🌐 5. Access Services

- Prometheus: [http://localhost:9090](http://localhost:9090)
- Grafana: [http://localhost:3000](http://localhost:3000)  
  **Login:** `admin / admin`

---

## 📈 6. Import Node Exporter Dashboard in Grafana

1. Go to Grafana → Sidebar → `+ Create` → `Import`
2. Enter Dashboard ID: **1860**
3. Click **Load**
4. Select data source: `Prometheus`
5. Click **Import**

You’ll now see real-time CPU, memory, disk, and network metrics.

---

## 🧼 7. Stopping Services

```bash
docker-compose down
```

---

## ❗ Common Errors & Solutions

### 🔴 Port 9100 already in use

```
Bind for 0.0.0.0:9100 failed: port is already allocated
```

🔧 **Fix:**
- Find what's using it:
  ```powershell
  netstat -aon | findstr :9100
  ```
- Then stop the process or change the port in `docker-compose.yml`.

---

### ❌ `curl localhost:9100/metrics` shows `426 Upgrade Required`

🔧 **Fix:**
You downloaded a **Darwin (macOS)** version of Node Exporter on Windows. Use this instead:
- [Download Node Exporter for Windows](https://prometheus.io/download/#node_exporter)
- Choose `.windows-amd64.zip` version

---

### ❌ Can’t Find the "Import Dashboard" Option

🔧 **Fix:**
You are likely inside a panel editor.  
- Go back to **Home**
- Click **+ Create → Import** in the left sidebar, not from inside an existing dashboard

---

## 🧠 Tips

- You can view Prometheus targets via: [http://localhost:9090/targets](http://localhost:9090/targets)
- Grafana lets you save/export your custom dashboards
- To persist Grafana dashboards across restarts, add volumes (optional)

---

## 📄 License

MIT License — Free to use and modify.

---

