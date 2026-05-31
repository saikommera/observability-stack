# 📊 Observability Stack

[![OpenTelemetry](https://img.shields.io/badge/OpenTelemetry-Enabled-black?style=flat-square&logo=opentelemetry)](https://opentelemetry.io)
[![Prometheus](https://img.shields.io/badge/Prometheus-2.x-E6522C?style=flat-square&logo=prometheus)](https://prometheus.io)
[![Grafana](https://img.shields.io/badge/Grafana-10.x-F46800?style=flat-square&logo=grafana)](https://grafana.com)
[![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)](LICENSE)

Production observability stack with OpenTelemetry, Prometheus, Grafana, and Alertmanager. Includes pre-built SLO/SLI dashboards, DORA metrics, and AIOps-ready alert correlation.

## 🏗️ Architecture

```
Application → OpenTelemetry Collector → Prometheus → Grafana
                                     ↓
                               Alertmanager → PagerDuty / Slack
```

## 📐 Components

| Component | Purpose |
|---|---|
| OpenTelemetry Collector | Unified metrics, traces, logs pipeline |
| Prometheus | Metrics storage and alerting rules |
| Grafana | Dashboards — SLO, DORA, infrastructure |
| Alertmanager | Alert routing to PagerDuty / Slack |
| Loki | Log aggregation |

## 📊 Included Dashboards

- **SLO/SLI Dashboard** — Error budget burn rate, availability tracking
- **DORA Metrics** — Deployment frequency, lead time, MTTR, change failure rate
- **EKS Cluster** — Node CPU/memory, pod status, HPA activity
- **AWS Infrastructure** — EC2, RDS, ALB, Lambda metrics via CloudWatch exporter
- **CI/CD Pipeline** — Build duration, success rate, deployment frequency

## 🚀 Deploy with Helm

```bash
# Add Helm repos
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update

# Deploy full stack
helm upgrade --install monitoring prometheus-community/kube-prometheus-stack \
  --namespace monitoring \
  --create-namespace \
  --values helm/values-monitoring.yaml
```

## 📏 SLO Configuration Example

```yaml
# sloth SLO definition
version: "prometheus/v1"
service: "payment-api"
slos:
  - name: requests-availability
    objective: 99.9
    description: "99.9% of payment API requests succeed"
    sli:
      events:
        error_query: sum(rate(http_requests_total{job="payment-api",code=~"5.."}[5m]))
        total_query: sum(rate(http_requests_total{job="payment-api"}[5m]))
    alerting:
      page_alert:
        labels:
          severity: critical
      ticket_alert:
        labels:
          severity: warning
```

## 🧑‍💻 Author

**Sai Babji Kommera** — Senior DevOps / SRE Engineer
[LinkedIn](https://www.linkedin.com/in/sai-babji-kommera-a3b953396/) · saibabji1@gmail.com
