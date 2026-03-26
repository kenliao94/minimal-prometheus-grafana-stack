# Amazon MQ RabbitMQ Monitoring with Prometheus & Grafana

A Docker Compose stack that scrapes metrics from an Amazon MQ for RabbitMQ broker using Prometheus and visualizes them in Grafana with a pre-provisioned dashboard.

## Prerequisites

- [Docker](https://docs.docker.com/get-docker/) and Docker Compose
- A publicly accessible Amazon MQ for RabbitMQ broker with the Prometheus metrics endpoint enabled (port `16001`–`16003`)

## Configuration

Edit `prometheus.yml` and replace the placeholder values across all scrape jobs:

| Placeholder | Description |
|---|---|
| `<your-broker-id>` | Your Amazon MQ broker ID (e.g. `b-a9565a64-da39-4afc-9239-c43a9376b5ba`) |
| `<username>` | Broker username |
| `<password>` | Broker password |

The configuration scrapes the following metric families from each node:
- `/metrics` — cluster-wide metrics
- `/metrics/detailed` — connection churn, Raft (ra), queue, exchange, connection, channel, and exchange name metrics

The default scrape interval is `60s`.

## Usage

```bash
docker compose up -d
```

| Service | URL |
|---|---|
| Prometheus | [http://localhost:9090](http://localhost:9090) |
| Grafana | [http://localhost:3000](http://localhost:3000) |

Grafana credentials: `admin` / `admin`

The RabbitMQ Overview dashboard is automatically provisioned and available under **Dashboards** in Grafana. It includes panels for:
- Ready / unacknowledged messages
- Incoming & outgoing message rates
- Publisher confirms & unroutable messages
- Memory and disk space availability per node
- Queue, channel, and connection counts
- Message size distribution (requires RabbitMQ 4.1+)

## Stopping

```bash
docker compose down
```

## Files

| File | Purpose |
|---|---|
| `docker-compose.yml` | Defines Prometheus and Grafana services |
| `prometheus.yml` | Prometheus scrape configuration targeting the broker |
| `grafana-datasource.yml` | Auto-provisions Prometheus as a Grafana datasource |
| `grafana-dashboard.yml` | Auto-provisions the dashboard JSON from the file below |
| `10991_rev15.json` | RabbitMQ Overview Grafana dashboard (based on [Grafana dashboard 10991](https://grafana.com/grafana/dashboards/10991)) |
