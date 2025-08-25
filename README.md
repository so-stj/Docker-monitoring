# Docker Monitoring Stack

A comprehensive Docker-based monitoring solution using Prometheus, Grafana, Alertmanager, and Node Exporter for system monitoring and alerting.

## 🚀 Overview

This project provides a complete monitoring stack that includes:
- **Prometheus**: Time-series database for metrics collection
- **Grafana**: Visualization and dashboard platform
- **Alertmanager**: Alert routing and notification management
- **Node Exporter**: System metrics exporter for Prometheus

## 📋 Prerequisites

- Docker
- Docker Compose
- At least 2GB of available RAM
- Ports 3000, 9090, 9093, and 9100 available

## 🛠️ Installation & Setup

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd Docker-monitoring
   ```

2. **Configure Alertmanager (Optional)**
   Edit `alertmanager/config.yaml` to add your Slack webhook URL:
   ```yaml
   global:
     slack_api_url: "your-slack-webhook-url"
   ```

3. **Start the monitoring stack**
   ```bash
   docker-compose up -d
   ```

## 🌐 Access Points

Once the stack is running, you can access the following services:

| Service | URL | Default Credentials |
|---------|-----|-------------------|
| **Grafana** | http://localhost:3000 | admin/admin |
| **Prometheus** | http://localhost:9090 | None |
| **Alertmanager** | http://localhost:9093 | None |
| **Node Exporter** | http://localhost:9100 | None |

## 📊 Configuration

### Prometheus Configuration
- **Scrape Interval**: 15 seconds
- **Evaluation Interval**: 15 seconds
- **Targets**: Prometheus itself and Node Exporter

### Alert Rules
The system includes basic alert rules for:
- Instance down detection (5-minute threshold)
- Critical severity classification

### Alertmanager
- Configured for Slack notifications
- Route configuration for alert distribution
- Channel: #alerts

## 🔧 Customization

### Adding New Targets
To monitor additional services, edit `prometheus/prometheus.yaml`:
```yaml
scrape_configs:
  - job_name: 'your-service'
    static_configs:
      - targets:
        - 'your-service:port'
```

### Creating Custom Alerts
Add new alert rules in `prometheus/alert.rules.yaml`:
```yaml
- alert: your_alert_name
  expr: your_promql_expression
  for: 5m
  labels:
    severity: warning
  annotations:
    summary: "Alert summary"
    description: "Detailed description"
```

## 📈 Getting Started with Grafana

1. Access Grafana at http://localhost:3000
2. Login with admin/admin
3. Add Prometheus as a data source:
   - URL: `http://prometheus:9090`
   - Access: Server (default)
4. Import dashboards or create your own

## 🚨 Alerting

The system is configured to send alerts to Slack when:
- Instances go down for more than 5 minutes
- Other conditions defined in alert rules

To configure Slack notifications:
1. Create a Slack app and get the webhook URL
2. Update `alertmanager/config.yaml`
3. Restart the alertmanager service

## 🛑 Stopping the Stack

```bash
docker-compose down
```

To remove all data:
```bash
docker-compose down -v
```

## 📁 Project Structure

```
Docker-monitoring/
├── docker-compose.yml          # Main orchestration file
├── prometheus/
│   ├── prometheus.yaml         # Prometheus configuration
│   └── alert.rules.yaml        # Alert rules
├── alertmanager/
│   └── config.yaml             # Alertmanager configuration
└── README.md                   # This file
```

## 🔍 Troubleshooting

### Common Issues

1. **Port conflicts**: Ensure ports 3000, 9090, 9093, and 9100 are available
2. **Permission issues**: Run with appropriate Docker permissions
3. **Configuration errors**: Check YAML syntax in config files

### Logs
View logs for specific services:
```bash
docker-compose logs prometheus
docker-compose logs grafana
docker-compose logs alertmanager
```
