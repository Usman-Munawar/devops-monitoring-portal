# DevOps Monitoring Portal

A containerized observability stack built from scratch to learn core DevOps
monitoring concepts: metrics collection, the exporter/sidecar pattern,
dashboards, and (in progress) log aggregation.

## Stack

- **NGINX** — sample web server being monitored
- **nginx-prometheus-exporter** — sidecar that translates NGINX's native
  stats into Prometheus format
- **Prometheus** — scrapes and stores metrics on a schedule
- **Node Exporter** — exposes host-level metrics (CPU, memory)
- **Grafana** — dashboards querying Prometheus

## What it monitors

- **Application-level:** NGINX request rate (via stub_status + exporter)
- **Host-level:** CPU usage %, memory usage % (via Node Exporter) — these
  reflect the *entire host machine*, not any single container's usage.
  Per-container resource metrics (e.g. "how much CPU is just the NGINX
  container using") would require an additional exporter like cAdvisor,
  noted under Status below as a planned next step.

## Architecture

```
NGINX --> stub_status --> nginx-prometheus-exporter --> Prometheus --> Grafana
                                                              ^
                                                    Node Exporter
```

## Running it

All containers are connected via a shared Docker network (`monitoring-net`).
See individual config files (`nginx.conf`, `prometheus.yml`) for scrape
targets and routing.

## Status

Actively being built as a learning project — logging (Loki + Promtail),
alerting, and Docker Compose automation are planned next.
