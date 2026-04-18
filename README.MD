# Green Observability Benchmark

A research project comparing the energy consumption and carbon footprint of popular observability stacks using the OpenTelemetry Astronomy Shop as a standard workload.

## Motivation
Observability stacks are always-on infrastructure components. Their energy consumption is rarely measured or compared. This project applies the Green Software Foundation's SCI (Software Carbon Intensity) methodology to quantify and compare the environmental impact of different observability backends.

## Backends compared
- Grafana LGTM stack (Loki, Grafana, Tempo, Mimir)
- VictoriaMetrics + Grafana
- Elastic stack (Elasticsearch + Kibana)
- Prometheus (baseline reference)

## Measurement tools
- Scaphandre (Docker/bare metal energy measurement)
- Kepler (Kubernetes energy measurement)
- Grafana Pyroscope (continuous profiling)
- GSF Impact Framework (SCI scoring)

## Workload
OpenTelemetry Astronomy Shop — a standard microservices demo application maintained by the CNCF.

## Status
🚧 Work in progress

## Author
Angel Olivares — Observability Engineer
