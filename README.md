# System Monitoring with Prometheus & Grafana

This project demonstrates how to monitor **system metrics** such as CPU, memory, disk, and network usage using **Prometheus**, **Node Exporter**, and **Grafana**.  

---

## Objectives
- Learn how Prometheus collects metrics from exporters.
- Visualize system metrics in Grafana dashboards.
- Gain hands-on experience with Docker Compose for monitoring tools.

---

## Architecture
```
+--------------------+ +------------------+
| Node Exporter | ---> | Prometheus | ---> Grafana Dashboard
| (Collects metrics) | | (Stores metrics) | (Visualization)
+--------------------+ +------------------+
```

- **Node Exporter** → exposes system metrics (CPU, RAM, Disk, Network).  
- **Prometheus** → scrapes metrics from Node Exporter and stores them.  
- **Grafana** → connects to Prometheus and visualizes data in dashboards. 

---

## Project Structure

project1_prometheus_grafana/
│── docker-compose.yml # Services definition
│── config/
│ └── prometheus.yml # Prometheus scrape config
│── README.md # Project documentation


---

## Getting Started

### 1. Clone Repository
```bash
git clone <your-repo-url>
cd repo_name

```
### 2. Start Services

```
docker-compose up -d

```

### 2. Access Services

- **Prometheus** → `http://localhost:9090`
- **Node Exporter** → `http://localhost:9100/metrics`
- **Grafana** → `http://localhost:3000`
    - **Username**: `admin`
    - **Password**: `admin`

---

## Create a Grafana Dashboard

1. Log in to Grafana → [http://localhost:3000](http://localhost:3000)  
2. Go to **Connections → Data Sources → Add data source**  
3. Choose **Prometheus**  
4. Set URL: `http://prometheus:9090`  
5. Click **Save & Test**  


### Create a New Dashboard
- Go to **Dashboards → New Dashboard → Add a Panel**  
- Query Prometheus with the examples below:  


#### Example Queries

**CPU Usage**
```promql
100 - (avg by (instance)(irate(node_cpu_seconds_total{mode="idle"}[5m])) * 100)
```

**Memory Available**
```promql
node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes * 100

```

**Disk Free Space (GB)**
```promql
node_filesystem_free_bytes{mountpoint="/"} / 1024 / 1024 / 1024

```

**Network Received**
```promql
rate(node_network_receive_bytes_total[5m])

```

---

## Expected Dashboard

**You should see:**

- CPU utilization over time
- Available vs. used memory
- Disk space usage in GB
- Network I/O rate
